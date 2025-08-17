# A New Framework Combining Diffusion Models and the Convolution Classifier for Generating Images from EEG Signals

---

## 🔥INFO

**Blog**: 2025/07/31 by IgniSavium

- **Title**: A New Framework Combining Diffusion Models and the Convolution Classifier for Generating Images from EEG Signals
- **Authors**: Guangyu Yang and Jinguo Liu (CAS)
- **Published**: April 2024
- **Comment**: Brain Science
- **URL**: http://dx.doi.org/10.3390/brainsci14050478

🥜**TLDR**: EEG Conv-Encoder + PEFT SD.

---

## Motivation

This paper aims to address the challenge of generating high-quality images from complex EEG signals—characterized by low spatial resolution and noise—by proposing a novel EEG-ConDiffusion framework that overcomes the limitations of prior EEG-to-image methods such as LSTM, GANs, and VAEs through effective CNN-based feature extraction and fine-tuning of a pretrained stable diffusion model.

## Model

### Architecture

<img src="A New Framework Combining Diffusion and Convolution Classifier for Generating Images from EEG Signals.assets\image-20250731175353829.png" alt="image-20250731175353829" style="zoom: 50%;" />

1. Train EEG encoder by classification task.
2. Train SD partially by reconstruction

> During the model fine-tuning process, we fix the remainder of the SD model and optimize the **CLIP text encoder $τ_θ(y)$, cross-attention head, and projection head** at the same time...... EEG feature vectors that have undergone feature extraction and position encoding are used instead of text input to the pretrained CLIP Embedder. The CLIP Embedder was fine-tuned to help align the EEG feature vector space with the image feature space. Fine-tuning the cross-attention head is essential for bridging the pretraining conditional space and the latent space of the EEG features.

<img src="A New Framework Combining Diffusion and Convolution Classifier for Generating Images from EEG Signals.assets\image-20250731175843091.png" alt="image-20250731175843091" style="zoom: 67%;" />

🧐CLIP text embedder is very possibly NOT efficient here (big space gap between EEG Feature and Pure Text).

EEG encoder utilizes temporal + spatial convolutions.

<img src="A New Framework Combining Diffusion and Convolution Classifier for Generating Images from EEG Signals.assets\image-20250731175915967.png" alt="image-20250731175915967" style="zoom:50%;" />

## Evaluation

### frequency band influences

1. 1-70Hz performs much better than 5-95Hz
2. Subject Variance is very obvious for performance

<img src="A New Framework Combining Diffusion and Convolution Classifier for Generating Images from EEG Signals.assets\image-20250731180144112.png" alt="image-20250731180144112" style="zoom: 67%;" />

<img src="A New Framework Combining Diffusion and Convolution Classifier for Generating Images from EEG Signals.assets\image-20250731180220634.png" alt="image-20250731180220634" style="zoom: 67%;" />

### inter-subject generalizability

Use S1 weight as anchor.

<img src="A New Framework Combining Diffusion and Convolution Classifier for Generating Images from EEG Signals.assets\image-20250731180430037.png" alt="image-20250731180430037" style="zoom: 67%;" />

🧐Inception Score purely emphasizes the entropy of prediction scores, not very enough to show accuracy.