# Uncertainty-Aware Medical Image Classification

This project explores **predictive uncertainty estimation** in medical image classification using a convolutional neural network (CNN).  
Rather than focusing on accuracy alone, the project analyzes **model confidence, calibration, and selective prediction behavior**, which are critical for safety-critical domains such as healthcare.

The repository presents a **frozen baseline version (v1)** comparing:
- A deterministic CNN
- Monte Carlo (MC) Dropout inference for uncertainty estimation

---

## Motivation

Deep learning models often produce confident predictions even when they are wrong.  
In medical settings, this behavior is risky.

This project demonstrates how **uncertainty-aware inference** can:
- Identify unreliable predictions
- Improve decision safety via selective prediction
- Provide calibrated confidence estimates

The implementation is intentionally kept **clear and reproducible**, rather than overly complex.

---

## Dataset

- **DermaMNIST** (MedMNIST v2)
- 7-class dermatoscopic image classification
- RGB images
- Standard train / validation / test split

---

## Baseline Model

A convolutional neural network is trained using:
- Softmax output
- Categorical cross-entropy loss
- Standard deterministic inference

### Evaluation
- Test accuracy ≈ **0.74**
- Classification report (precision / recall / F1)
- Confusion matrix
- Training vs validation accuracy curves

The baseline achieves reasonable performance but provides **no uncertainty estimate** beyond raw softmax confidence.

---

## Monte Carlo Dropout

To estimate predictive uncertainty, **dropout is enabled at inference time** and multiple stochastic forward passes are performed.

- Number of MC samples: **T = 30**
- Predictive mean probabilities are computed
- **Predictive entropy** is used as the uncertainty measure

### Performance
- MC Dropout mean accuracy ≈ **0.74**
- Classification metrics remain comparable to the baseline

This confirms that uncertainty estimation does **not degrade predictive performance**.

---

## Uncertainty Analysis

### Confidence vs Correctness
Correct predictions tend to exhibit **higher confidence**, while incorrect predictions are concentrated at lower confidence values.

### Predictive Entropy
Incorrect predictions show **significantly higher entropy**, indicating increased epistemic uncertainty.

This separation demonstrates that uncertainty correlates with model error.

---

## Selective Prediction

Predictions are selectively accepted based on uncertainty.

- Low-uncertainty predictions are kept
- High-uncertainty predictions are rejected

At **50% coverage**:
- Selective accuracy exceeds **92%**
- Both entropy-based and confidence-based selection improve reliability

This shows how uncertainty can be used to **trade coverage for accuracy**.

---

## Calibration

A reliability diagram is computed using MC Dropout mean confidence.

- Expected Calibration Error (ECE): **≈ 0.01**
- Predictions closely follow the ideal calibration line

This indicates **well-calibrated confidence estimates**, which is essential for downstream decision-making.

---

## Qualitative Results

Visual inspection of samples shows:
- **Low-entropy predictions** correspond to clear, unambiguous lesions
- **High-entropy predictions** often involve ambiguous or atypical patterns and misclassifications

This aligns uncertainty estimates with intuitive visual difficulty.

---

## Summary of Findings

- MC Dropout enables uncertainty estimation without loss of accuracy
- Predictive entropy separates correct and incorrect predictions
- Selective prediction significantly improves reliability
- Confidence estimates are well calibrated

---

## Status

This repository represents a **frozen Version 1** focused on:
- Baseline comparison
- Interpretability
- Reproducibility

Further extensions (ensembles, distribution shift, OOD detection) are intentionally left for future work.
