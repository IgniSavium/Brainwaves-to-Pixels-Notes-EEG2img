# Learning Robust Deep Visual Representations from EEG Brain Recordings

---

## 🔥INFO

**Blog**: 2025/07/28 by IgniSavium

- **Title**: Learning Robust Deep Visual Representations from EEG Brain Recordings
- **Authors**: Prajwal Singh, Shanmuganathan Raman (IIT Gandhinagar, India)
- **Published**: October 2023
- **Comment**: WACV
- **URL**: https://openaccess.thecvf.com/content/WACV2024/papers/Singh_Learning_Robust_Deep_Visual_Representations_From_EEG_Brain_Recordings_WACV_2024_paper.pdf

🥜**TLDR**: Encoder Training: additional CLIP distillation

---

## Motivation

This paper aims to overcome the limitations of low-quality image synthesis and heavy reliance on label supervision in EEG-based image generation by proposing two-stage framework (EEGStyleGAN-ADA) that significantly improves synthesis quality and generalizability across datasets compared to prior state-of-the-art methods.

## Model

### Architecture

<img src="Learning Robust Deep Visual Representations from EEG Brain Recordings.assets\image-20250729173333788.png" alt="image-20250729173333788" style="zoom:67%;" />

1. Train the EEG feature encoder by triplets loss.

<img src="Learning Robust Deep Visual Representations from EEG Brain Recordings.assets\image-20250729172425488.png" alt="image-20250729172425488" style="zoom: 80%;" />

2. Fine-tune the EEG feature encoder by standard CLIP loss.

   <img src="Learning Robust Deep Visual Representations from EEG Brain Recordings.assets\image-20250729173432920.png" alt="image-20250729173432920" style="zoom: 80%;" />

   

3. Use the CLIP-aligned EEG feature as StyleGAN-ADA input.

## Evaluation

### Pre-training Effectiveness

**triplet loss vs. supervised classification loss *linear separability***

<img src="Learning Robust Deep Visual Representations from EEG Brain Recordings.assets\image-20250729174606924.png" alt="image-20250729174606924" style="zoom: 80%;" />

<img src="Learning Robust Deep Visual Representations from EEG Brain Recordings.assets\image-20250729174622100.png" alt="image-20250729174622100" style="zoom: 80%;" />

<img src="Learning Robust Deep Visual Representations from EEG Brain Recordings.assets\image-20250729175259719.png" alt="image-20250729175259719" style="zoom: 80%;" />

![image-20250729174043802](Learning Robust Deep Visual Representations from EEG Brain Recordings.assets\image-20250729174043802.png)

### Image Synthesis Performance

<img src="Learning Robust Deep Visual Representations from EEG Brain Recordings.assets\image-20250729175654015.png" alt="image-20250729175654015" style="zoom: 80%;" />

### Data simulation

<img src="Learning Robust Deep Visual Representations from EEG Brain Recordings.assets\image-20250729180232550.png" alt="image-20250729180232550" style="zoom: 80%;" />

### Image Retrieval Validation

![image-20250729180429112](Learning Robust Deep Visual Representations from EEG Brain Recordings.assets\image-20250729180429112.png)
