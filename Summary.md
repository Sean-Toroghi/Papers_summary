# <a name = 'up'>Paper deository - summary</a>

Objective: quick reference to the papers and a short summary of their findings. 

This depository is organized by the project topic or field topic.

## Table of contents

__Topic 1 - [Deep learning](#dl)__
- [](#dl_1)

__Topic 2 - [Machine learning](#ml)__
- [](#ml_1)

__Topic 3 - [Tabular data](#tbd)__
- [2019 Model: NODE -  Neural oblivious decision ensemble for deep learning on tabular data](#tbd_1)
- [2024 Model: GANDE - Gradient-based decision tree ensembles for tabular data](#tbd_2)
- [2024 Model: GANDALF - Gated adaptive network for deep automated learning of features for tabular data](#tnd_3)


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


__NODE architecture__

NODE cosists of several blocks of e





---
---

