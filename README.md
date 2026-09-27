# SAR Image Colorization using Lightweight GANs

> **Hackathon / Research Prototype — Raw Implementation**

A deep learning prototype for **SAR-to-optical image translation**, developed as a hackathon project. The project explores whether a lightweight Generative Adversarial Network (GAN) can learn to transform grayscale Synthetic Aperture Radar (SAR) imagery into RGB images resembling corresponding optical imagery.

The current repository preserves the **original notebook-based implementation developed during the hackathon**. It is intentionally kept close to the team's original implementation rather than being presented as a production-ready system.

---

## Overview

Synthetic Aperture Radar (SAR) imagery is valuable for remote sensing because it can capture information under challenging conditions such as cloud cover and low-light environments. However, SAR images have a visual representation that differs significantly from conventional optical satellite imagery.

This project explores a computer vision approach to bridge that gap:

```text
             SAR Image
                 │
                 ▼
        Image Preprocessing
       Resize + Normalize
                 │
                 ▼
        ┌─────────────────┐
        │    Generator    │
        │                 │
        │ Lightweight CNN │
        │ + Residual      │
        │   Blocks        │
        │ + Upsampling    │
        └────────┬────────┘
                 │
                 ▼
        Generated RGB Image
                 │
        ┌────────┴────────┐
        │                 │
        ▼                 ▼
 Discriminator        L1 Loss
        │                 │
        └────────┬────────┘
                 ▼
           GAN Training
```

The objective is not simply to colorize an image using a fixed color mapping, but to investigate whether a learned generator can produce visually meaningful RGB representations from SAR inputs.

---

## Current Project Status

**Status: Hackathon prototype / research implementation**

The current version is primarily implemented as a **Jupyter/Google Colab notebook**.

It demonstrates the complete experimental workflow:

* Dataset loading
* SAR and optical image preprocessing
* Train/test splitting
* PyTorch Dataset and DataLoader setup
* Generator architecture
* Discriminator architecture
* GAN training
* L1 reconstruction loss
* Adversarial loss
* GPU and mixed-precision support
* Model checkpointing
* Inference
* Visual comparison of generated and real optical images

The repository is **not currently intended to be presented as a production deployment or a fully packaged ML system**.

The code and notebook reflect the implementation produced during the hackathon and are being preserved as the original project version.

---

## Key Technical Components

### 1. SAR Image Processing

The input SAR images are loaded from `.tif` files and processed before being passed to the model.

The preprocessing pipeline includes:

* Image loading
* Grayscale conversion
* Resizing to `256 × 256`
* Normalization
* Conversion to PyTorch tensors

The target optical images are processed as RGB images.

---

### 2. Lightweight Generator

The generator receives a single-channel SAR image and produces a three-channel RGB image.

Conceptually:

```text
Input SAR
  │
  ▼
Initial Convolution
  │
  ▼
Depthwise Separable Convolutions
  │
  ▼
Residual Blocks
  │
  ▼
Upsampling
  │
  ▼
RGB Output
```

The architecture uses lightweight convolutional components and residual blocks to explore a balance between image generation capability and computational efficiency.

---

### 3. Discriminator

A convolutional discriminator is used to distinguish between:

* Real optical images
* Generated optical images

This provides adversarial feedback to the generator during training.

The discriminator uses convolutional layers together with activation and normalization components and produces a patch-level authenticity signal.

---

## Loss Function

The training process combines adversarial learning with pixel-level reconstruction.

### Adversarial Loss

The project uses:

```text
BCEWithLogitsLoss
```

to train the generator and discriminator through adversarial feedback.

### Reconstruction Loss

An L1 loss is also used:

```text
L1Loss
```

The generator therefore attempts to both:

1. Produce images that appear realistic to the discriminator.
2. Remain close to the corresponding optical target at the pixel level.

Conceptually:

```text
Generator Loss
=
Adversarial Loss
+
L1 Reconstruction Loss
```

This combination is useful for encouraging both perceptual realism and similarity to the paired optical target.

---

## Training Configuration

The original notebook uses the following main configuration:

| Parameter             |     Value |
| --------------------- | --------: |
| Image Size            | 256 × 256 |
| Input Channels        |         1 |
| Output Channels       |         3 |
| Training Epochs       |       100 |
| Data Split            | 80% / 20% |
| DataLoader Batch Size |        16 |
| Learning Rate         |    0.0001 |
| Adam β₁               |       0.5 |
| Random State          |        42 |
| Framework             |   PyTorch |

The notebook also includes GPU support and mixed-precision training.

---


## Model Checkpoints

During training, the notebook saves PyTorch model checkpoints.

The original workflow uses checkpoint files for:

```text
Generator
Discriminator
```

Example:

```text
generator_epoch_X.pth
discriminator_epoch_X.pth
```

Large model files and training artifacts may be excluded from GitHub depending on their size.


## Results

The notebook generates visual comparisons between:

```text
SAR Input
     │
     ├── Generated RGB Image
     │
     └── Ground Truth Optical Image
```

Example result layout:

