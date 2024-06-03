# Language models


1. [2022 On the Use of BERT for Automated Essay Scoring: Joint Learning of Multi-Scale Essay Representation](#p1) - [link]()
2. []() - [link]()
3. []() - [link]()
 
---


# <a id='p1'>On the Use of BERT for Automated Essay Scoring: Joint Learning of Multi-Scale Essay Representation</a>

## Main idea
To improve BERT capability for scoring an essay, employ multiscale essay representation for BERT that can be jointly learned (ensemble). Also, employ multiple losses and transformer learning fro m out-of-domain essays.

## Details

AES consists of two modules:
- essay reresentation module: handles features' extraction
- scoring module: rates the essay based on the extracted features

__multiple losses:__
RME as loss function does not take into account the the distribution of the sample poplation and the sorting properties btw sampeles. As the result, employ a range of optimizations could bring diversity to the final overal score distribution and improves the effectiveness of ensemble learning.

To have diversity of essay scoring distribution, the authors combine two loss functions with MSE.

__Transfer learning from out-of-domain essays__: to address the issue of limited available data to train on, employ transfer learning from out-of-domain, inspired by [Song et al., 2020](https://aclanthology.org/2020.emnlp-main.546/).

Authors also employed [__R-Drop__](https://www.bing.com/search?q=R-drop%3A+Regularized+dropout+for+neural+networks.+I&cvid=669b4b95309741f3bc6c6e5318b892d9&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIGCAEQRRg60gEHMjE0ajBqMagCALACAA&FORM=ANAB01&PC=LCTS).

---

__Problem definition:__ Given essay $X$ ($|X|=n$), ouput single score $y$.

__Evaluation metric__: Quadratic weighted Kappa (QWK) (Cohen,
1968), which measures the agreement between the scoring results of two raters.

---

__Multi-scale essay repressentation__


Representation is driven from three levels:
- token scale
- segment scale
- document scale

__Pre-trained BERT for token- and document-scale input__
- BERT wordPEice tokenizer to tokenize the essay: $T_1$
- Construct new sequence $T_2$ from $T_1$:


$T_2 =\begin{cases} [CLS]+[t_1,\dots, t_L]+[SEP] && n>L\\ [CLS]+T_1+[SEP]   && n=L \\ [CLS]+T_1+[PAD]*(L-n)+[SEP] && n<L \end{cases}$

  Where $L$ is the max seq length supported by BERT (510).

- The final input representation are the sum of the token embeddings, the segmentation embeddings, and position embeddings.


__Document-scale__ $[CLS]$ output of the BERT model

__Token-scale__: apply max-pooling to all sequnce outputs and obtain the combined token-scale essay representation.

__Segment_scale__: for $K$ segments we would like to explore, given token $T_1$ obtain the segment-scale representation $k_i$ as follows:
1. define $n_p$ as the max number of tokens corresponding to eahc essay prompt $p$. Truncate token seq to $n_p$ tokens or pad it with $[PAD]$, based on the adjust the length to $n_p$.
2. divide the token seq into $m=\lceil \frac{n_p}{k_i} \rceil$ segments (each with length $k_i$) similar to (Mulyar et al., 2019).
3. Input each $m$ segments into BERT and get $[CLS]$ representataion.
4. EMploy LSTM to process the sequence of $m$ segment representaitons, followed by attention pooling  (Dong et al., 2017) on the LSTM output to get segment scale essay representation corresponding to scale $k_i$.

---
__Model architecture__

Document- and token-scale
- One BERT model to obtain document- and token- scale representation.
- Concatenate the outputs
- employ regression leyer to output score corresponding to document- and token- scale.

Segment-scale
- for each segment $k$ out of total $m$ segments:
  - apply BERT and get $m$ $[CLS]$
- employ LSTM followed by attention layer to get segment-scale representatiopn
- use dense regression layer to get score correesponding to segment scale $k$

__Final Score__:

Compute final score by adding all $S$ segment-scales, and the score from the document- and token- scale


---
__Loss function__

Loss function is the weighted sum of three loss functions:
1. MSE: measures the average value of square errors between predicted scores and labels
2. Similarity (SIM): measures whether two vectors are similar or dissimilar by using cosine function.

  This loss function mimics how teachers take into account the overall level distribution when scoring an essay.

3. Margin Ranking (MR): measures the ranking orders for each essay pair in the batch.

  Here, for a pair of essays, if the actual score of one essay be larger than the other essay, the predicted score should be larger. Otherwise, it penalize the loss function by the difference in the wrong direction.


---
---
---

__Experiment__

- 5-fold CV: 60/20/20
- Metric: QWK

---
__Ablation study__

- __Model with all three scales: BERT-DOC-TOK-SEG__
- Authors experienced combine their proposed method with __Longformer__ instead of BERT to handle the long sequence: __Longformer-DOC-TOK-SEG__.
- __Models with transfer learning from out of domain essays Tran-BERT-MS__

- <font color = 'orange'>Authors employ pre-training stage similar to (Song et al., 2020)</font>.
  - first scale all the labels from out-of-domain into 0-1 range
  - pre-train model with MSE loss
  - continue fine-tuning with in-domain essays
- __Model with multiple loss functions:  Tran-BERT-MS-ML__
- __Model with multiple loss and R-drop strategy: __Tran-BERT-MS-ML-R__

---
__Training__
- employ BERT `bert-base-uncased` as backbone with 12 transformer layers and 768 hidden size
- During training:  freeze all the layers in the BERT models except the last layer
- For the Longformer model employ `longformer-base-4096`
- learning rate of 6e-5, β1=0.9, β2=0.999, L2 weight decay of 0.005. The coefficient weight α in R-Drop is 9. We set the batch size to 32. We use dropout in the training stage and the drop rate is set to 0.1. We train all the models for 80 epochs, and select the best model according the performance on the develop set. We use a greedy search method to find the best combination of segment scales.
- Following (Cao et al., 2020), we perform the significance test for our models.

---
__Results__

__Tran-BERT-MS-ML-R__ ahieved the best result, followed by __BERT-DOC-TOK-SEG__.


## Related works

__Traditional methods__

Ususally employ regression or ranking systems with hand crafted features.These approach could achive a good performance for a small amount of data, because the feature extraction heavily relies of prior knowledge of linguistis.

- (Larkey, 1998; Rudner and Liang, 2002; Attali and Burstein, 2006; Yannakoudakis et al., 2011; Chen and He, 2013; Phandi et al., 2015; Cozma et al., 2018).

__Deep neural network methods__

They acieved comparable results. These methods use DL model to extract features,
-  (Taghipour and Ng, 2016; Dong and Zhang, 2016; Dong et al. 2017; Alikaniotis et al., 2016; Wang et al., 2018; Tay et al., 2018; Farag et al., 2018; Song et al., 2020; Ridley et al., 2021; Muangkammuen and Fukumoto, 2020; Mathias et al., 2020).


__Hybrid methdos__

Combining the traditional and neural networks models AES task could achieve higher performance, although thetraditional part still relies on hand crafted feature extraction:
- (Jin et al., 2018; Dasgupta et al., 2018; Uto et al., 2020).

__Pre-training method__

Only two approaches could show benefit from pre-trained models, and surpas other deep learning methods. While their improvement mainly comes from training optimization.
1. Cao et al. 2020: employ two self-supervised tasks and domain adversarial training
2. Yang et al. 2020: combine regresson and ranking to train their model.

---

__Other methods__

1. __EASE__: it employs regression with handcrafted features.
2. __CNN+RNN__: various method by  (Taghipour and Ng, 2016).
3. __Hierarchical LSTM-CNN-Attention__ by (Dong et al., 2017): builds a hierarchical sentence-document model, which uses CNN to encode sentences and LSTM to encode texts. The attention mechanism is used to automatically determine the relative weights of words and sentences in generating sentence representations and text representations respectively.
4. SKIPFLOW (Tay et al., 2018) proposes to use SKIPFLOW mechanism to model the relationships between snapshots of the hidden representations of an LSTM
5. __Dilated LSTM with Reinforcement Learning__ (Wang et al., 2018) proposes a method using a dilated LSTM network in a reinforcement learning framework. They attempt to directly optimize the model using the QWK metric which considers the rating schema.
6. __HA-LSTM+SST+DAT and BERT+SST+DAT__ (Cao et al., 2020) propose to use two self-supervised tasks and a domain adversarial training technique to optimize their training, which is the first work to use pre-trained language model to outperform LSTM based methods.
7. BERT

---

__Methods used in the comparison__
1.

## Usefull information

 __Longformer__ (Beltagy et al., 2020) is an extension for transformers with an attention mechanism that scales linearly with sequence length, making it easy to process long documents.
----
