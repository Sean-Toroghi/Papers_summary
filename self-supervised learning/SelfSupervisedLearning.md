# Self supervised learning

Papers:
1. [2022 Audio Self-supervised Learning: A Survey](#p1) - [link](https://github.com/Sean-Toroghi/Papers_summary/blob/f91498a949d4977219f5fd09ce517bf6f695e9d4/self-supervised%20learning/2022%20Audio%20Self-supervised%20Learning---%20A%20Survey.pdf)
2. [2022 data2vec: A General Framework for Self-supervised Learning in Speech, Vision and Language](#p2) - [link](https://arxiv.org/abs/2202.03555)
3. [2020 Bootstrap Your Own Latent A New Approach to Self-Supervised Learning](#p3) - [link]()
4. [2021 BYOL for Audio: Self-Supervised Learning for General-Purpose Audio Representation](#p4) - [link]()
5. [2022 BYOL for Audio: Exploring Pre-trainedGeneral-purpose Audio Representations](#p5) - [link]()
6. [2023 MASKED MODELING DUO: LEARNING REPRESENTATIONS BY ENCOURAGING BOTH NETWORKS TO MODEL THE INPUT](#p6) - [link]()
7. [2022 ATST: Audio Representation Learning with Teacher-Student Transformer](#p7) - [link]()
8. [2023 Self-supervised Audio Teacher-Student Transformer for Both Clip-level and Frame-level Tasks](#p8) - [link]()
9. [2023 FINE-TUNE THE PRETRAINED ATST MODEL FOR SOUND EVENT DETECTION](#p9) - [link]()
10. []() - [link]()
11. []() - [link]()
 
---


<a id='p1'> # Audio Self-supervised Learning: A Survey</a>


---
# <a id='p2'>data2vec: A General Framework for Self-supervised Learning in Speech, Vision and Language</a>



## Main idea

The core idea is to predict latent representations of the full input data based on a masked view of the input in a self distillation setup using a standard Transformer architecture.

data2vec predicts contextualized latent representations that contain information from the entire input, instead of modality specific target.


data2vec combines two ideas: 
1. masked prediction (Devlin et al.,2019; Baevski et al., 2020b; Bao et al., 2021)
2. learning of latent target representations (Grill et al., 2020; Caron et al., 2021)

  It generalizes the second idda by using multiple network layers as targets and shows that this approach works across several modalities.

Both teacher and student networks use transformer architecture:
- teacher mode: build representations of the full input data whose purpose is to serve as targets in the learning task 
- student: predict full data representation from masked version of the original input
- The weights of the teacher are an exponentially decaying average of the student (He et al., 2019; Grill et al., 2020; Caron et al., 2021).


## Details
__Model architecture__

- Step 1: encode the input

  a standard transformer is used as the base, with modality-specific encoding of the input data (ViT strategy for image, a multi-layer 1-D convolutional neural network that maps 16 kHz waveform to 50 Hz representations for speech,  and sub-word unit for language). 
- Step 2: masking

  Mask a part of embedded input (as a sequence of token). The MASKs are learnable. Then feed the maked embedded sequence to the Transformer networ. Computer vision: block-wise masking (Bao et al. (2021), speech : mask spans of latent speech representations (Baevski et al., 2020b) and for language: mask tokens (Devlin et al., 2019).
- Step 3: training

  Goal is to predict the original unmasked representation only for time-steps which are masked.

 teacher parametarization by EMA

 Training targets are constructed based on the output of the top K blocks of the teacher network for time-steps which are masked in student-mode. Also before average top K block, the output of each block are normalized. This normalization prevents model from collapsing. 


__Deal with issue of representation collaps__

A common failure mode when learning targets is for similar representations for targets to be produced resulting in a trivial task (Jing et al., 2021).

Address the issue:
- constrastive learnig: wav2vec 2.0
- not optimizing teacher parameter to minimize loss: BYOL
-  adds an explicit loss encouraging variance among different representations: VicReg (Bardes et al., 2021)


## Related works

__SSL in computer vision__
- one approach is building contrasting representations of augmentations of the same image, entirely different
images contrastive (Chen et al 2021).
- another approach is online clustering (Caron et al., 2020).
- Some models employ masked prediction (Bao et al., 2021; He et al., 2021; Xie et al., 2021), and make prediction at different stages: before pretraining, during pretraining, or after pretraining and dircetly predict pixels.
- Two famous models (BYOL and DINO) employ regress neural network representations of a momentum encoder (only on top layer).

__SSL in NLP__
- NLP approach: masked learning task, and a famous model that employs this approach is BERT. There are also several works on knowledge distillation.
- data2vec in comparison, does not predict discrete linguistic tokens such as words, sub-words or bytes but rather a continuous and contextualized representation. Also, unlike BERT, targets are contextualized, taking context information into account.
