# Source code

The original model and training implementation were not present in the paper file supplied for preparing this repository package. Add the verified implementation here when available.

Suggested organization:

```text
src/
├── models/
│   ├── generator.py
│   └── discriminator.py
├── datasets/
│   └── euvp.py
├── losses/
│   └── gan_l1.py
├── metrics/
│   ├── psnr.py
│   ├── ssim.py
│   └── uiqm.py
├── train.py
└── evaluate.py
```

Do not label a newly written implementation as the exact original implementation unless it has been checked against the authors' code and experimental settings.
