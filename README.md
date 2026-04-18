# Which Distribution Fits Best? Learning Mixture Models on MNIST

Comparing Gaussian, Laplacian, Bernoulli, and Student-t mixture models to test which distribution separates MNIST digit clusters best.

## Problem Statement

Our project asks one direct question: are Gaussians really the best choice for mixture modelling on MNIST, or can alternative distributions separate digit clusters better?

To answer this, we trained multiple mixture-model families under the same evaluation setup and compared both clustering quality and generated samples.

## Overview

We implemented four different mixture models and evaluated them using Accuracy (Purity), ARI, and NMI. The comparison was done on the same dataset pipeline so the results stay fair across methods.

From the current results, **Gaussian MM (GMM)** is the best overall model, with **Accuracy = 0.7689** and **NMI = 0.6206**.

## Methods Implemented

| # | Model | Type | Key Idea |
|---|---|---|---|
| 1 | Gaussian MM (GMM) | EM from scratch + sklearn | Baseline on PCA features |
| 2 | Laplacian MM (LMM) | EM from scratch | Sharper peak, robust to outlier-like points |
| 3 | Bernoulli MM (BMM) | EM from scratch | Binary pixel model for thresholded MNIST |
| 4 | Student-t MM (SMM) | EM from scratch | Heavy tails with learned nu_k per component |

## Results

| Model | Accuracy | ARI | NMI |
|---|---:|---:|---:|
| GMM | **0.7689** | **0.3592** | **0.6206** |
| LMM | 0.7071 | 0.2886 | 0.5385 |
| BMM | 0.7595 | 0.3285 | 0.5754 |
| SMM | 0.7318 | 0.3133 | 0.5667 |

## Findings

GMM gives the strongest separation in this pipeline, with the best score across all three clustering metrics.

Bernoulli MM is close on Accuracy, but still behind GMM overall. Student-t and Laplacian are more robust in theory, but in PCA-50 space the Gaussian assumption is already a strong fit.

So for this project setup, Gaussians are still the most reliable default.

## Repository Structure

```text
MNIST-Mixture-Models-Analysis/
├── data/                        # Preprocessed arrays + saved metrics
├── images/                      # Saved plots, confusion matrices, and comparison figures
├── 1_Preprocessing.ipynb        # PCA-50 dimensionality reduction + binary track
├── 2_Gaussian_MM.ipynb          # GMM baseline
├── 3_Laplacian_MM.ipynb         # Laplacian mixture model
├── 4_Bernoulli_MM.ipynb         # Bernoulli mixture model
├── 5_StudentT_MM.ipynb          # Student-t mixture model
├── 6_Final_comparison.ipynb     # Cross-model comparison + final verdict
└── README.md
```

## Implementation Notes

- All mixture models are implemented from scratch using NumPy/SciPy. sklearn is used only for PCA and KMeans initialisation.
- We used PCA-50 instead of PCA-100 due to runtime limits on our system. PCA-100 was taking much longer per model, while PCA-50 gave a practical speed-quality tradeoff.
- The originally proposed VAE was replaced with Bernoulli Mixture Model because our binarised MNIST setup matches Bernoulli assumptions better and supports clean EM updates.
- K=25 components were selected using BIC analysis in the GMM notebook instead of fixing K=10 at the start.

## Team: Blurry 8s

| Name | Roll Number |
|---|---|
| Kushal Mehta | 23110205 |
| Krina Paghadar | 24110169 |
| Kshitij Giri | 23110177 |
| Shivangi Thaker | 24110371 |
