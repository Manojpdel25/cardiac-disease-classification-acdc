# Cardiac Disease Classification from Cine MRI — ACDC Benchmark

Controlled comparison of image-based CNNs, segmentation-guided CNNs, ground-truth mask CNN baselines, and feature-based classifiers for five-class cardiac disease classification on the ACDC benchmark.(https://www.creatis.insa-lyon.fr/Challenge/acdc/).

This repository contains the full implementation for the MSc thesis:  
**"Comparing Image-Based Deep Learning, Segmentation-Guided Deep Learning, and Feature-Based Classifiers for Five-Class Cardiac Disease Classification from Cine Cardiac MRI"**  
OsloMet — Oslo Metropolitan University, 2026

## Overview

The thesis compares four model families and fifteen classification models under identical dataset splits, preprocessing, and evaluation:
| Family | Models | Input |
|--------|--------|-------|
| Non-segmentation-guided CNN | M1–M3 | ED image, ES image, or ED+ES image fusion |
| Segmentation-guided CNN | M4–M6 | U-Net masked ED, ES, or ED+ES MRI slices |
| Feature-based classifiers | M7–M12 | 7 clinical volumetric features from predicted and GT masks |
| Ground-truth mask CNN baselines | M13–M15 | Expert-masked ED, ES, or ED+ES MRI slices |

**Five diagnostic classes:** Normal (NOR), Myocardial Infarction (MINF), Dilated Cardiomyopathy (DCM), Hypertrophic Cardiomyopathy (HCM), Abnormal Right Ventricle (ARV)

## Repository Structure

```
├── data/                       # Data preprocessing and loading scripts
├── segmentation/
│   ├── training/               # U-Net segmentation training
│   └── inference/              # Mask prediction on ACDC test set
├── classification/
│   ├── cnn_models/             # All CNN classification notebooks (M1–M6, M13-M15)
│   ├── gt_baselines/           # Ground truth mask CNN baselines
│   └── feature_based/          # RF, SVM, MLP classifiers (M7–M12)
├── feature_extraction/         # Seven clinical feature extraction scripts
├── explainability/             # Smooth Grad-CAM++ analysis
├── results/                    # Saved results, metrics, and figures
├── configs/                    # Experiment configurations
└── README.md
```

## Pipeline

```
ACDC Dataset (100 train / 50 test)
        │
        ▼
┌───────────────────┐
│  U-Net Segmentation │  →  Predicted masks (LV, MYO, RV BG)
└───────────────────┘
        │
        ├──────────────────────────────────┐
        ▼                                  ▼
┌─────────────────────┐      ┌──────────────────────────┐
│  CNN Classification   │      │  Feature Extraction        │
│  (image-based /       │      │  (7 clinical measurements) │
│   guided / GT mask)   │      └──────────────────────────┘
└─────────────────────┘                    │
        │                                  ▼
        │                      ┌──────────────────────┐
        │                      │  RF / SVM / MLP        │
        │                      └──────────────────────┘
        │                                  │
        ▼                                  ▼
┌─────────────────────────────────────────────────┐
│  Evaluation: Accuracy,  Per-class Metrics     │
│  Explainability: Smooth Grad-CAM++                │
└─────────────────────────────────────────────────┘
```

## Seven Clinical Features

Extracted from segmentation masks at ED and ES phases:

1. Left Ventricular End-Diastolic Volume (LVEDV)
2. Left Ventricular End-Systolic Volume (LVESV)
3. Left Ventricular Ejection Fraction (LVEF)
4. Right Ventricular End-Diastolic Volume (RVEDV)
5. Right Ventricular End-Systolic Volume (RVESV)
6. Right Ventricular Ejection Fraction (RVEF)
7. Myocardial Mass

## Dataset

The [Automated Cardiac Diagnosis Challenge (ACDC)](https://www.creatis.insa-lyon.fr/Challenge/acdc/). The ACDC dataset contains cine cardiac MRI scans from 150 patients across five diagnostic groups, with 30 patients per class. The official split includes 100 training patients and 50 held-out test patients.

- **Training:** 100 patients (4-fold stratified cross-validation)
- **Testing:** 50 patients (held-out)

The dataset is not included in this repository. Download it from the [ACDC website](https://www.creatis.insa-lyon.fr/Challenge/acdc/).

## Requirements

```
python >= 3.8
torch
torchvision
nibabel
numpy
scikit-learn
matplotlib
opencv-python
```

Install dependencies:
```bash
pip install -r requirements.txt
```

## Reproducibility

All experiments use a fixed 4-fold stratified cross-validation split on the 100 training patients. The same split is used across all model families. Random seeds are fixed for reproducibility.

## Evaluation Protocol

- **Metrics:** Overall accuracy, macro-averaged AUC, per-class precision/recall/F1
- **Test set:** 50 held-out ACDC patients
- **Cross-validation:** 4-fold stratified on 100 training patients

## Explainability

Smooth Grad-CAM++ visualisations are generated for the best-performing CNN model to show which image regions contribute to classification decisions.

## Citation

If you use this code in your research, please cite:

```bibtex
@mastersthesis{poudel2026cardiac,
  title   = {Comparing Image-Based Deep Learning, Segmentation-Guided Deep Learning, and Feature-Based Classifiers for Five-Class Cardiac Disease Classification from Cine Cardiac MRI},
  author  = {Poudel, Manoj},
  school  = {OsloMet -- Oslo Metropolitan University},
  year    = {2026},
  type    = {MSc Thesis}
}
```

## Affiliated Project

This work is part of the [SEARCH project](https://www.search-project.eu/), which supports trustworthy and reproducible research using health data.

## Supervisors

- Vajira Thambawita
- Pål Halvorsen

## License

This project is released for academic and research purposes.
