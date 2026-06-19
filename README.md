# R-Peak Detection on MIT-BIH Arrhythmia Database

This repository contains implementations of various Digital Signal Processing (DSP) and Deep Learning algorithms for R-peak detection in Electrocardiogram (ECG) signals using the MIT-BIH Arrhythmia Database.

## Algorithms Implemented

### 1. Digital Signal Processing (DSP)
*   **Pan-Tompkins Algorithm**: The classic time-domain approach using bandpass filtering, differentiation, squaring, and moving window integration.
*   **Continuous Wavelet Transform (CWT)**: Uses `mexh` wavelet focusing on QRS energy band.
*   **Discrete Wavelet Transform (DWT)**: Uses decimated `sym4` wavelet with Multi-Resolution Analysis (inverse DWT) for precise temporal reconstruction.
*   **Stationary Wavelet Transform (SWT)**: Uses un-decimated `sym4` wavelet to preserve perfect time resolution.

### 2. Deep Learning
*   **Standard UNet**: Custom PyTorch UNet trained with a dynamically weighted Dice + Cross-Entropy loss.
*   **UNet++**: Advanced UNet with dense skip connections across semantic levels for sharper localization.
*   *(Note: Deep learning models were trained and tested using the rigorous De Chazal inter-patient split paradigm: DS1 for training, DS2 for testing).*

## Overall Performance Comparison

The following table compiles the global performance metrics evaluated across the dataset (using the standard AAMI 150ms tolerance window).

| Method                             |   Total Beats |     TP |   FP |   FN |   Sensitivity (%) |   +Predictivity (%) |   Error Rate (%) |   Avg Time/Record (s) |
|:-----------------------------------|--------------:|-------:|-----:|-----:|------------------:|--------------------:|-----------------:|----------------------:|
| Continuous Wavelet Transform (CWT) |        102941 | 101485 |  467 | 1456 |             98.59 |               99.54 |             1.87 |                  0.12 |
| Stationary Wavelet Transform (SWT) |        102941 | 101026 |  303 | 1915 |             98.14 |               99.70 |             2.15 |                  0.15 |
| Pan-Tompkins Algorithm             |        102941 |  99025 |  235 | 3916 |             96.20 |               99.76 |             4.03 |                  0.07 |
| Standard UNet                      |         49651 |  49609 | 2101 |   42 |             99.92 |               95.94 |             4.32 |                  2.60 |
| UNet++                             |         49651 |  49622 | 2314 |   29 |             99.94 |               95.54 |             4.72 |                  2.86 |
| Discrete Wavelet Transform (DWT)   |        102941 |  97512 |  342 | 5429 |             94.73 |               99.65 |             5.61 |                  0.11 |

### Metric Definitions
*   **TP (True Positives)**: Correctly identified R-peaks.
*   **FP (False Positives)**: Noise or artifacts incorrectly identified as R-peaks.
*   **FN (False Negatives)**: True R-peaks that the algorithm missed.
*   **Sensitivity (Se)**: $TP / (TP + FN)$ - Measures the proportion of actual peaks correctly identified.
*   **+Predictivity (+P)**: $TP / (TP + FP)$ - Measures the proportion of detected peaks that are truly peaks.
*   **Error Rate**: $(FP + FN) / Total Beats$ - Overall percentage of detection mistakes.

## Repository Structure
*   `pan-thomp.ipynb`: Pan-Tompkins Implementation.
*   `wavelet-cwt.ipynb`: CWT Implementation.
*   `wavelet.ipynb`: DWT and SWT Implementations.
*   `UNet_MITBIH.ipynb`: Standard UNet Implementation.
*   `UNet++_MITBIH.ipynb`: UNet++ Implementation.
*   `*_results.csv`: Per-record evaluation metrics for each method.
