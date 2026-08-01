<div align="center">

# 🫁 COPD Audio Progression Pipeline
**A Multi-Stage Hierarchical Classification System for Respiratory Sounds**

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)](https://www.tensorflow.org/)
[![Paper](https://img.shields.io/badge/Published_in-ICBCB_2026_Japan-purple.svg)]()
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

</div>

<br>

## 📌 Overview

This repository contains the official codebase and pre-trained expert models for **COPD Audio Progression**, an advanced diagnostic pipeline designed to classify the severity stages of Chronic Obstructive Pulmonary Disease (COPD) using raw respiratory audio signals.

Our research proposes a two-step, multi-stage hierarchical classification system that mimics a clinical diagnostic process. By segmenting the problem into granular stages (Healthy vs. Early Stage, Stage 1 vs. 2, Stage 2 vs. 3, and Stage 3 vs. 4), the pipeline achieves significantly higher confidence and accuracy than single monolithic models.

---

## 🧠 Data Processing & Training Methodology

To achieve clinical-grade accuracy, our models do not analyze raw `.wav` audio directly. Instead, we use advanced digital signal processing to extract meaningful features from the lung sounds before feeding them into our deep learning architectures (like CNNs and ResNets).

### 1. Audio Preprocessing
When an audio file is passed into our pipeline, it undergoes standard clinical preprocessing:
*   **Loading & Resampling:** The audio is loaded using `librosa` and resampled to a standardized frequency (typically 16,000 Hz) to ensure consistency across different electronic stethoscopes.
*   **Noise Reduction (Optional):** Ambient background noise is filtered out to isolate the acoustic lung cycle.

### 2. Feature Extraction
Once the audio is clean, we extract two primary types of acoustic features, depending on the specific expert model being used:
*   **Log-Mel Spectrograms (`n_mels=128` or `n_mels=20`):** We convert the audio into a visual representation of sound frequencies over time. The Log-Mel scale mimics how the human ear (and standard stethoscopes) perceive sound. The raw spectrogram is converted to decibels (`power_to_db`) and fed into 2D Convolutional Neural Networks (CNNs) and ResNet architectures.
*   **MFCCs (Mel-Frequency Cepstral Coefficients):** For certain boundaries, we extract 20 MFCCs (`n_mfcc=20`), pad them to a maximum uniform length, and normalize them using pre-calculated training means and standard deviations. This creates a dense, normalized 2D matrix used for fine-grained prediction.

### 3. Training the Models
The extracted features (Spectrograms/MFCCs) are used to train independent, specialized expert models. Instead of training one massive model to predict all 5 stages, we train:
*   A **Base Classifier** to learn global COPD patterns.
*   **Boundary Expert Models** (e.g., a specific ResNet trained *only* on Stage 3 vs Stage 4 data) to learn the minute acoustic differences at the clinical boundary lines.

---

## 🔬 The 2-Step Diagnostic Flow

Our pipeline mirrors clinical evaluation by first establishing a baseline severity, and then assessing the probability of progression.

### Step 1: Base Stage Classification
**Primary Script:** `classifier.ipynb`

In this initial phase, raw audio data is processed (extracting the Log-Mel Spectrograms and MFCCs) and evaluated to determine the patient's current baseline severity stage. 

### Step 2: Progression Prediction
**Primary Folders:** `COPD 0-1/`, `COPD 1-2/`, `COPD 2-3/`, `COPD 3-4/`

Once the baseline stage is classified, the pipeline invokes the corresponding **Expert Model**. 
For example, if the patient is classified as Stage 1, the pipeline dynamically loads the models from the `COPD 1-2/` folder. This expert model is highly specialized to detect the subtle acoustic boundary between Stage 1 and Stage 2, predicting the probabilistic chances of the patient progressing to the next stage.

*(Note: For a fully automated end-to-end wrapper of this 2-step methodology, see `run_full_pipeline.ipynb`.)*

---

## 📂 Comprehensive File Glossary

We maintain this repository mirroring professional deep learning lab standards. Here is exactly what each component does:

### 1. Core Executables & Configuration
*   **`classifier.ipynb`**: The primary research notebook for Step 1. It handles raw `.wav` loading, visualizes the audio waveforms and spectrograms, and computes the base classification.
*   **`run_full_pipeline.ipynb`**: The automated script that strings Step 1 and Step 2 together. It takes a Patient ID, processes their audio, polls the expert models, and outputs a complete text-based Diagnostic Report.
*   **`requirements.txt`**: The exact Python environment dependencies (e.g., `librosa`, `tensorflow`, `numpy`) needed to reproduce our results without version conflicts.

### 2. Expert Model Directories (`COPD X-Y/`)
Each directory contains specialized `.keras` models, Jupyter Notebooks for training/evaluation, and normalizer statistics (`.npy`) tuned specifically for binary boundary classification.
*   **`COPD 0-1/`**: Contains the model determining Healthy vs. Early Stage (Stage 1).
*   **`COPD 1-2/`**: Contains the model determining progression from Stage 1 to Stage 2. *(Includes specific Log-Mel mean/std normalizers).*
*   **`COPD 2-3/`**: Contains the model determining progression from Stage 2 to Stage 3.
*   **`COPD 3-4/`**: Contains the model determining progression from Stage 3 to Stage 4 (Severe). Includes a specialized ResNet (`resnet_copd3_vs_4.keras`).
*   **`models/`**: A centralized repository of the combined `.keras` weights and `.npy` scalers used globally by the automated `run_full_pipeline` script.

### 3. Data & Ground Truth Metadata
*   **`RespiratoryDatabase@TR/`**: A small, clean sample directory of testing `.wav` files included directly in this repo for immediate testing and verification out-of-the-box.
*   **`archive/`**: The complete 2.1 GB dataset of over 1,840 respiratory audio files and textual metadata.
*   **`balanced_labels.csv` & `Labels.xlsx`**: The clinical ground-truth metadata mapping patient IDs to their respective diagnoses and respiratory cycle events.

---

## 🛠️ Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/punithbr27/COPD-Research-project.git
   cd COPD-Research-project
   ```

2. **Set up a virtual environment (Recommended):**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use: venv\Scripts\activate
   ```

3. **Install the dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

---

## 💻 How to Run

### Manual Evaluation (Research Mode)
1. Open `classifier.ipynb` in Jupyter Notebook.
2. Provide a path to a single `.wav` file to visualize the Log-Mel Spectrogram and compute its base classification.
3. Open the corresponding Jupyter Notebook located inside the `COPD X-Y/` folders (e.g., `model.ipynb`) to evaluate the exact probability of progression.

### Automated Evaluation (Clinical Mode)
1. Open `run_full_pipeline.ipynb` in Jupyter Notebook.
2. Set the `PATIENT_ID_TO_ANALYZE` variable (e.g., `"221"`).
3. Run the notebook to generate a definitive text report that internally utilizes the 2-step methodology and expert model polling.

---

## 📖 Citation

If you use this codebase, methodology, or pre-trained models in your research, please cite our paper:

```bibtex
@inproceedings{punith2026copd,
  title={COPD Audio Progression: A Multi-Stage Hierarchical Classification System},
  author={Punith B. R.},
  booktitle={Proceedings of the 2026 International Conference on Bioinformatics and Computational Biology (ICBCB)},
  year={2026},
  address={Japan}
}
```

## 📜 License

This project is released under the [MIT License](LICENSE).

---
*Developed as part of cutting-edge computational biology research in respiratory sound analysis.*
