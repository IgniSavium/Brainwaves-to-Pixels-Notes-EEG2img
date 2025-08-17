# Visual Decoding and Reconstruction via EEG Embeddings with Guided Diffusion

---

## 🔥INFO

**Blog**: 2025/07/30 by IgniSavium

- **Title**: Visual Decoding and Reconstruction via EEG Embeddings with Guided Diffusion
- **Authors**: Dongyang Li et al. (Southern University of Science and Technology)
- **Published**: March 2024
- **Comment**: NIPS2024
- **URL**: https://arxiv.org/abs/2403.07721

🥜**TLDR**: diffusion prior + SDXL-Turbo + IP-Adapter

---

## Motivation

This paper aims to address the limited practicality and performance gap of EEG-based visual decoding compared to fMRI-based methods by introducing a novel zero-shot EEG-to-image reconstruction framework that achieves state-of-the-art results through a tailored encoder and **two-stage generation** (prior diffusion to convert $Z_{EEG}$ to $Z_{img}$) strategy, overcoming previous challenges like low signal quality and limited model design.

## Model

### Architecture

<img src="Visual Decoding and Reconstruction via EEG Embeddings with Guided Diffusion.assets\image-20250730175329128.png" alt="image-20250730175329128" style="zoom: 80%;" />

EEG Encoder (Channel-wise Transformer + Synthesis Module):

<img src="Visual Decoding and Reconstruction via EEG Embeddings with Guided Diffusion.assets\image-20250730175403618.png" alt="image-20250730175403618" style="zoom: 80%;" />

Image Reconstruction training uses weighted loss of these two:

1. contrastive loss between EEG-Encoder and CLIP Visual Encoder ($R^{1024}$ in ViT-14/Large)
2. MSE to encoder EEG into image's VAE latent



**Two-stage EEG-guided image generation pipeline** using **conditional diffusion models** (ref. DALLE-2):

1. **Stage I – EEG-Conditioned Diffusion**:
   EEG embeddings condition a diffusion model to generate **CLIP image embeddings** using a U-Net and **classifier-free guidance**.

   <img src="Visual Decoding and Reconstruction via EEG Embeddings with Guided Diffusion.assets\image-20250730181135486.png" alt="image-20250730181135486" style="zoom:67%;" />

2. **Stage II – Image Synthesis**:
   The generated CLIP embedding guides image generation using **pre-trained SDXL**(Stable Diffusion) and **IP-Adapter**
   (ImagePrompt: adapter weights for image-embedding-conditioned SD), with optional acceleration via **SDXL-Turbo**.

3. **Low-Level Pipeline**:
   To recover fine visual details (e.g., contours, posture), EEG featurevs are aligned with **VAE latents**. Only **latent-space loss** works reliably; full image-level losses are unstable and memory-intensive.

4. **Semantic-Level Pipeline**:
   EEG-derived image features generate captions via **GIT**, optionally guiding generation via text prompts. Due to potential semantic drift, this step is not always applied.

## Evaluation

### Overall Performance

<img src="Visual Decoding and Reconstruction via EEG Embeddings with Guided Diffusion.assets\image-20250730180142364.png" alt="image-20250730180142364" style="zoom:67%;" />

### Reconstruction Performance

<img src="Visual Decoding and Reconstruction via EEG Embeddings with Guided Diffusion.assets\image-20250730180456573.png" alt="image-20250730180456573" style="zoom: 80%;" />

<img src="Visual Decoding and Reconstruction via EEG Embeddings with Guided Diffusion.assets\image-20250730180552701.png" alt="image-20250730180552701" style="zoom: 80%;" />

### Temporal and Spatial Interpretability

<img src="Visual Decoding and Reconstruction via EEG Embeddings with Guided Diffusion.assets\image-20250730180802665.png" alt="image-20250730180802665" style="zoom: 80%;" />

<img src="Visual Decoding and Reconstruction via EEG Embeddings with Guided Diffusion.assets\image-20250730180822566.png" alt="image-20250730180822566" style="zoom: 80%;" />



### Retrieval and Classification Performance

<img src="Visual Decoding and Reconstruction via EEG Embeddings with Guided Diffusion.assets\image-20250730180242375.png" alt="image-20250730180242375" style="zoom: 50%;" />