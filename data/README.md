# Dataset

This directory contains the dataset required by the SAR-to-optical image translation notebook.

The original hackathon implementation uses **paired SAR and optical `.tif` images**.

## Expected Structure

```text
data/
└── raw/
    ├── sar/
    │   ├── image_001.tif
    │   ├── image_002.tif
    │   └── ...
    │
    └── optical/
        ├── image_001.tif
        ├── image_002.tif
        └── ...
```

The SAR and optical directories should contain corresponding image pairs.

For example:

```text
sar/image_001.tif
optical/image_001.tif
```

represent one training pair.

## Dataset Format

The notebook expects:

* `.tif` image files
* SAR imagery as the input modality
* Optical imagery as the target modality
* Corresponding SAR/optical image pairs

The notebook preprocesses the images to `256 × 256` resolution before they are passed to the model.

## Dataset Availability

The original dataset used by the team during the hackathon is **not included in this repository** because of its size and redistribution considerations.

The original notebook accessed the dataset through Google Drive.

The source, license, and redistribution permissions of the original dataset should be verified separately before sharing the dataset publicly.

## Using Your Own Dataset

To run the notebook with another compatible dataset, organize the files as:

```text
data/raw/sar/
data/raw/optical/
```

and ensure that the SAR and optical images are correctly paired.

The paths used by the original notebook may need to be updated for the local environment.

## Important Note

This repository preserves the dataset organization expected by the original hackathon implementation. It does not claim ownership of, or redistribution rights for, the original remote-sensing dataset.
