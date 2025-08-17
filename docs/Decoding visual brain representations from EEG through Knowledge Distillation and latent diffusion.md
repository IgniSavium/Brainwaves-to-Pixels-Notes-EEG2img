# Decoding visual brain representations from electroencephalography through Knowledge Distillation and latent diffusion models

---

## 🔥INFO

**Blog**: 2025/07/23 by IgniSavium

- **Title**: Decoding visual brain representations from electroencephalography through
  Knowledge Distillation and latent diffusion models
- **Authors**: Matteo Ferrante, Nicola Toschi (University of Rome Tor Vergata)
- **Published**: September 2023
- **Comment**: arxiv
- **URL**: https://arxiv.org/abs/2309.07149

🥜**TLDR**: Only train the Conv Encoder which maps EEG-signal spectrogram (derived from STFT) to ImageNet classification scores with **CLIP knowledge distillation**], and then use a category-related text template to guide Stable Diffusion generation.

---

## Motivation

This research aims to enhance EEG-based brain decoding by developing an individualized, real-time image classification and reconstruction pipeline, addressing the limitations of prior studies that relied on multisubject models and struggled with low-fidelity visual reconstructions.

## Model

### Architecture

<img src="./Decoding visual brain representations from EEG through Knowledge Distillation and latent diffusion.assets/image-20250723111118234.png" alt="image-20250723111118234" style="zoom: 67%;" />

1. train a EEG-based **classifier** using CLIP distillation:

   <img src="./Decoding visual brain representations from EEG through Knowledge Distillation and latent diffusion.assets/image-20250723111234750.png" alt="image-20250723111234750"  />

   <img src="./Decoding visual brain representations from EEG through Knowledge Distillation and latent diffusion.assets/image-20250723111300710.png" alt="image-20250723111300710" style="zoom: 80%;" />

1. Use text prompt such as "an image of a \<predicted class\>" to guide SD generation with random white noise $z_T$.

   

   

## Evaluation

<img src="./Decoding visual brain representations from EEG through Knowledge Distillation and latent diffusion.assets/image-20250723111550680.png" alt="image-20250723111550680" style="zoom: 80%;" />

### Performance

Compared with other typical **CLASSIFIER** architectures.

🤔Last two rows in the table below show the benefit of knowledge distillation, but the acc. gain is VERY LIKELY **exaggerated**.

🤔Using typical LSTM to model EEG time series directly can also have not bad performance.

<img src="./Decoding visual brain representations from EEG through Knowledge Distillation and latent diffusion.assets/image-20250723111606327.png" alt="image-20250723111606327" style="zoom:67%;" />

This bar chart below seems more plausible.

<img src="./Decoding visual brain representations from EEG through Knowledge Distillation and latent diffusion.assets/image-20250723111849268.png" alt="image-20250723111849268" style="zoom: 67%;" />

## 🤔Reflections

Class-wise encoding is obviously insufficient for detailed semantic reconstruction.

Other works have mapped the EEG-signal to conditional text features and visual latent features in the SD.