```text
┌──────────────┬───────────────────┬─────────────────────┐
│ SAR Input    │ Generated Output  │ Real Optical Image  │
├──────────────┼───────────────────┼─────────────────────┤
│              │                   │                     │
│    SAR       │    Generated      │     Ground Truth    │
│              │      RGB          │       Optical       │
└──────────────┴───────────────────┴─────────────────────┘
```

Representative outputs from the hackathon implementation are included in the `results/` directory where available.

> **Important:** The current project does not claim a comprehensive quantitative evaluation using metrics such as SSIM, PSNR, FID, or LPIPS. The current results are primarily visual demonstrations from the original implementation.

---

## What Makes the Project Interesting

Although this repository is a raw hackathon implementation, it contains several technically relevant ideas.

### Remote Sensing + Generative AI

The project combines:

* Synthetic Aperture Radar imagery
* Computer vision
* Generative models
* Image-to-image translation
* Deep learning

This creates potential applications in remote sensing and satellite image analysis.

### Lightweight Architecture

Instead of relying on an extremely large image-generation architecture, the project explores a comparatively lightweight generator using convolutional and residual components.

This creates an interesting direction for investigating:

```text
Model Quality
      ↕
Computational Cost
      ↕
Inference Efficiency
```

### Paired Image Translation

The model is trained using corresponding SAR and optical imagery rather than treating colorization as an entirely unconstrained generation problem.

This allows the model to learn relationships between two different remote-sensing modalities.

### End-to-End Experimental Pipeline

Despite being notebook-based, the implementation covers the major stages of a deep learning experiment:

```text
Data
 ↓
Preprocessing
 ↓
Dataset Construction
 ↓
Model Definition
 ↓
Training
 ↓
Checkpointing
 ↓
Inference
 ↓
Visualization
```

---

## Current Limitations

This repository should be considered a **prototype rather than a finished research or production system**.

Current limitations include:

* Notebook-centric implementation
* No dedicated training CLI
* No dedicated inference script
* No automated experiment tracking
* No production API
* No deployment interface
* Limited quantitative evaluation
* No systematic comparison against alternative image-translation architectures
* Model checkpoints may need to be obtained separately
* The original hackathon environment relied on Google Drive/Colab paths

These limitations are intentionally documented rather than hidden.

---

## Potential Future Improvements

The current implementation provides a foundation for a more complete research project.

Possible extensions include:

### Quantitative Evaluation

Add objective evaluation using metrics such as:

* SSIM
* PSNR
* LPIPS
* FID

and compare generated images against the corresponding optical targets.

### Improved Architectures

Experiment with:

* Pix2Pix
* Pix2PixHD
* CycleGAN
* U-Net based generators
* Attention mechanisms
* Transformer-based image translation
* Diffusion-based approaches

### Better Dataset Pipeline

Develop:

```text
Dataset Download
      ↓
Validation
      ↓
Pair Matching
      ↓
Preprocessing
      ↓
Train / Validation / Test
```

with reproducible configuration.

### Experiment Tracking

Track:

* Generator loss
* Discriminator loss
* Reconstruction loss
* Validation metrics
* Sample outputs
* Training configuration

using an experiment tracking system.

### Deployment

A future version could expose the trained generator through an API:

```text
SAR Image
    ↓
FastAPI
    ↓
Trained Generator
    ↓
Generated Optical-like Image
```

and potentially provide a web interface for uploading SAR images and viewing generated outputs.

### Remote-Sensing Applications

With further validation, the approach could be investigated as a preprocessing or visualization component for:

* satellite image interpretation
* remote-sensing visualization
* land-cover analysis
* disaster monitoring
* environmental monitoring
* multi-modal satellite imagery research

These are **potential directions**, not capabilities currently validated by this repository.

---

## Project Structure

The current repository is intentionally lightweight:

```text
SAR-Image-Colorization-GAN/
│
├── README.md
├── requirements.txt
├── .gitignore
├── LICENSE
│
├── notebook/
│   └── SAR_Image_Colorization_GAN.ipynb
│
├── data/
│
├── checkpoints/

```

The notebook remains the primary implementation.

A modular `src/` implementation can be added in a future version without modifying the original hackathon notebook.

---

## Reproducibility Note

This repository preserves the original hackathon implementation.

Therefore, the exact results may vary depending on:

* dataset version
* dataset ordering
* GPU environment
* PyTorch version
* random initialization
* available checkpoints
* training configuration

The repository should currently be viewed as a record of the team's hackathon implementation and an experimental foundation rather than a fully reproducible research benchmark.

---

## Hackathon Context

This project was developed as part of a team-based hackathon project.

The repository is being published to document:

* the problem we explored
* our model architecture
* our experimental approach
* the implementation produced during the hackathon
* the potential directions for further development

The current repository intentionally preserves the project's original state rather than retrospectively presenting it as a production-ready system.


---

## Technologies

* Python
* PyTorch
* Torchvision
* NumPy
* OpenCV
* Albumentations
* Scikit-learn
* Scikit-image
* Matplotlib
* Google Colab / Jupyter

---

