# Reproducibility checklist

Before publishing a reproduction, record:

- [ ] Dataset source, license, subset, and download date.
- [ ] Exact split manifests or stable identifiers.
- [ ] Pairing validation and image preprocessing.
- [ ] Input resolution and normalization.
- [ ] Full generator configuration (depths, dimensions, heads, windows).
- [ ] Full discriminator configuration.
- [ ] Optimizer, learning rate, betas, scheduler, and loss weights.
- [ ] Random seed and deterministic settings.
- [ ] Python/PyTorch/CUDA/GPU environment.
- [ ] Training command, logs, checkpoint-selection rule, and duration.
- [ ] PSNR, SSIM, and UIQM implementations and aggregation protocol.
- [ ] Test-set results and qualitative examples, with permission to redistribute.

A reproduction should be distinguished from the values reported in the original paper.
