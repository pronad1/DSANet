# DSANet-ISLES

[![Live Demo](https://img.shields.io/badge/Live-Demo-brightgreen)](https://pronad1.github.io/DSANet/)
[![Dataset](https://img.shields.io/badge/Dataset-ISLES%202022-blue)](https://zenodo.org/records/7153326)
[![Paper](https://img.shields.io/badge/Paper-Under%20Review-red)](#citation)
[![License](https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey)](http://creativecommons.org/licenses/by-sa/4.0/)

**DSANet-ISLES: An Interpretable Multi-Architecture Ensemble of DerNet, SegResNet, and Attention U-Net for Low-Resource Stroke Lesion Segmentation**

## Project Page

Live website: https://pronad1.github.io/DSANet/

## Abstract

Accurate automated segmentation of ischemic stroke lesions is essential for timely clinical decision-making and treatment planning. However, practical deployment remains challenging in low-resource settings with limited computational infrastructure. DSANet-ISLES is an interpretable multi-architecture ensemble framework for ischemic stroke lesion segmentation from multi-modal MRI.

The method combines DerNet, SegResNet, and Attention U-Net as complementary experts, integrating FLAIR, DWI, and ADC into a unified 3D representation. Predictions are fused through a validation-driven weighted ensemble, followed by threshold optimization, test-time augmentation (TTA), and morphological post-processing to improve robustness and reduce false positives.

On the ISLES 2022 test set, DSANet-ISLES achieves:

- Dice: **0.8092**
- Micro-F1: **0.8814**

## Dataset and Split

- Dataset: ISLES 2022
- Modalities: FLAIR, DWI, ADC
- Split: 70% train / 15% validation / 15% test
- Access: https://zenodo.org/records/7153326

## Method Summary

1. Multi-modal MRI input preparation (FLAIR, DWI, ADC).
2. Independent training of DerNet, SegResNet, and Attention U-Net.
3. Validation-driven weighted ensemble and threshold search.
4. TTA aggregation and connected-component post-processing.

### Weighted Ensemble

$$
P_{ens} = w_D P_{DERNet} + w_A P_{AttUNet} + w_S P_{SegResNet}
$$

Final weights:

$$
w_D = 0.8,\quad w_A = 0.2,\quad w_S = 0.0
$$

### TTA and Decision Rule

$$
\begin{aligned}
P_{TTA} = \frac{1}{3}\Big(&P_{ens}(x)
+ Flip_x^{-1}(P_{ens}(Flip_x(x))) \\
&+ Flip_y^{-1}(P_{ens}(Flip_y(x)))\Big)
\end{aligned}
$$

$$
\hat{Y} = \mathbb{1}[P_{TTA} \geq \tau], \quad \tau = 0.5
$$

Post-processing removes connected components smaller than 30 voxels.

## Training Configuration

| Parameter | Value |
| :--- | :--- |
| Optimizer | AdamW |
| Learning rate | $1\times10^{-4}$ (Cosine Annealing) |
| Loss | DiceFocalLoss / DiceCELoss |
| Epochs | 100-150 (model-dependent) |
| Effective batch | 1-4 (model-dependent accumulation) |
| Crop size | $64 \times 64 \times 64$ |

## Key Outcomes

- Improves robustness over single-model predictions.
- Handles variable MRI quality in low-resource clinical settings.
- Reduces small noisy false positives through TTA and morphology refinement.

## Repository Structure

```text
.
|- index.html
|- README.md
|- cimilab.md
`- static/
   |- css/
   |- images/
   `- js/
```

## Local Preview

```bash
python -m http.server 8000
```

Open http://localhost:8000 in your browser.

## Citation

```bibtex
@article{DSANetISLES2026,
  author  = {Anonymized Authors},
  title   = {DSANet-ISLES: An Interpretable Multi-Architecture Ensemble of DerNet, SegResNet, and Attention U-Net for Low-Resource Stroke Lesion Segmentation},
  journal = {Under Review},
  year    = {2026}
}
```

## Authors

- Anonymized Authors

## Acknowledgment

Website template adapted from Nerfies: https://github.com/nerfies/nerfies.github.io

## License

See LICENSE for project licensing terms.
