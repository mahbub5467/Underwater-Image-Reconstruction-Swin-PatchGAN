# Model architecture notes

This document summarizes the architecture described in the ICCIT 2025 paper. It is not a complete implementation specification.

## Generator
1. Input: degraded RGB image.
2. Partition the image into non-overlapping 4 × 4 patches.
3. Project patches into an embedding space.
4. Encode through four hierarchical Swin Transformer stages.
5. Use patch merging between encoder stages.
6. Process deep features in a Swin Transformer bottleneck.
7. Decode with patch-expanding layers.
8. Fuse corresponding encoder and decoder features through skip connections.
9. Apply a final linear projection and `tanh` activation to obtain output in `[-1, 1]`.

## Swin Transformer block
The paper describes shifted-window multi-head self-attention (SW-MSA), with relative position bias. Consecutive regular-window and shifted-window blocks enable communication across window boundaries.

## Discriminator
The conditional PatchGAN receives concatenated pairs:
- Real: `(degraded image, high-quality target)`
- Fake: `(degraded image, generated reconstruction)`

It uses five convolutional layers with channel sizes 64, 128, 256, 512, and 1. Intermediate layers use batch normalization and LeakyReLU with slope 0.2; the final layer uses sigmoid to output a patch probability map.

## Loss
`L_G = L_GAN + 100 × L1`

The paper does not provide all architectural hyperparameters (such as exact embedding dimensions, depths, number of attention heads, and window size) in the available description. These must not be guessed when claiming an exact reproduction.
