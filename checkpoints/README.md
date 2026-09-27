# Model Checkpoints

This directory is intended to contain the trained PyTorch model checkpoints produced by the SAR-to-optical image translation notebook.

The project uses two neural networks:

```text
Generator
    ↓
Generates RGB optical-like images from SAR images

Discriminator
    ↓
Distinguishes generated images from real optical images
```

## Expected Checkpoint Files

The original notebook saves checkpoints for both models using the following naming pattern:

```text
checkpoints/
├── generator_epoch_X.pth
└── discriminator_epoch_X.pth
```

where `X` represents the training epoch.

For example:

```text
generator_epoch_50.pth
discriminator_epoch_50.pth
```

## Generator

The generator takes a single-channel SAR image as input and produces a three-channel RGB image.

```text
SAR Image (1 channel)
        ↓
    Generator
        ↓
RGB Generated Image (3 channels)
```

The generator is the primary model required for inference after training.

## Discriminator

The discriminator is used during GAN training to distinguish between real optical images and generated images.

It is required when continuing GAN training or reproducing the original training process.

For inference using an already trained generator, the discriminator is generally not required.

## Checkpoint Availability

Large `.pth` files are intentionally **not committed to the main Git repository**.

This keeps the repository lightweight and avoids storing large binary model files directly in Git history.

If checkpoints are distributed separately, place them inside this directory before running the relevant notebook cells.

## Loading Checkpoints

The notebook automatically searches for previously saved checkpoints when continuing training.

The expected checkpoint location and naming convention should therefore be preserved when using the original notebook.

If you are running the notebook in a new environment, update the checkpoint path to point to this directory or to the location where the downloaded checkpoints are stored.

## Recommended Distribution

For future versions of the project, trained checkpoints can be distributed through:

* GitHub Releases
* Hugging Face Hub
* Another appropriate model-hosting service

The main Git repository should contain the code and documentation rather than large model binaries.

## Important Note

The checkpoints represent models trained using the original hackathon implementation. Their performance depends on the dataset, preprocessing, training configuration, and environment used during training.

They should therefore be considered **experimental research/hackathon artifacts**, not production-ready models.
