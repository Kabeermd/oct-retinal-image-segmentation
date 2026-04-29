# OCT Retinal Image Segmentation

Retinal cyst detection in Optical Coherence Tomography (OCT) images using classical computer vision — no deep learning. Achieves **80.20% Dice coefficient** with K-means clustering.

## Overview

This project implements and evaluates a full segmentation pipeline on 10 OCT retinal images. Two unsupervised segmentation methods are compared: Otsu thresholding and K-means clustering, both preceded by a multi-stage preprocessing pipeline.

## Preprocessing Pipeline

```
Raw OCT Image → Median Filter → Bilateral Filter → CLAHE → Normalisation
```

| Step | Method | Purpose |
|------|--------|---------|
| 1 | Median Filter (5×5) | Remove salt-and-pepper noise |
| 2 | Bilateral Filter (σ_color=0.1, σ_spatial=15) | Edge-preserving smoothing |
| 3 | CLAHE (clip_limit=0.02) | Contrast-limited adaptive histogram equalisation |
| 4 | Min-max normalisation | Scale pixel values to [0, 1] |

## Segmentation Methods

**Otsu Thresholding** — Automatic global threshold selection; pixels below threshold classified as cyst regions, followed by morphological post-processing (remove small objects, close holes).

**K-Means Clustering** (k=2) — Pixel-level clustering into cyst/background; lower-intensity cluster assigned as cyst, followed by small-object removal and binary hole filling.

## Results (10 OCT images)

| Algorithm | Dice ± Std | IoU ± Std | Precision | Recall |
|-----------|-----------|----------|-----------|--------|
| Otsu | 0.7954 ± 0.0317 | 0.6614 ± 0.0432 | 0.9824 | 0.6695 |
| **K-means** | **0.8020 ± 0.0303** | **0.6705 ± 0.0409** | 0.9827 | 0.6787 |

K-means marginally outperforms Otsu across all metrics. Both methods achieve high precision (~98%) with moderate recall (~67–68%), indicating conservative segmentation with few false positives.

## Files

| File | Description |
|------|-------------|
| `oct_retinal_segmentation.ipynb` | Full preprocessing, segmentation, evaluation, and visualisation pipeline |
| `AssingmentReport_Segmentation.pdf` | Detailed project report |

## Tech Stack

- Python 3
- scikit-image (filters, morphology, exposure, segmentation)
- scikit-learn (KMeans, metrics)
- OpenCV, NumPy, Matplotlib, seaborn
- Google Colab
