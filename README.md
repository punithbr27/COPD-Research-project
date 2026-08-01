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

## 🔬 The 2-Step Methodology

Our pipeline mirrors clinical evaluation by first establishing a baseline severity, and then assessing the probability of progression.

### Step 1: Base Stage Classification
**Primary Script:** `classifier.ipynb`

In this initial phase, raw audio data is processed and classified to determine the patient's current baseline severity stage. This script extracts Log-Mel Spectrograms and utilizes our core generalized models to place the audio into a distinct COPD category.

### Step 2: Progression Prediction
**Primary Folders:** `COPD 0-1/`, `COPD 1-2/`, `COPD 2-3/`, `COPD 3-4/`

Once the baseline stage is classified, the pipeline invokes the corresponding **Expert Model**. 
For example, if the patient is classified as Stage 1, the pipeline dynamically loads the models from the `COPD 1-2/` folder. This expert model is highly specialized to detect the subtle acoustic boundary between Stage 1 and Stage 2, predicting the probabilistic chances of the patient progressing to the next stage.

*(Note: For a fully automated end-to-end wrapper of this 2-step methodology, see `run_full_pipeline.ipynb`.)*

---

## 📂 Project Structure & File Glossary

We maintain this repository mirroring professional deep learning lab standards. Here is exactly what each component does:

### 1. Root Code & Configuration
- **`classifier.ipynb`**: The primary research notebook for Step 1. It visualizes the audio waveforms/spectrograms and runs the baseline stage classification.
- **`run_full_pipeline.ipynb`**: The automated script that strings Step 1 and Step 2 together, outputting a complete text-based Diagnostic Report for a given patient.
- **`requirements.txt`**: The exact environment dependencies needed to reproduce our results.

### 2. Expert Model Directories
Each directory contains specialized `.keras` models and normalizer statistics (`.npy`) tuned specifically for binary boundary classification.
- **`COPD 0-1/`**: Contains the model determining Healthy vs. Early Stage (Stage 1).
- **`COPD 1-2/`**: Contains the model determining progression from Stage 1 to Stage 2. *(Note: This utilizes a distinct 128-Mel config).*
- **`COPD 2-3/`**: Contains the model determining progression from Stage 2 to Stage 3.
- **`COPD 3-4/`**: Contains the model determining progression from Stage 3 to Stage 4 (Severe).
- **`models/`**: A centralized repository of the combined `.keras` weights used by the automated pipeline.

### 3. Data & Labels
- **`RespiratoryDatabase@TR/`**: A small, clean sample directory of testing `.wav` files included directly in this repo for immediate testing and verification.
- **`balanced_labels.csv` & `Labels.xlsx`**: The clinical ground-truth metadata mapping patient IDs to their respective diagnoses and respiratory cycle events.

> **Where is the full 2.1 GB Dataset?**  
> To keep the repository lightweight and clone-able, we do not host the full 2.1 GB `archive/` database here. Our research utilized the open-source **Respiratory Sound Database** from Kaggle. You can download the full database and place it in the project root to run large-scale evaluations.

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
