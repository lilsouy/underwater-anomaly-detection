# Experiments Plan

## 1. Project Objective

The objective of the experiments is to evaluate the robustness and cross-scene generalization of a reconstruction-based CNN-LSTM Autoencoder for underwater anomaly detection.

The experiments investigate whether training on one marine scene or multiple scenes affects anomaly detection performance when evaluated across different underwater environments.

## 2. Experimental Setup

The experiments use the AURA dataset, which contains videos from two different marine scenes:

- Scene A
- Scene B

The baseline model is trained using normal video frames and evaluated using reconstruction-based anomaly scores.

The experimental framework includes four evaluation scenarios:

| Experiment | Training Scene | Testing Scene | Status |
|---|---|---|---|
| A → A | Scene A | Scene A | Completed |
| B → B | Scene B | Scene B | Completed |
| A → B | Scene A | Scene B | Completed |
| B → A | Scene B | Scene A | Completed |

## 3. Baseline Experiments

The baseline experiments establish the performance of the CNN-LSTM Autoencoder under same-scene and cross-scene evaluation settings.

Two scene-specific models are trained:

- Model A is trained using normal videos from Scene A.
- Model B is trained using normal videos from Scene B.

The models are evaluated across the four experimental scenarios.

The baseline evaluation includes AUROC, Average Precision, Precision, Recall, and F1-score.

## 4. Reproducibility Experiments

Checkpoint reproducibility is evaluated by reloading saved model checkpoints and repeating inference.

The resulting anomaly scores are compared with the original scores.

All four baseline evaluation scenarios passed the reproducibility check.

| Experiment | Number of Windows | Maximum Score Difference |
|---|---:|---:|
| A → A | 4,277 | 3.72 × 10⁻⁹ |
| A → B | 2,203 | 3.72 × 10⁻⁹ |
| B → B | 954 | 8.60 × 10⁻¹¹ |
| B → A | 9,506 | 7.45 × 10⁻⁹ |

## 5. Mixed-Scene Experiments

A mixed-scene model is trained using normal training videos from both Scene A and Scene B.

Its performance is compared with the baseline model using comparable test windows.

The main objective is to investigate whether exposure to multiple marine environments improves cross-scene generalization.

The experiment includes all four evaluation scenarios:

- A → A
- B → B
- A → B
- B → A

## 6. Post-processing Experiments

Additional experiments investigate the effect of post-processing on anomaly detection scores.

The evaluated configurations include:

1. Raw anomaly scores.
2. Temporal smoothing.
3. Per-video score normalization.
4. Per-video normalization combined with temporal smoothing.
5. V2 processing using Top-5 patches, normalization, and smoothing.

The results are compared against the original baseline to evaluate whether post-processing improves anomaly detection performance.

## 7. Statistical Analysis

Bootstrap analysis is conducted using 1,000 video-level resamples.

The analysis reports 95% confidence intervals for AUROC.

This experiment provides an estimate of uncertainty in the observed results. However, the limited number of independent test videos requires cautious interpretation.

## 8. Remaining Tasks

The following tasks should be checked before final project submission:

| Task | Status |
|---|---|
| Verify all experiment outputs are saved | Pending verification |
| Finalize evaluation result tables | Pending verification |
| Complete false-positive and false-negative analysis | Pending |
| Generate anomaly-score visualizations against ground truth | Pending |
| Verify Temporal Event Overlap implementation | Pending |
| Finalize mathematical induction proof | Pending |
| Verify GitHub repository organization | In progress |
| Complete final reproducibility documentation | Pending review |

## 9. Expected Final Deliverables

The final experimental deliverables include:

- Dataset preparation outputs.
- Baseline experiment results.
- Mixed-scene experiment results.
- Post-processing comparisons.
- Reproducibility verification.
- Bootstrap confidence intervals.
- Evaluation figures and result tables.
- Mathematical induction proof.
- Organized source code and documentation repository.

## 10. Conclusion

The experimental framework evaluates the baseline model under within-scene and cross-scene settings, investigates mixed-scene training, and examines supplementary post-processing methods.

The remaining work focuses on completing the analysis, verifying all saved outputs, and preparing the final reproducible project repository.