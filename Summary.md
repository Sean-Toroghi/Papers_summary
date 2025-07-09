# <a name = 'up'>Paper deository - summary</a>

Objective: quick reference to the papers and a short summary of their findings. 

This depository is organized by the project topic or field topic.

## Table of contents

__Topic 1 - [Deep learning](#dl)__
- [](#dl_1)

__Topic 2 - [Machine learning](#ml)__
- [](#ml_1)

__Topic 3 - [Tabular data](#tbd)__
- [2019 Model: NODE - Neural oblivious decision ensemble for deep learning on tabular data](#tbd_1)
- [2023 Model: GradTree - ](#tbd_2)
- [2024 Model: GANDE - Gradient-based decision tree ensembles for tabular data](#tbd_3)
- [2024 Model: GANDALF - Gated adaptive network for deep automated learning of features for tabular data](#tbd_4)
- [2022 - Why do tree-based models still outperform deep learning on tabular data](#tbd_5)
- [2020 - Model evaluation, model selection, and algorithm selection in machine learning](#tbd_6)


__Topic 10 - [Recommendation systems](#rec)__
- [](#rec_1)

---

# <a name = 'tbd'>[Tabular data](#up)</a>
Tabular heterogeneous data is a frequent hurdle in data science. For these datasets, ensemble methods built from multiple shallow models, like gradient-boosted decision trees, are typically the most effective. This section provides an overview of some current state-of-the-art (SOTA) approaches.


## <a name = 'tbd_1'>[Paper 1 - NDOE - Neural oblivious decision ensemble for deep learning on tabular data](#up)</a>
NODE architecture generalizes ensembles of oblivious decision trees, but benefits from both end-to-end gradient-based optimization and the power of multi-layer hierarchical representation learning. 

NODE took the idea of oblivious decision trees from CatBoost, in which:
- Every node at the same depth uses the same feature and threshold to split.
- The tree structure is fixed and non-differentiable, meaning you can't train it using gradient descent like neural networks.

NODE generalized that idea by making the following two differentiable:
- choice of splitting feature and
- routing through the trees
It results in a trainable end-to-end model that could use backpropagation, similar to deep neural networks.

Oblivious Decision Trees (ODTs) is a regular tree of depth d that is constrained to use the same splitting feature and splitting threshold in all internal nodes of the same depth. This constraint essentially allows representing an ODT as a table with 2d entries, corresponding to all possible combinations of d splits.



__NODE architecture__  consists of densely connected NODE layers. Each layer contains several trees whose outputs are concatenated and serve as input for the subsequent layer. The final prediction is obtained by averaging the outputs of all trees from all the layers.

__Entmax__ is a family of transformations that generalize softmax and sparsemax. It maps real-valued inputs into a probability distribution—just like softmax—but with controllable sparsity.
- Softmax: Always outputs dense probabilities (all values > 0).
- Sparsemax: Can output exact zeros, leading to sparse distributions.
- Entmax: Interpolates between the two, controlled by a parameter α (alpha).
  - $\alpha$ = 1 → softmax
  - $\alpha$ = 2 → sparsemax
  - $\alpha$ = 1.5 → commonly used in NODE for a balance between smoothness and sparsity

[__Implementation__]()

---
## <a name = 'tbd_2'>[Paper 2 - GradTree](#up)</a>

In GradTree, DTs are formulated as arithmetic functions based on addition and multiplication to facilitate gradient-based learning.
---

## <a name = 'tbd_3'>[Paper 3 - GANDE: Gradient-based decision tree ensembles for tabular data](#up)</a>

This paper extend the GradTree (Marton et al 2023) from individual trees to end-to-end gradient-based ensemble model. Authors also propose _softsign_ as a differentiable split function in their model.

---

## <a name = 'tbd_4'>[Paper 4 - GANDALF - Gated adaptive network for deep automated learning of features for tabular dataa](#up)</a>

---

## <a name = 'tbd_5'>[Paper 5 - Why do tree-based models still outperform deep learning on tabular data](#up)</a>



---


