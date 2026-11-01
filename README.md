# Robust Underwater Anomaly Detection Across Different Marine Scenes

## 1. Project Overview

This project investigates the robustness and cross-scene generalization of deep learning models for underwater anomaly detection.

The main objective is to evaluate whether a reconstruction-based CNN-LSTM Autoencoder can detect anomalous events across different underwater marine environments.

The project focuses on the impact of scene variation on anomaly detection performance and investigates whether mixed-scene training and score post-processing can improve generalization.

## 2. Research Objectives

- Evaluate anomaly detection performance within the same marine scene.
- Investigate cross-scene generalization between different underwater environments.
- Compare scene-specific training with mixed-scene training.
- Evaluate the effect of anomaly-score post-processing.
- Assess reproducibility and uncertainty through checkpoint verification and bootstrap analysis.

## 3. Dataset

The project uses the AURA underwater anomaly detection dataset.

The dataset contains:

- 25 underwater videos.
- 15,083 frames.
- Two different marine scenes.
- Annotations from 16 annotators per video.

### Scene Distribution

| Scene | Videos | Frames | Frame Rate |
|---|---:|---:|---:|
| Scene A | 10 | 12,524 | 30 FPS |
| Scene B | 15 | 2,559 | 15 FPS |
| Total | 25 | 15,083 | — |

Dataset preparation includes video-level splitting, frame preprocessing, label handling, and leakage checks.

## 4. Model Architecture

The baseline model is a reconstruction-based CNN-LSTM Autoencoder.

### Main Components

- **Input:** RGB video frames resized to 112 × 112.
- **Sequence length:** 16 frames.
- **CNN:** Extracts spatial features using channels [32, 64, 128, 128].
- **Feature dimension:** 256.
- **LSTM:** One layer with 256 hidden units.
- **Reconstruction loss:** Mean Squared Error (MSE).

The model is trained using normal video frames. Reconstruction error is used as the anomaly score.

## 5. Experimental Design

Four evaluation scenarios are used:

| Experiment | Training | Testing |
|---|---|---|
| A → A | Scene A | Scene A |
| B → B | Scene B | Scene B |
| A → B | Scene A | Scene B |
| B → A | Scene B | Scene A |

These experiments evaluate both within-scene performance and cross-scene generalization.

### Additional Experiments

**Mixed-Scene Training**

A model is trained using normal training videos from both scenes and compared with the scene-specific baseline.

**Post-processing**

The project evaluates temporal smoothing, per-video normalization, and a V2 configuration using Top-5 patches.

**Statistical Analysis**

Bootstrap analysis with 1,000 video-level resamples is used to estimate 95% confidence intervals for AUROC.

## 6. Evaluation Metrics

The evaluation uses:

- AUROC
- Average Precision (AP)
- Precision
- Recall
- F1-score

Additional analysis includes reproducibility checks and bootstrap confidence intervals.

## 7. Repository Structure

```text
underwater-anomaly-detection/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   ├── dataset_preparation.ipynb
│   ├── main_experiments.ipynb
│   └── lstm_mathematical_verification.ipynb
│
├── docs/
│   ├── architecture.md
│   ├── evaluation_protocol.md
│   ├── experiments_plan.md
│   └── mathematical_induction_proof.pdf
│
├── results/
│   ├── dataset/
│   ├── baseline/
│   ├── mixed_scene/
│   ├── postprocessing/
│   └── statistical_analysis/
│
└── figures/
    └── architecture_diagram.png
```

Some result files and documentation are expected deliverables and should be added when their actual outputs are available.

## 8. Notebooks

| Notebook | Description |
|---|---|
| `dataset_preparation.ipynb` | Dataset exploration, preprocessing, splitting, and leakage checks. |
| `main_experiments.ipynb` | Baseline, mixed-scene, post-processing, and evaluation experiments. |
| `lstm_mathematical_verification.ipynb` | Numerical and mathematical verification of LSTM operations. |

## 9. Reproducibility

The experimental pipeline uses deterministic settings and fixed random seeds.

Model checkpoints are saved and reloaded to verify inference reproducibility.

The notebook configurations document the model architecture, training parameters, and experimental settings.

Refer to `docs/evaluation_protocol.md` for the detailed evaluation methodology.

## 10. Documentation

Additional project documentation:

- `docs/architecture.md` — Model architecture and training configuration.
- `docs/evaluation_protocol.md` — Dataset splitting, evaluation scenarios, metrics, and statistical analysis.
- `docs/experiments_plan.md` — Completed experiments and remaining tasks.
- `docs/mathematical_induction_proof.pdf` — Formal mathematical induction proof.

## 11. Installation

Clone the repository:

```bash
git clone https://github.com/Lilsouy/underwater-anomaly-detection.git
cd underwater-anomaly-detection
```

Install the dependencies:

```bash
pip install -r requirements.txt
```

Open the notebooks using Jupyter Notebook, JupyterLab, or Google Colab.

**Note:** The dependency list and exact versions should match the environment used to run the experiments.

## 12. Current Project Status

The project includes dataset preparation, baseline experiments, cross-scene evaluation, mixed-scene experiments, post-processing, and reproducibility analysis.

Remaining work includes final verification of result files, completion of the remaining analysis, and preparation of the final project deliverables.

## 13. Research Focus

The central research question is:

**How does variation between underwater marine scenes affect the robustness and cross-scene generalization of reconstruction-based deep learning models for anomaly detection?**

## Team
Solaf Abdulaziz Alessa
Rayan Saleh Almanquri
Sara Abduallh Aldosary
Noor Ali Alshabeeb

## instructor
Abdulrahman Saleh Alayed