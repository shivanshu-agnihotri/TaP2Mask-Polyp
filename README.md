# Tap2Mask-Polyp

Official repository for **Tap2Mask-Polyp**, a prompt-free, end-to-end polyp segmentation network built on a foundation-model ViT encoder (SAM or DINOv2) with multi-level feature tapping.

>
> **Status: partial release.** This repository currently contains the model interface and the evaluation and visualization scripts. The full implementation will be released after acceptance.

---

## Overview

Tap2Mask-Polyp segments polyps from a single image without any manual prompts (no points, boxes, or text at inference). Features are tapped from several depths of a pretrained ViT encoder, fused by a neck module, and decoded into a segmentation mask. During training, auxiliary heads on perturbed features provide additional supervision. At inference only the main head is used.

Two encoder variants are supported:

| Variant | Encoder | Entry point |
|---|---|---|
| Tap2Mask-Polyp (SAM) | SAM ViT-B | `tap2mask_sam.py` |
| Tap2Mask-Polyp (DINOv2) | DINOv2 ViT-B/14 | `tap2mask_dinov2.py` |

---

## What is released now

| File | Description |
|---|---|
| `tap2Mask-polyp.ipynb` | Top-level model class and builder for the SAM variant (interface only) |
| `tap2Mask-polyp.ipynb` | Top-level model class and builder for the DINOv2 variant (interface only) |
| `tap2Mask-polyp.ipynb` | Quantitative evaluation: Dice, IoU, S-measure, weighted F-measure, MAE, FPS |
| `tap2Mask-polyp.ipynb` | Saves input, GT, prediction, and overlay images; plots random samples |
| `README.md` | This file |

**Please note:**
- The model files define how the network is assembled and what its inputs and outputs are. The internal modules (encoder tapping, fusion neck, decoder heads, perturbation modules) are **not** included yet, so the scripts cannot be run end to end from this repository alone.
- The scripts document the exact evaluation protocol used in the paper: datasets, metrics, resizing, and the 0.5 threshold.

---

## Evaluation protocol

- **Test datasets:** CVC-300, CVC-ClinicDB, CVC-ColonDB, Kvasir, ETIS-LaribPolypDB
- **Folder layout:** each dataset folder contains `images/` and `masks/` with matching filenames
- **Input:** RGB image resized to the model input size; the model normalizes internally
- **Prediction:** sigmoid on the polyp channel, bilinear resize back to the original resolution, binarized at 0.5
- **Metrics:** Dice, IoU, S-measure (Sm), weighted F-measure (wFm), MAE, and FPS (using [`py_sod_metrics`](https://github.com/lartpang/PySODMetrics))

```
TestDB/
├── CVC-300/
│   ├── images/
│   └── masks/
├── CVC-ClinicDB/
├── CVC-ColonDB/
├── Kvasir/
└── ETIS-LaribPolypDB/
```

---

## Requirements

```
torch
numpy
opencv-python
matplotlib
py_sod_metrics
segment-anything   # SAM variant
```

The DINOv2 variant loads the encoder through `torch.hub` (`facebookresearch/dinov2`).

---

## Release plan

**Planned for release after acceptance:**

- [ ] Full model implementation (multi-level encoder backbone, fusion neck, decoder heads, perturbation modules)
- [ ] Training code, including losses and the auxiliary-head scheme
- [ ] Data preparation and augmentation pipeline
- [ ] Training and model configuration files
- [ ] Trained checkpoints for both the SAM and DINOv2 variants
- [ ] Pre-computed prediction maps for all five test datasets
- [ ] Reproduction instructions with the exact environment

**Possible earlier additions (before acceptance):**

- [ ] Prediction maps for the test sets, so results can be inspected independently of the code
- [ ] Qualitative visualizations from the paper

---

## Citation

If you find this work useful, please cite our paper once it is available:

```bibtex
@article{tap2maskpolyp,
  title   = {[Paper title]},
  author  = {[Authors]},
  journal = {[Venue]},
  year    = {[Year]}
}
```

---

## License

[Choose a license, e.g. MIT, Apache-2.0, or CC BY-NC 4.0 for non-commercial use.]

## Acknowledgements

This work builds on [Segment Anything](https://github.com/facebookresearch/segment-anything) and [DINOv2](https://github.com/facebookresearch/dinov2), and uses the polyp datasets CVC-ClinicDB, CVC-ColonDB, CVC-300, Kvasir, and ETIS-LaribPolypDB.

## Contact

[Email or issue tracker link]
