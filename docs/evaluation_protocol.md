# Evaluation Protocol

## 1. Overview

This document describes the evaluation protocol used to assess the performance and generalization capability of the proposed reconstruction-based CNN-LSTM Autoencoder for underwater anomaly detection.

The evaluation focuses on two main aspects:

- Within-scene anomaly detection performance.
- Cross-scene generalization under different marine environments.

The experiments use the AURA underwater video dataset, which contains videos from two different marine scenes.

## 2. Dataset Splitting and Leakage Prevention

The dataset is divided at the video level rather than randomly splitting individual frames.

This approach helps prevent frames from the same video from appearing in both training and testing sets, reducing the risk of data leakage.

The dataset preparation pipeline includes:

- Video-level training, validation, and testing splits.
- Scene assignment checks.
- Frame assignment validation.
- Label coverage analysis.
- Leakage verification.

The split manifests and leakage-check outputs are used to verify the integrity of the evaluation process.

The random seed is set to 42 to support reproducibility.

## 3. Evaluation Scenarios

Four primary evaluation scenarios are used.

| Experiment | Training Scene | Testing Scene | Evaluation Type |
|---|---|---|---|
| A → A | Scene A | Scene A | Same-scene |
| B → B | Scene B | Scene B | Same-scene |
| A → B | Scene A | Scene B | Cross-scene |
| B → A | Scene B | Scene A | Cross-scene |

### 3.1 Same-Scene Evaluation

In same-scene evaluation, the model is trained and tested using videos from the same marine environment.

- A → A evaluates performance within Scene A.
- B → B evaluates performance within Scene B.

These experiments establish the baseline performance when the training and testing environments are similar.

### 3.2 Cross-Scene Evaluation

Cross-scene evaluation investigates whether the model can generalize to a different underwater environment.

- A → B trains on Scene A and tests on Scene B.
- B → A trains on Scene B and tests on Scene A.

These experiments are important for measuring the effect of scene variation and domain shift on anomaly detection performance.

## 4. Baseline Model Evaluation

The baseline consists of a reconstruction-based CNN-LSTM Autoencoder trained using normal video frames.

Two scene-specific models are trained:

- Scene A model: trained using normal training videos from Scene A.
- Scene B model: trained using normal training videos from Scene B.

The Scene A model is evaluated on A → A and A → B.

The Scene B model is evaluated on B → B and B → A.

During inference, the model reconstructs input temporal windows and calculates reconstruction error.

The mean squared reconstruction error across each window is used as its anomaly score. The score is assigned to the last frame of the corresponding window.

The best validation checkpoint is used for evaluation.

## 5. Evaluation Metrics

The following metrics are used to assess anomaly detection performance.

### 5.1 Area Under the Receiver Operating Characteristic Curve (AUROC)

AUROC measures the ability of the model to distinguish between normal and anomalous samples across different classification thresholds.

A higher AUROC indicates better discrimination between the two classes.

### 5.2 Average Precision (AP)

Average Precision summarizes the precision-recall curve and measures the model's ability to identify anomalous samples while accounting for precision and recall.

It is particularly useful when anomaly labels are imbalanced.

### 5.3 Precision

Precision measures the proportion of predicted anomalous samples that are actually anomalous.

\[
Precision = \frac{TP}{TP+FP}
\]

### 5.4 Recall

Recall measures the proportion of actual anomalous samples correctly identified by the model.

\[
Recall = \frac{TP}{TP+FN}
\]

### 5.5 F1-Score

The F1-score is the harmonic mean of precision and recall.

\[
F1 = 2 \times
\frac{Precision \times Recall}
{Precision + Recall}
\]

It provides a combined measure of precision and recall.

## 6. Mixed-Scene Evaluation

An additional experiment investigates whether training on data from both marine scenes improves generalization.

A single mixed-scene model is trained using normal training videos from Scene A and Scene B.

The mixed-scene model is compared against the baseline using comparable test windows.

For cross-scene comparisons, videos used by the mixed model during training or validation are excluded from the corresponding test comparisons to prevent data leakage.

The evaluation includes:

- A → A
- B → B
- A → B
- B → A

The results are compared using the same evaluation metrics.

## 7. Post-processing Evaluation

Post-processing experiments investigate whether anomaly score processing can improve detection performance.

The evaluated approaches include:

- Raw anomaly scores.
- Temporal smoothing.
- Per-video score normalization.
- Per-video normalization combined with temporal smoothing.
- V2 processing using Top-5 patches, normalization, and smoothing.

The raw baseline results are retained as the primary reference, while post-processing results are reported as supplementary experiments.

Comparisons are conducted across the four evaluation scenarios to investigate whether post-processing affects within-scene and cross-scene performance differently.

## 8. Reproducibility

The implementation includes deterministic configuration settings to improve reproducibility.

The reproducibility procedure includes:

- Setting random seeds for Python, NumPy, and PyTorch.
- Configuring deterministic operations where supported.
- Saving the best validation checkpoints.
- Recording baseline configuration settings.
- Reloading saved checkpoints and repeating inference.
- Comparing the resulting anomaly scores.

A reproducibility check is performed by comparing anomaly scores generated before and after checkpoint reloading.

## 9. Statistical Analysis

Bootstrap confidence intervals are used to estimate the uncertainty of the evaluation results.

The statistical analysis uses 1,000 video-level bootstrap resamples and reports 95% confidence intervals for AUROC.

Video-level resampling is used to account for the fact that frames and temporal windows from the same video are not independent observations.

The confidence intervals are interpreted cautiously because the number of independent test videos is limited.

## 10. Evaluation Objectives

The evaluation protocol is designed to answer the following research questions:

1. How effectively does the CNN-LSTM Autoencoder detect anomalies within the same marine scene?
2. How does performance change when the model is evaluated on a different marine scene?
3. Does training on both scenes improve cross-scene generalization?
4. Can post-processing improve anomaly detection performance?
5. How stable and reproducible are the obtained results?

These experiments provide a systematic assessment of the model's performance and its ability to generalize across different underwater environments.