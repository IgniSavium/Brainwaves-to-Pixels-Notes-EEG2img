# MIND'S EYE: IMAGE RECOGNITION BY EEG VIA MULTIMODAL SIMILARITY-KEEPING CONTRASTIVE LEARNING

---

## 🔥INFO

**Blog**: 2025/07/31 by IgniSavium

- **Title**: MIND'S EYE: IMAGE RECOGNITION BY EEG VIA MULTIMODAL SIMILARITY-KEEPING CONTRASTIVE LEARNING
- **Authors**: Chi-Sheng Chen, Chun-Shu Wei (National Yang Ming Chiao Tung University)
- **Published**: June 2024
- **Comment**: arxiv
- **URL**: https://arxiv.org/abs/2406.16910

🥜**TLDR**: Regularize contrastive loss with **similarity-keeping term**

---

## Motivation

This paper aims to overcome the challenges of low signal-to-noise ratio and nonstationarity in EEG-based image decoding by proposing a self-supervised contrastive learning framework (MUSE) (similarity-keeping regularization term) with tailored EEG encoders, significantly outperforming prior methods in zero-shot image classification on large-scale datasets.

## Model

EEG encoder architecture: (🔥upstream spatial conv + Spatial-Then-Time/Time-Then-Spatial combination + Graph Attention)

<img src="MIND'S EYE-IMAGE RECOGNITION BY EEG VIA MULTIMODAL SIMILARITY-KEEPING CONTRASTIVE LEARNING.assets\image-20250731232009059.png" alt="image-20250731232009059" style="zoom:50%;" />

regularize the standard contrastive loss with Similarity Keeping term:

🤔*Innate character:* Similarity Keeping term keeps the distribution of similarities with negative samples

<img src="MIND'S EYE-IMAGE RECOGNITION BY EEG VIA MULTIMODAL SIMILARITY-KEEPING CONTRASTIVE LEARNING.assets\image-20250731232215133.png" alt="image-20250731232215133" style="zoom: 80%;" />

## Evaluation

SK term is helpful in Nerv-series architecture but not very helpful in MUSE-series.

<img src="MIND'S EYE-IMAGE RECOGNITION BY EEG VIA MULTIMODAL SIMILARITY-KEEPING CONTRASTIVE LEARNING.assets\image-20250731232321568.png" alt="image-20250731232321568" style="zoom:50%;" />

### Interpretability

<img src="MIND'S EYE-IMAGE RECOGNITION BY EEG VIA MULTIMODAL SIMILARITY-KEEPING CONTRASTIVE LEARNING.assets\image-20250731232448094.png" alt="image-20250731232448094" style="zoom:50%;" />

MUSE-SK model highlights the model’s enhanced focus on **temporal and occipital areas**🧐Seems not obvious...

<img src="MIND'S EYE-IMAGE RECOGNITION BY EEG VIA MULTIMODAL SIMILARITY-KEEPING CONTRASTIVE LEARNING.assets\image-20250731232517426.png" alt="image-20250731232517426" style="zoom:50%;" />

<img src="MIND'S EYE-IMAGE RECOGNITION BY EEG VIA MULTIMODAL SIMILARITY-KEEPING CONTRASTIVE LEARNING.assets\image-20250731232701263.png" alt="image-20250731232701263" style="zoom: 80%;" />