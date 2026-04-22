# DSANet-ISLES: Interpretable Multi-Architecture Ensemble for Stroke Lesion Segmentation

[![Live Demo](https://img.shields.io/badge/Live-Demo-brightgreen)](https://pronad1.github.io/DSANet/)
[![Dataset](https://img.shields.io/badge/Dataset-ISLES%202022-blue)](https://zenodo.org/records/7153326)
[![Paper](https://img.shields.io/badge/Paper-Under%20Review-red)](#citation)
[![License](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey)](http://creativecommons.org/licenses/by-sa/4.0/)

**DSANet-ISLES: An Interpretable Multi-Architecture Ensemble of DERNet, SegResNet, and Attention U-Net for Low-Resource Stroke Lesion Segmentation**

## Live Project

Visit the project webpage for figures, qualitative outputs, and full visual narrative:

**https://pronad1.github.io/DSANet/**

---

## Abstract

Accurate automated segmentation of ischemic stroke lesions is essential for timely clinical decision-making, but real-world deployment remains difficult in low-resource settings due to heterogeneity in image quality and limited computational infrastructure. DSANet-ISLES introduces an interpretable ensemble framework that combines DERNet, SegResNet, and Attention U-Net to capture complementary spatial and contextual features from multi-modal MRI (FLAIR, DWI, ADC). Model predictions are fused using validation-optimized weighted ensembling with threshold selection, then refined via test-time augmentation and morphological post-processing. On ISLES 2022, DSANet-ISLES achieves Dice 0.8092 and Micro-F1 0.8814, demonstrating robust lesion delineation compared to individual model outputs.

---

## Key Contributions

- Multi-architecture fusion of DERNet, SegResNet, and Attention U-Net for complementary representation learning.
- Validation-driven weighted ensemble with grid-searched threshold selection for stable inference.
- Robust inference strategy combining TTA (original, X-flip, Y-flip) and component-level post-processing.
- Strong performance on ISLES 2022 under a low-resource deployment perspective.
- Interpretable and practical workflow designed for variable MRI quality conditions.

---

## Performance Summary

### Final Segmentation Metrics (ISLES 2022 Test)

| Metric | Score |
| :--- | :---: |
| Dice | **0.8092** |
| Micro-F1 | **0.8814** |

### Final Inference Setting

| Setting | Value |
| :--- | :--- |
| Ensemble Weights $(w_D, w_A, w_S)$ | $(0.8, 0.2, 0.0)$ |
| Decision Threshold $\tau$ | $0.5$ |
| TTA | Original + X-flip + Y-flip |
| Post-processing | Remove connected components with size < 30 |

---

## Dataset

- **Benchmark:** ISLES 2022
- **Modalities:** FLAIR, DWI, ADC
- **Split:** 70% train / 15% validation / 15% test
- **Access:** https://zenodo.org/records/7153326

---

## Technical Methodology

### 1. Multi-Modal Input Construction

FLAIR, DWI, and ADC volumes are aligned and fused into a unified 3D input representation to improve lesion characterization.

### 2. Independent Backbone Training

Three architectures are trained independently as complementary experts:

- DERNet
- SegResNet
- Attention U-Net

### 3. Weighted Ensemble and Threshold Selection

Predictions are fused by weighted averaging with validation-driven search:

$$
P_{ens} = w_D P_{DERNet} + w_A P_{AttUNet} + w_S P_{SegResNet}
$$

Final weights:

$$
(w_D, w_A, w_S) = (0.8, 0.2, 0.0)
$$

Binary decision rule:

$$
\hat{Y} = \mathbb{1}[P_{ens} \ge \tau], \quad \tau = 0.5
$$

### 4. Test-Time Augmentation (TTA)

$$
P_{TTA} = \frac{1}{3}\big(P(x) + P(Flip_x(x)) + P(Flip_y(x))\big)
$$

### 5. Post-Processing

Small connected components are removed (minimum size threshold = 30) to suppress tiny false positives.

---

## Methodology Overview (Figure)

Core four-stage workflow:

1. Multi-modal data preparation (FLAIR, DWI, ADC)
2. Independent expert model training
3. Weighted ensemble + threshold optimization
4. TTA and morphological refinement

![DSANet-ISLES Methodology 1](./static/images/methodoloy%201.jpeg)

---

## Training Configuration (Reported)

| Parameter | Value |
| :--- | :--- |
| Optimizer | AdamW |
| Learning Rate | $1 \times 10^{-4}$ (Cosine Annealing) |
| Loss | DiceFocalLoss / DiceCELoss |
| Epochs | 100-150 (model dependent) |
| Effective Batch | 1-4 (model dependent accumulation) |
| Crop Size | $64 \times 64 \times 64$ |

---

## Error Characteristics (Observed)

- Boundary ambiguity in low-contrast lesion edges.
- Small isolated false positives in noisy regions.
- Fusion + TTA + connected-component filtering improves consistency and contour quality.

---

## Repository Structure

```text
.
|- index.html
|- README.md
|- excel_contents/
`- static/
   |- css/
   |- images/
   `- js/
```

---

## Local Preview

This repository is a static website.

```bash
python -m http.server 8000
```

Open: http://localhost:8000

---

## Citation

If you use this project, please cite:

```bibtex
@article{DSANetISLES2026,
  author    = {Anonymized Authors},
  title     = {DSANet-ISLES: An Interpretable Multi-Architecture Ensemble of DerNet, SegResNet, and Attention U-Net for Low-Resource Stroke Lesion Segmentation},
  journal   = {Under Review},
  year      = {2026}
}
```

## Authors

- Anonymized Authors

## Acknowledgment

Website template adapted from Nerfies:
https://github.com/nerfies/nerfies.github.io

## License

See LICENSE for project licensing terms.
