# Dataset preparation

## Dataset used in the paper
The experiments use the paired `underwater-dark` subset of the Enhancing Underwater Visual Perception (EUVP) dataset.

Reported split counts:
- Training: 11,442 paired images
- Validation: 638 paired images

The paper does not specify a separate test count in the dataset paragraph. Its results are reported on an EUVP test set.

## Download and licensing
Obtain EUVP from its official distribution/source. Follow the dataset's license, citation, and redistribution rules. This repository does not include the dataset.

## Suggested local layout
```text
data/EUVP/underwater-dark/
├── trainA/
├── trainB/
├── testA/
└── testB/
```
This is a suggested convention, not a claim about the exact names in every EUVP release. Inspect your downloaded archive and map the paired degraded/reference folders correctly.

## Data integrity checklist
- Ensure every degraded image has its corresponding reference image.
- Verify pairing by filename or an explicit pair manifest.
- Avoid mixing train, validation, and test images.
- Resize to 224 × 224 as reported.
- Apply random horizontal flipping only to training pairs, identically to both images.
- Record color-space conversion, normalization, and split manifests.
- Never commit dataset images or private download credentials to Git.
