# Experiment protocol

The following values are reported in the paper.

| Parameter | Value |
|---|---|
| Dataset | EUVP `underwater-dark` paired subset |
| Training pairs | 11,442 |
| Validation pairs | 638 |
| Image size | 224 × 224 |
| Framework | PyTorch |
| GPU | NVIDIA Tesla V100 |
| Optimizer | Adam |
| Learning rate | 0.0002 |
| β1 / β2 | 0.5 / 0.999 |
| Batch size | 16 |
| Epochs | 200 |
| Augmentation | Random horizontal flip |
| Approximate duration | 48 hours on one GPU |
| Generator loss | Adversarial + λL1 |
| λ | 100 |

## Not fully specified in the paper
The following must be recovered from the original implementation or explicitly selected and documented for a new reproduction:
- Python, CUDA, cuDNN, and PyTorch versions.
- Exact Swin stage depths, embedding dimensions, attention heads, and window size.
- Initialization and learning-rate scheduling details.
- Random seed and deterministic settings.
- Exact train/validation/test manifests and preprocessing/normalization implementation.
- Metric implementation details and aggregation protocol.

Do not present chosen defaults for these missing details as paper-reported settings.
