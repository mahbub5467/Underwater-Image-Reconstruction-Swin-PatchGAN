# Underwater Image Reconstruction Using a Swin Transformer-Based Generator and PatchGAN Discriminator

[![Paper](https://img.shields.io/badge/IEEE-ICCIT%202025-blue)](https://doi.org/10.1109/ICCIT68739.2025.11491261)
[![Task](https://img.shields.io/badge/Task-Underwater%20Image%20Reconstruction-0A7B83)](#)
[![Framework](https://img.shields.io/badge/Framework-PyTorch-ee4c2c)](https://pytorch.org/)

Official research project repository for **Swin-PatchGAN**, an underwater image reconstruction framework combining a Swin Transformer-based U-Net generator with a PatchGAN discriminator.

> **Repository status:** This repository currently provides the paper summary, reported results, experiment protocol, dataset guidance, and reproducibility structure. The original training source code, trained checkpoints, and dataset files have not been included in this release. Do not interpret the documentation or starter configuration as a complete executable reproduction of the published model.

## Paper

**Underwater Image Reconstruction Using a Swin Transformer-Based Generator and PatchGAN Discriminator**

- **Authors:** Md. Mahbub Hasan Akash, Aria Tasnim Mridula, Sheekar Banerjee, and Ishtiak Al Mamoon
- **Venue:** 2025 28th International Conference on Computer and Information Technology (ICCIT)
- **Conference dates:** 19–21 December 2025
- **Location:** Cox's Bazar, Bangladesh
- **Pages:** 4687–4692
- **DOI:** [10.1109/ICCIT68739.2025.11491261](https://doi.org/10.1109/ICCIT68739.2025.11491261)

## Abstract

Underwater images often suffer from wavelength-dependent absorption and light scattering, producing color distortion, low contrast, haze, and loss of detail. This work presents Swin-PatchGAN, a GAN-based framework for underwater image reconstruction. Its generator follows a U-Net encoder–decoder design and integrates Swin Transformer blocks to model local features and long-range dependencies. A PatchGAN discriminator evaluates local image patches and provides adversarial feedback to encourage realistic high-frequency details. The framework was trained and evaluated using the paired `underwater-dark` subset of the EUVP dataset. The reported results are 24.76 dB PSNR, 0.89 SSIM, and 4.18 UIQM.

## Method overview

### Generator: Swin-UNet

The generator maps a degraded underwater image to an enhanced image. As described in the paper, it uses:

- A symmetric U-Net encoder–decoder with skip connections.
- Non-overlapping `4 × 4` patch partitioning and linear embedding.
- Four hierarchical encoder stages.
- Swin Transformer blocks with shifted-window self-attention.
- Patch-merging operations between encoder stages.
- Patch-expanding operations in the decoder.
- A final linear projection and `tanh` output activation, producing values in `[-1, 1]`.

### Discriminator: PatchGAN

The conditional discriminator receives the low-quality image concatenated with either the target image or the generated image. It produces a patch-level probability map rather than a single whole-image score.

The paper describes five convolutional layers with feature dimensions `64, 128, 256, 512, 1`. Intermediate layers use batch normalization and LeakyReLU (slope `0.2`); the final layer uses sigmoid activation.

### Objective

The generator objective combines adversarial loss and L1 reconstruction loss:

`L_G = L_GAN + λ L_L1`, where `λ = 100`.

The discriminator is trained to distinguish real paired samples from generated paired samples.

## Dataset and training protocol

The paper reports using the paired **`underwater-dark`** subset of the **Enhancing Underwater Visual Perception (EUVP)** dataset.

| Setting | Reported value |
|---|---:|
| Training pairs | 11,442 |
| Validation pairs | 638 |
| Input image size | 224 × 224 |
| Framework | PyTorch |
| GPU | NVIDIA Tesla V100 |
| Optimizer | Adam |
| Learning rate | 0.0002 |
| β1 | 0.5 |
| β2 | 0.999 |
| Batch size | 16 |
| Epochs | 200 |
| Augmentation | Random horizontal flip |
| Approximate training time | 48 hours on a single GPU |

The paper does not specify every implementation detail required for bit-for-bit reproduction (for example, all Swin block dimensions, exact preprocessing code, random seeds, and software package versions). Those details should be documented when the original implementation is released.

### Dataset access and organization

Obtain EUVP from its official project/source and follow its terms of use. The dataset is not redistributed in this repository.

A suggested local layout is:

```text
data/
└── EUVP/
    └── underwater-dark/
        ├── trainA/       # degraded/input images
        ├── trainB/       # paired reference images
        ├── testA/        # if supplied by the dataset split
        └── testB/        # if supplied by the dataset split
```

The directory names above are an organizational suggestion; check the downloaded EUVP release and map its actual folder names before running any data loader. Keep paired input and reference filenames aligned.

See [`docs/DATASET.md`](docs/DATASET.md) for data handling notes.

## Reported quantitative results

### Comparison on the EUVP test set

| Method | PSNR (dB) ↑ | SSIM ↑ | UIQM ↑ |
|---|---:|---:|---:|
| Input | 16.45 | 0.72 | 2.89 |
| Histogram Equalization | 17.23 | 0.74 | 3.12 |
| UDCP | 18.76 | 0.79 | 3.45 |
| WaterGAN | 21.34 | 0.83 | 3.78 |
| UIE-DAL | 23.15 | 0.86 | 3.92 |
| **Swin-PatchGAN (proposed)** | **24.76** | **0.89** | **4.18** |

### Ablation study on the EUVP test set

| Configuration | PSNR (dB) ↑ | SSIM ↑ | UIQM ↑ |
|---|---:|---:|---:|
| Baseline U-Net | 22.45 | 0.84 | 3.87 |
| U-Net + Attention | 23.12 | 0.86 | 3.94 |
| U-Net + PatchGAN | 23.38 | 0.87 | 3.96 |
| **Swin-UNet + PatchGAN** | **24.76** | **0.89** | **4.18** |

The paper reports that replacing conventional convolutional blocks with Swin Transformer blocks gives a `+1.38 dB` PSNR improvement over the U-Net + PatchGAN configuration.

Machine-readable copies of these tables are available in [`results/`](results/).

## Qualitative observations reported in the paper

The paper describes improved color balance, contrast, and visibility, including reduced green/blue color casts and haze while retaining details and textures. Add original comparison images to `results/qualitative/` only when redistribution is permitted, and include captions identifying the input, reference, and reconstructed output.

## Evaluation

The paper reports three image-quality metrics:

- **PSNR:** pixel-level reconstruction fidelity.
- **SSIM:** structural similarity.
- **UIQM:** underwater image quality measure.

For reproducible comparisons, evaluate all methods on the same test split, image resolution, color space, and metric implementation. Record whether metrics are computed per image then averaged or computed over a concatenated set. The paper summary here does not specify every such implementation detail.

## Repository structure

```text
.
├── .github/
│   └── ISSUE_TEMPLATE/
├── checkpoints/           # Place trained weights here; not included
├── data/                  # Place locally obtained EUVP data here; not included
├── docs/
│   ├── DATASET.md
│   ├── EXPERIMENTS.md
│   ├── MODEL.md
│   └── REPRODUCIBILITY.md
├── paper/                 # Paper metadata and citation
├── results/               # Reported metric tables and your own outputs
├── src/                   # Original model/training implementation to be added
├── CITATION.cff
├── CONTRIBUTING.md
├── LICENSE
├── README.md
└── requirements.txt
```

## Limitations and future work

The paper identifies several practical considerations:

- Swin self-attention introduces computational overhead.
- The supervised setup relies on paired training data.
- Loss-weight selection and training hyperparameters require tuning.

Potential future directions include improving computational efficiency, evaluating generalization across additional underwater datasets and water conditions, and documenting deployment constraints for edge devices.

## Citation

If you use or discuss this work, please cite the paper:

```bibtex
@inproceedings{akash2025underwater,
  author    = {Akash, Md. Mahbub Hasan and Mridula, Aria Tasnim and
               Banerjee, Sheekar and Mamoon, Ishtiak Al},
  title     = {Underwater Image Reconstruction Using a Swin Transformer-Based
               Generator and PatchGAN Discriminator},
  booktitle = {2025 28th International Conference on Computer and Information
               Technology (ICCIT)},
  year      = {2025},
  pages     = {4687--4692},
  address   = {Cox's Bazar, Bangladesh},
  doi       = {10.1109/ICCIT68739.2025.11491261}
}
```

See also [`CITATION.cff`](CITATION.cff).

## Authors

- **Md. Mahbub Hasan Akash** — Department of Computer Science and Engineering, IUBAT, Dhaka, Bangladesh
- **Aria Tasnim Mridula** — Department of Computer Science and Engineering, IUBAT, Dhaka, Bangladesh
- **Sheekar Banerjee** — Department of Computer Science and Engineering, IUBAT, Dhaka, Bangladesh
- **Ishtiak Al Mamoon** — Department of Computer Science and Engineering, IUBAT, Dhaka, Bangladesh

## Acknowledgement

The authors acknowledge the EUVP dataset as the data source used in this research. Please consult the dataset's own documentation and license for attribution and usage requirements.
