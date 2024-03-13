# General idea

 Recovering the input prompt conditioned on the language model’s next-token probabilities. More specifically the proposed method predicts prompts by “unrolling” the distribution vector into a sequence that can be processed effectively by a pretrained encoder-decoder language model.


__Additional findings__
- Additionaly show feasibility of retrieving input via only access the output.
- inversion directly from the probability outputs of language models.
- Research into ‘model stealing’ aims to explore how models themselves can be replicated via API queries (Tramer et al., 2016). Stealing NLP models has been demonstrated in many domainsRecent work. Gudibande et al. (2023) suggests that this form of imitation may create models that replicate surface-level syntax but do not learn more complex attributes like knowledge or factuality. Different than these works, proposed method does not focus on reconstructing model weights from third-party model outputs, but finding a hidden prompt from outputs of a third-party model.
 

# Approach


## Other approaches
- Song & Raghunathan (2020); Li et al. (2023); Morris et al. (2023) investigate the privacy leakage of text embeddings from encoder models.
- Morris et al. (2023) succeeds in recovering full text sequences from their embeddings by conditioning the encoder of an encoder-decoder Transformer for inversion.




# Technical details
Inversion method was implemented and compared with the following 3 methods:
- Jailbreak
- LLM few-shot
- sample inverter
__Prompt reconstruction__

Probability of each of next possible token in the vocabulary :$v = p(x_{T +1} | x_1, ..., x_T ; θ), where\:  v ∈ ∆^{|V|-1}$

__Modeling options__ considering following levels of access
1. full distributional access,
2. partial distributional access (top-K or by request),
3. text output with user-defined logit bias, and
4. text output access only.

__Modeling sssumption__: no access to the model weights or activations.


 Learn a conditional language model that maps from next-token probabilities back to tokens: $p(x_{1:T} | v)$. And parameterize this distribution using a pretrained Transformer language model and train on samples from the unconditional model.  Employ cross-attention in an encoder-decoder Transformer to condition on the next-token vector (inherent from work by Dumoulin et al. (2018) on feature-level conditioning).

__If we have access to full ouput probability vector__:

Steps:
- reformat $v$ to be used as input to a LM encoder (T5).
  - option 1: project $v$ to $R^d$ and feed it as an input hidden vector. Does not work well: vocabulary size is too big, and passing through softmax results in large reduction in rank and information.
  - __Option 2 (used)__: 'unroll' the vector into a sequence of pseudo-embeddings $c_i ∈ R^d$, to be able condition transformer outputs on the full probability vector $v$:
    
  $$c_i = MLP_i(log(v_{d(i−1):di}))\: ∀ i ∈ \{1 . . . ⌈|V|/d⌉\}$$
  $$x^*= arg max_x Dec(x, Enc(c))$$, Where
  - $x^*$: predicted inversion
  - $d$ embedding dim
  - $v$ is padded with zeros at the final position

Config: $|V|$ = 32000 and $d$ = 768 for all experiments, leads to a fixed-length input sequence of 42 words.

__If we do not have access to full ouput probabily vector__.

- LM typically allow users to
  1. add a logit bias to adjust the distribution (a logit bias per API call).
  2. set the temperature parameter to zero to provide argmax of the distribution.
- Recover the probability of each token by finding its difference with the most likely word:  find the smallest logit bias to make that word most likely. The authors employ binary search to find logit bias for each word.
-  Reconstruct the full distribution $v = softmax(logits)$ by running described procedure for each word in the vocabulary.
- The number of required queries to determine the distribution is |V| times the number of bits required to represent the desired precision.

__Metrics__
- F1-score: token level
- BLUE-score: string overlap and string match
- exact match
-  cosine similarity: for semantic relatedness, between the text embeddings of the original and recovered text
- For cosine similarity, employ embeddings from the model text-embeddings-ada-002 available through the OpenAI API.


__Approaches in the experiment__
1. jailbreak:
  - aggregate jailbreak strings (human written that attempt to presuade a model to disvulg its prompt) from a variety of sources.
  - employ 20 jailbreak strings
  - For pre-trained Llama models, jailbreak strings are simply appended to the prompt.
  - in the report provide the mean performance of all prompts as well as an oracle figure.
2. few-shot: prompt GPT-4 with examples of the top-K tokens by probability from Llama-2 input predictions. These example input-output pairs are prepended to the top probabilities for the hidden input.
3. sample inverter: sample
outputs from Llama-2 7b chat and train a T5-base encoder-decoder to predict the input prompt from a given language model output
4. inversion

__Results__
- The failure of _sample inversion_ indicates that logit vector provides more usable information about the prompt than the argmax outputs alone.


__Defense against inversion__:

Solution: add noise to the language model output distribution; instead of providing a deterministic (argmax) output, from which an attacker could trivially reconstruct the output probabilities, language model providers, could instead sample
from the output distribution.

Three different LM sampling mechanisms
1. adjusting the softmax temperature during sampling,
2. adjusting the top-p parameter of nucleus sampling (Holtzman et al., 2020), and
3. adjusting the total number of tokens considered (top-K)
# Additional notes
__Prompt inversion vs input inversion:__
Prompt inversion is a form of model inversion, but we work with significantly more complex language models, where dimensionality of inputs is much higher than in the classifiers studied in prior model-inversion work. Instead of recovering information about the training data, we aim to recover the specific prompt given to the model and filter out information related to the training data.
