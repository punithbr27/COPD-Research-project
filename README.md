<div align="center">
  <h1>🫁 COPD Audio Progression Pipeline</h1>
  <p><strong>Published at the 2026 International Conference on Bioinformatics and Computational Biology (ICBCB), Japan.</strong></p>
  
  [![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
  [![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)](https://www.tensorflow.org/)
  [![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
</div>

<br>

## 📌 Overview

This repository contains the official codebase and models for **COPD Audio Progression**, a state-of-the-art diagnostic pipeline designed to classify the severity stages of Chronic Obstructive Pulmonary Disease (COPD) using respiratory audio signals.

Our research proposes a robust, multi-stage hierarchical classification system that processes raw audio signals (like those from the Respiratory Sound Database) and progressively categorizes them into stages of severity (Healthy vs. Early Stage, Stage 1 vs. 2, Stage 2 vs. 3, and Stage 3 vs. 4). 

## 🚀 Key Features

- **Multi-Stage Expert Models:** Uses specialized `.keras` models for granular classification at each severity boundary.
- **Custom Group Normalization:** Implements custom layer definitions to improve model stability across varying acoustic environments.
- **Majority Voting System:** Employs a robust voting mechanism across multiple audio chunks to ensure high-confidence diagnostics.
- **Plug-and-Play Architecture:** Simply point the pipeline to a folder of patient audio files, and it generates a comprehensive diagnostic report.

## 📂 Repository Structure

```text
📦 COPD-Research-project
 ┣ 📂 RespiratoryDatabase@TR/   # Respiratory audio datasets for testing
 ┣ 📂 models/                   # Pre-trained .keras models and .npy scaler files
 ┣ 📜 run_full_pipeline.ipynb   # The main Jupyter Notebook to run predictions
 ┣ 📜 balanced_labels.csv       # Dataset labels
 ┣ 📜 Labels.xlsx               # Additional label metadata
 ┣ 📜 requirements.txt          # Python dependencies
 ┗ 📜 README.md                 # Project documentation (You are here!)
```

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

## 💻 How to Use

To generate a full diagnostic report for a patient:

1. Launch Jupyter Notebook:
   ```bash
   jupyter notebook
   ```
2. Open `run_full_pipeline.ipynb`.
3. The notebook is pre-configured with relative paths to the included `models/` and `RespiratoryDatabase@TR/` directories.
4. Set the `PATIENT_ID_TO_ANALYZE` variable in **Step 1** to the desired patient ID (e.g., `"221"`).
5. Run all cells to execute the multi-stage classification pipeline and view the final **Comprehensive Diagnostic Report**.

## 📖 Citation

If you use this codebase or models in your research, please cite our paper:

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
*Developed as part of cutting-edge research in respiratory sound analysis.*
