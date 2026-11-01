# Robust Underwater Anomaly Detection Across Different Marine Scenes

## Project Overview
This project investigates the generalization of video anomaly detection models across different underwater marine scenes using the AURA dataset.

The main objective is to evaluate how scene variation affects anomaly detection performance and to compare within-scene and cross-scene evaluation settings.

## Dataset
- Dataset: AURA (Anomalous Underwater Reef Activity)
- Total videos: 25
- Total frames: 15,083
- Marine scenes: Scene A and Scene B
- Annotations: Human-annotated anomaly labels

The dataset is not included in this repository. Refer to the original dataset source and follow its license requirements.

## Method
The project uses a CNN-LSTM Autoencoder trained on normal video sequences. Reconstruction error is used as the anomaly score.

Additional experiments investigate:
- Mixed-scene training
- Patch-based anomaly scoring
- Per-video score normalization
- Temporal smoothing

## Evaluation Protocol
Four primary settings are considered:

1. A → A: Train and test on Scene A.
2. B → B: Train and test on Scene B.
3. A → B: Train on Scene A and test on Scene B.
4. B → A: Train on Scene B and test on Scene A.

Video-level splits are used to reduce data leakage.

## Evaluation Metrics
- AUROC
- Average Precision (AP)
- Precision
- Recall
- F1-score
- Spearman correlation with soft labels
- Video-level bootstrap confidence intervals

## Repository Structure
- `notebooks/`: Dataset preparation, model implementation, and experiments.
- `results/`: Saved evaluation outputs and result tables.
- `figures/`: Generated charts and visualizations.
- `configs/`: Experiment configurations.
- `docs/`: Architecture and evaluation documentation.

## Reproducibility
The project uses a fixed random seed of 42 in the dataset preparation and baseline pipeline. Refer to the notebooks for the experiment configurations and execution details.

## Team
Solaf Abdulaziz Alessa
Rayan Saleh Almanquri
Sara Abduallh Aldosary
Noor Ali Alshabeeb

## instructor
Abdulrahman Saleh Alayed