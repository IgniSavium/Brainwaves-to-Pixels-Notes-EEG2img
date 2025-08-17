

# DreamDiffusion: Generating High-Quality Images from Brain EEG Signals

---

## 🔥INFO

**Blog**: 2025/07/22 by IgniSavium

- **Title**: DreamDiffusion: Generating High-Quality Images from Brain EEG Signals
- **Authors**: Yunpeng Bai, Chun Yuan (THU Shenzhen Inernational)
- **Published**: June 2023
- **Comment**: ECCV
- **URL**: https://www.ecva.net/papers/eccv_2024/papers_ECCV/papers/04605.pdf

🥜**TLDR**: Highlight is a MAE (very large scale) self-supervised learning method for EEG Encoder.

---

## Motivation

This research aims to enable direct, high-quality image generation from **EEG signals**—overcoming the limitations of prior work that relied on costly, non-portable fMRI or poorly aligned EEG-text-image mappings—by leveraging pre-trained diffusion models and novel temporal EEG pretraining (masked signal modeling) to realize low-cost, accessible “thoughts-to-image” applications.

<img src="DreamDiffusion-Generating High-Quality Images from Brain EEG Signals.assets\image-20250722213913271.png" alt="image-20250722213913271" style="zoom: 50%;" />

## Model

### Architecture

<img src="DreamDiffusion-Generating High-Quality Images from Brain EEG Signals.assets\image-20250722213946774.png" alt="image-20250722213946774" style="zoom: 50%;" />

#### self-supervised learning for EEG modeling

> Given the high temporal resolution of EEG signals, we first divide them into **tokens** in the time domain, and randomly mask a certain percentage of tokens. Subsequently, these tokens will be **transformed into embeddings** by using a one dimensional convolutional layer. Then, we use an asymmetric architecture such as **MAE**  to predict the missing tokens based on contextual cues from the surrounding tokens.

reconstruction loss is only applied at masked token and unified across all EEG channels.

#### fine-tuning with Stable Diffusion Framework

only update the encoder(+projector) and the **cross_attn in U-Net** (denoise network in SD) with combined loss:

1. original image reconstruction loss
   ![image-20250722235238569](DreamDiffusion-Generating High-Quality Images from Brain EEG Signals.assets\image-20250722235238569.png)
2. align projected feature to CLIP description feature
   <img src="DreamDiffusion-Generating High-Quality Images from Brain EEG Signals.assets\image-20250722235203520.png" alt="image-20250722235203520" style="zoom: 80%;" />

>The **pre-trained Stable Diffusion mode**l is specifically trained for **text-to-image generation**; however, the EEG signal has its own characteristics, and its latent space is quite different from that of text and image. Therefore, directly fine-tuning the Stable Diffusion model using limited EEG-image paired data is unlikely to accurately align the EEG features with the text embeddings (🧐CLIP embeddings in SD).

### Data

>Due to variations in the equipment used for data acquisition, the channel counts of these EEG data samples differ significantly. To facilitate pre-training, we have uniformly padded all the data that has fewer channels to **128 channels** by **filling missing channels with replicated values**. 

## Evaluation

### Performance

<img src="DreamDiffusion-Generating High-Quality Images from Brain EEG Signals.assets\image-20250722235718366.png" alt="image-20250722235718366" style="zoom: 80%;" />

#### 1. FID (Fréchet Inception Distance)

**Purpose**: Measures the difference in feature space distribution between generated and real images.  
**Formula**:  
$$
\mathrm{FID} = \|\mu_r - \mu_g\|_2^2 + \mathrm{Tr}\left( \Sigma_r + \Sigma_g - 2(\Sigma_r \Sigma_g)^{1/2} \right)
$$

* $\mu_r, \Sigma_r$: Mean and covariance of real image features. 
* $\mu_g, \Sigma_g$: Mean and covariance of generated image features.  
* Features are typically extracted from a layer of the Inception-V3 network.  

#### 2. PSNR (Peak Signal-to-Noise Ratio)  

**Purpose**: Evaluates pixel-level error in image reconstruction/compression.  
**Formula**:  

$$
\mathrm{PSNR} = 10 \cdot \log_{10} \left( \frac{MAX_I^2}{\mathrm{MSE}} \right)
$$

* $MAX_I$: Maximum pixel value of the image (usually 255).  
* $\mathrm{MSE}$: Mean Squared Error, calculated as:  

$$
\mathrm{MSE} = \frac{1}{MN} \sum_{i=1}^{M} \sum_{j=1}^{N} \left[I(i,j) - K(i,j)\right]^2
$$

####  3. LPIPS (Learned Perceptual Image Patch Similarity)  

**Purpose**: Evaluates perceptual differences between images (aligned with human vision).  
**Formula** (based on deep features):  

$$
\mathrm{LPIPS}(x, x_0) = \sum_l \frac{1}{H_l W_l} \sum_{h,w} w_l \left\| \hat{y}^l_{h,w} - y^l_{h,w} \right\|_2^2
$$

* $y^l, \hat{y}^l$: Feature maps extracted from layer $l$ (normalized).  
* $w_l$: Learned weights for each layer.  
* Measures differences in deep feature representations, not pixel-level.

<img src="DreamDiffusion-Generating High-Quality Images from Brain EEG Signals.assets\image-20250723000012535.png" alt="image-20250723000012535" style="zoom:50%;" />

<img src="DreamDiffusion-Generating High-Quality Images from Brain EEG Signals.assets\image-20250723000038495.png" alt="image-20250723000038495" style="zoom:50%;" />

### Ablation

🧐Only tuning the pretrained encoder with CLIP text supervision yields obvious worse acc. compared with both tuning the encoder and the cross_attn in U-Net. Maybe it's because this framework doesn't map the EEG feature to the original image's latent state (z).

<img src="DreamDiffusion-Generating High-Quality Images from Brain EEG Signals.assets\image-20250723000342786.png" alt="image-20250723000342786" style="zoom:50%;" />

<img src="DreamDiffusion-Generating High-Quality Images from Brain EEG Signals.assets\image-20250723000404312.png" alt="image-20250723000404312" style="zoom:50%;" />

## 🧐Reflection

We can try implement CLIP alignment first, and then implement image reconstruction tuning later. i.e. divide this combined loss into two consequent process.