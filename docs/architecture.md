# Model Architecture

## 1. Overview

The proposed system uses a reconstruction-based CNN-LSTM Autoencoder for underwater video anomaly detection. It combines Convolutional Neural Networks (CNNs) for spatial feature extraction with Long Short-Term Memory (LSTM) networks for temporal sequence modeling.

The main objective is to learn the normal patterns of underwater scenes and identify potentially anomalous events through reconstruction errors. The system is also evaluated across different marine scenes to investigate its generalization capability under scene and domain shifts.

The architecture consists of four main stages:

1. Input Video Sequence
2. CNN Feature Extraction
3. LSTM Temporal Modeling
4. Reconstruction-Based Anomaly Scoring

## 2. Input Video Sequence

The system receives an underwater video represented as a sequence of consecutive RGB frames.

Each frame is resized to a spatial resolution of 112 × 112 pixels. The frames are grouped into fixed-length temporal sequences.

The main input configuration is:

| Parameter | Value |
|---|---|
| Input type | RGB video frames |
| Frame resolution | 112 × 112 |
| Sequence length | 16 frames |
| Input channels | 3 |
| Scene A frame stride | 2 |
| Scene B frame stride | 1 |

The frame stride is adjusted according to the original frame rate of each scene. Scene A uses a stride of 2 at 30 FPS, while Scene B uses a stride of 1 at 15 FPS.

This configuration produces approximately comparable temporal coverage for both scenes.

## 3. CNN Feature Extraction

The CNN component extracts spatial features from individual video frames.

Each frame passes through convolutional layers followed by ReLU activation and spatial pooling operations. These operations progressively extract higher-level visual representations while reducing spatial dimensions.

The CNN channel configuration is:

| CNN Stage | Number of Channels |
|---|---:|
| Stage 1 | 32 |
| Stage 2 | 64 |
| Stage 3 | 128 |
| Stage 4 | 128 |

The extracted visual representation is projected into a feature vector with a dimensionality of 256.

For a sequence containing 16 frames, the resulting feature representation is:

\[
X \in \mathbb{R}^{16 \times 256}
\]

This representation preserves the temporal ordering of the extracted frame-level features.

## 4. LSTM Temporal Modeling

The extracted feature sequence is passed to an LSTM network to model temporal dependencies between consecutive frames.

Unlike independent frame-level processing, the LSTM maintains a hidden state and a cell state that allow information from previous time steps to influence subsequent representations.

The LSTM configuration is:

| Parameter | Value |
|---|---:|
| Input feature dimension | 256 |
| Hidden dimension | 256 |
| Number of LSTM layers | 1 |
| Sequence length | 16 |

At each time step, the LSTM updates its internal states based on the current input and the previous states.

This enables the model to learn temporal patterns associated with normal underwater activities.

## 5. Reconstruction-Based Anomaly Detection

The model follows a CNN-LSTM Autoencoder architecture consisting of an encoder and a decoder.

The CNN encoder extracts spatial features from each input frame. The resulting feature sequence is processed by the LSTM encoder to capture temporal dependencies and generate a compact latent representation.

The LSTM decoder processes the latent representation to reconstruct the temporal feature sequence, while the CNN decoder reconstructs the original video frames.

During training, the model learns to reconstruct normal underwater video sequences using a Mean Squared Error (MSE) reconstruction objective.

### 5.1 Baseline Anomaly Scoring

At inference time, the reconstructed frames are compared with the original input frames.

The reconstruction loss is based on Mean Squared Error:

\[
L_{\mathrm{MSE}} =
\frac{1}{N}
\sum_{i=1}^{N}
(x_i-\hat{x}_i)^2
\]

Where:

- \(x_i\) represents an original input value.
- \(\hat{x}_i\) represents its reconstructed value.
- \(N\) represents the number of compared values.

The baseline anomaly score is calculated as the mean squared reconstruction error across each temporal window.

Each window-level anomaly score is assigned to the last frame of its corresponding temporal window.

A higher reconstruction error indicates a larger difference between the original and reconstructed input and may indicate anomalous behavior.

### 5.2 Supplementary V2 Post-processing

In addition to the baseline scoring approach, supplementary experiments investigate patch-based anomaly scoring and score post-processing.

The V2 processing pipeline includes:

1. Patch-based scoring using reconstruction error maps.
2. Top-K patch selection (K = 5).
3. Aggregation of selected patch scores.
4. Per-video score normalization.
5. Temporal smoothing.

These operations are evaluated as supplementary post-processing experiments and are not considered mandatory components of the baseline anomaly scoring method.

The baseline results are retained as the primary reference for evaluating the effect of these additional processing steps.

## 6. Training Configuration

The model is trained using normal AURA training frames.

The training configuration is summarized below:

| Parameter | Configuration |
|---|---|
| Model | CNN-LSTM Autoencoder |
| Training objective | Reconstruction |
| Loss function | Mean Squared Error (MSE) |
| Optimizer | Adam |
| Learning rate | 0.001 |
| Weight decay | 0.00001 |
| Batch size | 16 |
| Maximum epochs | 30 |
| Early stopping patience | 7 |
| Learning rate scheduler | ReduceLROnPlateau |
| Scheduler factor | 0.5 |
| Scheduler patience | 3 |
| Gradient clipping | 1.0 |
| Random seed | 42 |

The best validation checkpoint is retained for evaluation.

All dataset splits are performed at the video level to prevent frames from the same video from appearing across training, validation, and testing sets.

## 7. Evaluation Settings

The system is evaluated under four experimental settings to investigate both within-scene performance and cross-scene generalization.

### 7.1 Same-Scene Evaluation

The model is trained and evaluated using data from the same marine scene.

- A → A: Training, validation, and testing are performed using Scene A.
- B → B: Training, validation, and testing are performed using Scene B.

These experiments measure the model's ability to detect anomalies when the training and testing environments belong to the same scene.

### 7.2 Cross-Scene Evaluation

The model is trained using one marine scene and tested on another.

- A → B: Training on Scene A and testing on Scene B.
- B → A: Training on Scene B and testing on Scene A.

These experiments evaluate the model's generalization capability under changes in marine environments.

### 7.3 Mixed-Scene Evaluation

An additional experiment trains a single model using normal training videos from both Scene A and Scene B.

The mixed-scene model is evaluated against the baseline model using comparable test windows. Videos used during mixed-model training or validation are excluded from the corresponding cross-scene test comparisons to prevent data leakage.

### 7.4 Robustness Analysis

The experimental results are compared across different evaluation settings to investigate the effect of scene variation on model performance.

The analysis includes comparisons between baseline and mixed-scene models, as well as supplementary post-processing and statistical evaluation.

## 8. Summary

The proposed architecture combines spatial feature extraction and temporal sequence modeling within a reconstruction-based anomaly detection framework.

By evaluating the model under same-scene, cross-scene, and mixed-scene settings, the project investigates how effectively learned normal patterns transfer between different underwater environments.

The architecture and evaluation protocol are designed to support a systematic investigation of underwater anomaly detection and scene generalization.
