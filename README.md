# Uncertainty-Aware Fusion of Foundation and Task-Specific Models for Cardiac MRI Segmentation

This repository contains the implementation of an uncertainty-aware fusion framework for cardiac MRI segmentation. The method combines predictions from foundation models and task-specific segmentation models using **Dempster–Shafer Theory (DST)** and an **entropy-guided fallback mechanism**.

The goal is to improve segmentation robustness, especially under domain shift, by explicitly modeling inter-model agreement, conflict, and uncertainty rather than using simple averaging.

## Overview

Cardiac MRI segmentation remains challenging due to anatomical variability, weak boundaries, scanner differences, and cross-dataset domain shift. Foundation models such as SAM/SAM2 offer strong generalization, while task-specific models such as nnU-Net and UNet++ provide domain-specific accuracy. This project explores how these complementary strengths can be fused in a principled way.

Our framework:
- takes calibrated soft probability maps from two models,
- computes voxel-wise conflict between them,
- applies DST fusion in low-conflict regions,
- uses entropy-guided fallback in high-conflict regions,
- produces a final segmentation that is more robust than either model alone.

## Main Contributions

- A **training-free voxel-wise fusion framework** for combining foundation and task-specific segmentation models.
- Use of **Dempster–Shafer Theory** to model agreement and disagreement between model predictions.
- An **entropy-based fallback strategy** for handling high-conflict regions where direct DST fusion becomes unreliable.
- Evaluation on **cardiac MRI segmentation** under both **in-domain** and **cross-domain** settings.

## Repository Structure

This repository currently contains the following notebooks:

- `MedSAM_finetuning_MnM_ACDC.ipynb` — fine-tuning MedSAM/MedSAM2-style model on MnM and evaluation on ACDC
- `SAM2_finetuning_MnM_ACDC.ipynb` — fine-tuning SAM2 on MnM and evaluation on ACDC
- `nnUnet_MnM.ipynb` — nnU-Net experiments on the MnM dataset
- `nnUnet_Acdc.ipynb` — nnU-Net experiments on the ACDC dataset
- `UNet++_MnM_ACDC.ipynb` — UNet++ experiments for MnM/ACDC
- `DST_fusion_MedSAM2.ipynb` — uncertainty-aware DST fusion using MedSAM2 outputs
- `DST_fusion_SAM2.ipynb` — uncertainty-aware DST fusion using SAM2 outputs
- `ablation1_averaging.ipynb` — averaging-based ablation for comparison with DST fusion
- `statistical_testing.ipynb` — statistical significance testing of segmentation results

## Method Summary

The fusion pipeline consists of the following steps:

1. Train or fine-tune base segmentation models.
2. Generate softmax probability maps for each model.
3. Calibrate probabilities if needed.
4. Compute voxel-wise entropy and inter-model conflict.
5. Apply DST fusion in low-conflict regions.
6. Apply entropy-guided fallback in high-conflict regions.
7. Evaluate the fused segmentation using Dice and IoU.

## Datasets

Experiments are conducted on:
- **MnM (Multi-Centre, Multi-Vendor & Multi-Disease Cardiac Image Segmentation Challenge)** for in-domain training/evaluation
- **ACDC (Automated Cardiac Diagnosis Challenge)** for cross-domain evaluation

Please ensure you have permission to access and use these datasets according to their official licensing terms.

## Experimental Setting

This repository studies fusion between:
- foundation models: **SAM2 / MedSAM2**
- task-specific models: **nnU-Net / UNet++**

Evaluation is performed for cardiac structures such as:
- Left Ventricle (LV)
- Right Ventricle (RV)
- Myocardium (MYO)

Metrics include:
- Dice score
- IoU
- Class-wise performance
- Statistical significance analysis

## How to Run

Because this repository is notebook-based, experiments are organized by stage.

Suggested order:

### 1. Train or fine-tune base models
Run one or more of:
- `MedSAM_finetuning_MnM_ACDC.ipynb`
- `SAM2_finetuning_MnM_ACDC.ipynb`
- `nnUnet_MnM.ipynb`
- `nnUnet_Acdc.ipynb`
- `UNet++_MnM_ACDC.ipynb`

### 2. Generate soft probability maps
Save model outputs in probability form for fusion.

### 3. Run fusion
Use:
- `DST_fusion_MedSAM2.ipynb`
- `DST_fusion_SAM2.ipynb`

### 4. Run ablation
Use:
- `ablation1_averaging.ipynb`

### 5. Perform statistical analysis
Use:
- `statistical_testing.ipynb`

## Requirements

Recommended environment:
- Python 3.10+
- Jupyter Notebook / Google Colab
- PyTorch
- NumPy
- SciPy
- Matplotlib
- MONAI
- nibabel
- scikit-learn

> Exact package versions may vary depending on the notebook. A `requirements.txt` file can be added later for reproducibility.

## Results

The uncertainty-aware DST fusion framework consistently improves robustness by:
- preserving agreement when both models are confident,
- resolving disagreement through explicit conflict handling,
- reducing the impact of unreliable predictions in uncertain regions.

This is especially useful when combining models with complementary strengths under domain shift.

## Notes

- This repository is currently organized primarily as research notebooks.
- Some dataset paths and runtime settings may need to be adapted locally.
- Future updates may include script-based pipelines and cleaner reproducibility support.

## Citation

If you use this work, please cite:

```bibtex
@misc{rumman_uncertainty_aware_fusion,
  title={Uncertainty-Aware Fusion of Foundation and Task-Specific Models for Cardiac MRI Segmentation},
  author={Isaba Rumman and others},
  year={2026},
  note={GitHub repository}
}

