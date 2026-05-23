# Dermatology Computer Vision Project

This repository contains notebooks, scripts, and datasets for training a custom residual CNN model to classify skin lesions into three consumer-facing macro classes:
- **Acne & Rosacea**
- **Infections**
- **Eczema & Dermatitis**

The notebook `modeling_cv-colab.ipynb` is designed to be fully hybrid: it runs seamlessly on a local machine using local data/weights or in Google Colab utilizing mounted Google Drive storage.

---

## Local Directory Structure

The project layout on your local machine is organized as follows:

```text
dematology_cv/
├── modeling_cv-colab.ipynb       # Restructured hybrid notebook (Local & Colab compatible)
├── modeling_cv.ipynb             # Local training notebook (pre-run)
├── environment.yml               # Conda environment dependency definition
├── best_skin_classifier_weights.h5 # Trained model weights
├── data/                         # Root directory for all dataset files
│   ├── acne_dataset_manifest_cleaned.csv  # Manifest for the Acne dataset
│   ├── skin_defects_cleaned.csv           # Cleaned skin defects labels
│   ├── Acne_and_Skin/            # Supplementary Acne dataset sheets and CSVs
│   ├── archive_2/                # HAM10000 dataset partition folders
│   ├── archive_3/                # Secondary dataset files (acne, bags, redness)
│   └── dermnet/                  # Main DermNet dataset folder
│       ├── dermnet_manifest_cleaned.csv  # Manifest for the DermNet dataset
│       ├── train/                # DermNet training images split by class
│       │   ├── Acne and Rosacea Photos/  # Contains physical Acne training images
│       │   └── ... (other class folders)
│       └── test/                 # DermNet testing images split by class
│           ├── Acne and Rosacea Photos/  # Contains physical Acne testing images
│           └── ... (other class folders)
├── _check_images.py              # Utility script to check local dataset integrity
├── _inspect_acne.py              # Utility script to inspect the Acne manifest
├── _inspect_data.py              # Utility script to inspect the DermNet manifest
├── _update_nb.py                 # Notebook synchronization utility
└── add_confusion_matrix.py       # Utility script to generate a confusion matrix
```

---

## Restructured Notebook Explanation

The `modeling_cv-colab.ipynb` notebook handles pathing and environments dynamically:

- **Environment Detection**: Uses `try-except` on `google.colab` to automatically determine if it is running in Colab.
- **Dynamic Manifest Loading**:
  - In Colab: Mounts Google Drive at `/content/drive` and reads the manifest CSVs directly from `/content/drive/MyDrive/dermind/...`.
  - Locally: Reads manifests directly from the local `./data/` folder.
- **Acne Image Path Resolution**:
  - Locally, the physical Acne images are stored under `data/dermnet/train/Acne and Rosacea Photos` and `data/dermnet/test/Acne and Rosacea Photos`.
  - The notebook uses a custom mapping resolver to locate each local Acne file by checking both folders, rather than crashing with a `FileNotFoundError` due to missing `Acne dataset/Acne` paths.
- **Model Weights**:
  - Saves the best trained model weights to the local directory as `best_skin_classifier_weights.h5` during local runs, or to Google Drive when running in Colab.

---

## Local Setup Instructions

To set up and run this project on your local machine:

### 1. Environment Activation
You can activate your pre-installed Conda environment named `dermind-cv`:
```bash
conda activate dermind-cv
```

*(If you ever need to recreate or install the environment from scratch, run `conda env create -f environment.yml`)*

### 2. Verify Your Dataset Paths
Ensure the image paths physically resolve by running the helper script:
```bash
python _check_images.py
```
This script will report how many manifest-listed images are successfully found in your local folders.

### 3. Open the Notebook
Launch JupyterLab or Notebook to open `modeling_cv-colab.ipynb`:
```bash
jupyter lab
```
Run the cells sequentially. The first cells will print `"Running on a local device."` and instantly load the datasets from your local `./data/` folder.
