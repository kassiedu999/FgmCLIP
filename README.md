# FgmCLIP: Patch Embedding-Anomaly Score Feedback Guidance Mechanism for Zero-Shot Industrial Anomaly Detection

This repository accompanies the manuscript:

> **Patch Embedding-Anomaly Score Feedback Guidance Mechanism for Zero-Shot Industrial Anomaly Detection**
>
> Submitted to *Computers and Electrical Engineering*; currently under review.

## News

- 2026-06-24: Submitted to Computers and Electrical Engineering
- The paper is currently under review. The code will be made available upon acceptance.

## Overview

FgmCLIP is a CLIP-based framework for zero-shot industrial anomaly detection. It uses patch-level anomaly scores to guide interactions among patch embeddings and their semantic aggregation, producing image-level representations for anomaly discrimination. A dual-path anomaly scoring loss jointly supervises image-level detection and pixel-level localization.

## Highlights

- **Anomaly score feedback-guided patch representation (AGR):** Fused patch anomaly scores guide Top-k patch selection, attention interactions, and cluster-level weighted aggregation to construct discriminative image-level representations.
- **Residual fusion and cluster-level aggregation:** AGR combines attention-enhanced features with a residual path, applies K-Means clustering, and weights regional cluster representations using their mean anomaly scores.
- **Dual-path anomaly scoring loss (DPL):** Image-level binary cross-entropy loss and pixel-level Focal and Dice losses jointly supervise anomaly detection and localization.
- **Separate image-level and pixel-level outputs:** AGR operates on image-level representation construction. The pixel-level anomaly score map is generated from multi-layer patch-text similarities and supervised by pixel-level losses.
- **Seven industrial benchmarks:** Evaluation covers MVTec-AD, VisA, MPDD, BTAD, SDD, DAGM, and DTD-Synthetic under a cross-dataset zero-shot protocol.

## Main Results

The following results are transcribed from Tables 2-5 of the manuscript. All values are percentages (%); higher is better. Results are reported as mean ± standard deviation over five independent runs with seeds 111-115.

### FgmCLIP on Seven Industrial Benchmarks

| Dataset | Image-AUROC ↑ | Image-AP ↑ | Pixel-AUROC ↑ | Pixel-F1-max ↑ |
|:--|:--:|:--:|:--:|:--:|
| MVTec-AD | 91.6 ± 0.2 | 96.1 ± 0.1 | 90.2 ± 0.1 | 43.5 ± 0.1 |
| VisA | 87.1 ± 0.1 | 89.2 ± 0.1 | 95.9 ± 0.2 | 37.1 ± 0.1 |
| MPDD | 74.3 ± 0.4 | 79.8 ± 0.5 | 96.4 ± 0.3 | 33.4 ± 0.4 |
| BTAD | 90.6 ± 0.2 | 95.6 ± 0.2 | 92.9 ± 0.3 | 47.8 ± 0.4 |
| SDD | 96.5 ± 0.1 | 90.4 ± 0.2 | 97.8 ± 0.1 | 48.7 ± 0.2 |
| DAGM | 98.4 ± 0.3 | 93.7 ± 0.3 | 94.9 ± 0.2 | 62.9 ± 0.4 |
| DTD-Synthetic | 97.4 ± 0.3 | 98.6 ± 0.3 | 98.9 ± 0.2 | 73.6 ± 0.4 |
| **Average** | **90.8 ± 0.2** | **91.9 ± 0.2** | **95.3 ± 0.1** | **49.6 ± 0.3** |

### Average Performance Comparison

The table below reports the seven-dataset averages for selected reproduced baselines and FgmCLIP under the evaluation settings described in the manuscript.

| Method | Image-AUROC ↑ | Image-AP ↑ | Pixel-AUROC ↑ | Pixel-F1-max ↑ |
|:--|:--:|:--:|:--:|:--:|
| APRIL-GAN | 81.6 ± 0.2 | 82.0 ± 0.3 | 84.9 ± 0.3 | 47.2 ± 0.3 |
| AnomalyCLIP | 87.8 ± 0.3 | 88.6 ± 0.2 | 94.5 ± 0.3 | 40.5 ± 0.3 |
| AdaCLIP | 90.2 ± 0.3 | 89.9 ± 0.3 | 94.2 ± 0.3 | 48.2 ± 0.2 |
| **FgmCLIP** | **90.8 ± 0.2** | **91.9 ± 0.2** | **95.3 ± 0.1** | **49.6 ± 0.3** |

For each dataset, metrics are computed separately for each target category and then averaged across categories. The Average row summarizes performance across the seven datasets. Full baseline comparisons and ablation studies are reported in the manuscript.

## Experimental Setup

### 1. Cross-Dataset Zero-Shot Protocol

In each experiment, the model is trained on a single source-domain dataset and evaluated on an independent target-domain dataset. Target-domain samples are not used for training.

| Source Dataset | Target Datasets |
|:--|:--|
| MVTec-AD | VisA, MPDD, BTAD, SDD, DAGM, DTD-Synthetic |
| VisA | MVTec-AD |

Auxiliary training uses the images and annotations from the entire official test split of the source-domain dataset, including normal and abnormal images. Image labels provide image-level supervision, and abnormal-image masks provide pixel-level supervision; normal images use all-zero masks. Target-domain labels and masks are used for evaluation only.

No independent validation set is used. As described in the manuscript, default hyperparameters are selected based on test performance on VisA and MVTec-AD and then held fixed for the final comparisons across all target datasets, without further dataset-specific adjustments.

### 2. Backbone and Training Configuration

| Item | Setting |
|:--|:--|
| Backbone | Pretrained CLIP ViT-L/14@336px |
| Input resolution | 518 × 518 pixels |
| Selected visual feature layers | 6, 12, 18, 24 |
| Patch embedding projection | 1024 → 768 dimensions per selected layer |
| Prompt depth | 4 |
| Prompt length | 6 |
| Top-k patches | 100 |
| Number of clusters | 20 |
| Guidance strength | 0.8 |
| Optimizer | AdamW |
| Learning rate | 0.01 |
| Training epochs | 8 |
| Framework | PyTorch 2.3.1 with CUDA 12.1 |
| GPU | NVIDIA RTX 3090, 24 GB |

The backbone name identifies the pretrained CLIP variant; the input resolution used in this study is 518 × 518 pixels.

## Code and Model Release

The implementation is not yet publicly available. The code will be made available upon acceptance.

Installation instructions, dataset preparation scripts, training and evaluation commands, and checkpoint information will be documented alongside the code release.

## Citation

The manuscript is currently under review. Citation details will be updated following acceptance.

## Acknowledgements

We thank the authors of [CLIP](https://github.com/openai/CLIP), [AnomalyCLIP](https://github.com/zqhang/AnomalyCLIP), and [AdaCLIP](https://github.com/caoyunkang/AdaCLIP) for making their implementations publicly available.
