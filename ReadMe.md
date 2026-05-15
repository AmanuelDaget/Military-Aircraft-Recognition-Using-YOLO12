# 🛩️ Military Aircraft Detection from Satellite Imagery Using YOLO12

![Python](https://img.shields.io/badge/Python-3.12-blue)
![YOLO](https://img.shields.io/badge/YOLO-v12-darkgreen)
![Ultralytics](https://img.shields.io/badge/Ultralytics-8.4+-orange)
![License](https://img.shields.io/badge/License-MIT-green)
![Platform](https://img.shields.io/badge/Platform-Google%20Colab-yellow)

A deep learning project for detecting and classifying military aircraft from satellite imagery using YOLO12 (You Only Look Once version 12). The model is trained on the MAR20 dataset and can identify 20 categories of military aircraft directly from overhead satellite images.

----

## 📌 Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [Setup & Path Configuration](#setup--path-configuration)
- [Workflow](#workflow)
- [Results](#results)
- [Limitations](#limitations)
- [How to Run](#how-to-run)
- [Author](#author)

---

## Overview

This project builds an end-to-end object detection pipeline that:

- Cleans and validates raw satellite image annotations
- Converts Pascal VOC XML annotations to YOLO format
- Splits data into train / validation / test sets (70 / 15 / 15)
- Trains a YOLO12n model with augmentation and early stopping
- Evaluates on a fully unseen test set
- Visualizes training curves, confusion matrices, and predictions
- Supports custom image prediction from a local machine

---

## Dataset

**MAR20 — Military Aircraft Recognition Dataset**

- 20 categories of military aircraft
- Annotated with horizontal bounding boxes in Pascal VOC XML format
- Images are high-resolution satellite/aerial photographs

> Download the dataset and place it in your Google Drive before running the notebook. See [Setup & Path Configuration](#setup--path-configuration) below.

### Aircraft Classes

| ID | Class | ID | Class |
|----|-------|----|-------|
| 0  | A-10  | 10 | F-22  |
| 1  | A400M | 11 | F-35  |
| 2  | AG600 | 12 | J-20  |
| 3  | AV8B  | 13 | JAS39 |
| 4  | B-1   | 14 | MQ-9  |
| 5  | B-2   | 15 | Mig31 |
| 6  | B-52  | 16 | Mirage2000 |
| 7  | C-130 | 17 | RQ-4  |
| 8  | C-17  | 18 | Su-34 |
| 9  | E-2   | 19 | Tu-160|

---

## Project Structure

```
Military_Aircraft_Detection/
│
├── Military_Aircraft_detection_from_Satellite_image_using_YOLO12.ipynb
│
├── README.md
│
└── (generated after running the notebook)
    ├── cleaned_dataset/
    │   ├── images/
    │   └── annotations/
    │
    ├── yolo_dataset/
    │   ├── dataset.yaml
    │   ├── images/
    │   │   ├── train/
    │   │   ├── val/
    │   │   └── test/
    │   └── labels/
    │       ├── train/
    │       ├── val/
    │       └── test/
    │
    └── runs/
        └── detect/
            └── train/
                ├── weights/
                │   ├── best.pt
                │   └── last.pt
                ├── results.csv
                ├── confusion_matrix.png
                └── confusion_matrix_normalized.png
```

---

## Requirements

```
ultralytics
lxml
seaborn
opencv-python
pandas
numpy
matplotlib
scikit-learn
torch
torchvision
Pillow
tqdm
```

Install all at once:
```bash
pip install ultralytics lxml seaborn opencv-python pandas numpy matplotlib scikit-learn torch torchvision Pillow tqdm
```

> The notebook runs on **Google Colab** which has most of these pre-installed. Only `ultralytics` and `lxml` need to be manually installed.

---

## Setup & Path Configuration

> ⚠️ **Important: You must update the dataset path before running the notebook.**

### Step 1 — Upload the Dataset to Google Drive

1. Download the **MAR20** dataset
2. Upload the `MAR20.zip` file to your Google Drive
3. Place it inside a folder. For example:
```
My Drive/
└── Colab Notebooks/
    └── Computer Vision and Image Processing/
        └── Military_Aircraft_Detection_from_Satellite_image_CV_Project/
            └── MAR20.zip
```

### Step 2 — Update the Path in the Notebook

Find the **"Copy dataset from Drive"** cell and update the path to match where you saved the zip file:

```python
# CHANGE THIS to match your own Google Drive path
!cp "/content/drive/MyDrive/Colab Notebooks/Computer Vision and Image Processing/Military_Aircraft_Detection_from_Satellite_image_CV_Project/MAR20.zip" /content/
```

For example, if you saved it directly in `My Drive/datasets/`:
```python
!cp "/content/drive/MyDrive/datasets/MAR20.zip" /content/
```

> The rest of the notebook uses `/content/` paths which are automatically handled by Colab — **you only need to change this one cell.**

### Step 3 — GPU (Recommended)

For faster training, enable GPU in Colab before running:

```
Runtime → Change runtime type → Hardware accelerator → GPU (T4)
```

If no GPU is available, the notebook automatically falls back to CPU:
```python
device='cuda' if torch.cuda.is_available() else 'cpu'
```

> ⚠️ CPU training on 30 epochs with this dataset can take several hours. GPU is strongly recommended.

---

## Workflow

The notebook follows this pipeline from top to bottom:

```
1. Install & Import Libraries
        ↓
2. Mount Google Drive & Load Dataset
        ↓
3. Dataset Cleaning
   (check file existence, image readability, bounding box validity)
        ↓
4. 70/15/15 Train / Val / Test Split
        ↓
5. Class Distribution Analysis & Visualization
        ↓
6. Convert XML Annotations → YOLO Format
        ↓
7. Create YOLO Folder Structure & YAML Config
        ↓
8. Train YOLO12n (30 epochs, early stopping at patience=10)
        ↓
9. Evaluate on Validation Set  →  val_metrics
        ↓
10. Evaluate on Unseen Test Set  →  test_metrics
        ↓
11. Run Predictions on Test Images
        ↓
12. Plot Training Curves, Confusion Matrices
        ↓
13. Val vs Test Comparison Table
        ↓
14. Download Results as ZIP
        ↓
15. Predict on Custom Image from PC
```

---

## Results

### Training Configuration

| Parameter | Value |
|-----------|-------|
| Model | YOLO12n |
| Epochs | 30 (early stopping patience=10) |
| Image Size | 640×640 |
| Batch Size | 16 |
| Optimizer | AdamW |
| Learning Rate | 0.001 |
| Train / Val / Test Split | 70% / 15% / 15% |

### Evaluation Metrics

| Metric | Validation | Test (Unseen) |
|--------|-----------|---------------|
| mAP@50 | — | — |
| mAP@50-95 | — | — |
| Precision | — | — |
| Recall | — | — |

> Metrics will be filled in after training completes.

---

## Limitations

- The model is trained **exclusively on military aircraft classes**. If given a civilian aircraft image, it will attempt to match it to the nearest military class. Low confidence scores (below 0.55) on predictions likely indicate a non-military aircraft.

- The model performs best on **satellite/overhead imagery** similar to the MAR20 dataset. Side-view or ground-level aircraft photos may not yield accurate results.

- Training on CPU is supported but very slow. GPU is strongly recommended.

---

## How to Run

1. Clone this repository:
```bash
git clone https://github.com/your-username/military-aircraft-detection.git
cd military-aircraft-detection
```

2. Open the notebook in Google Colab:
   - Go to [colab.research.google.com](https://colab.research.google.com)
   - Upload the `.ipynb` file or open it from GitHub directly

3. Follow [Setup & Path Configuration](#setup--path-configuration) to point the notebook to your dataset

4. Run all cells from top to bottom:
```
Runtime → Run all
```

5. To predict on your own image, run the last cell — it will open a file picker to upload any image directly from your PC

---

## Downloading Results

At the end of the notebook, a ZIP file is automatically created and downloaded containing:

- `dataset.yaml` — YOLO configuration
- `cleaned_dataset/` — cleaned images and annotations
- `yolo_dataset/` — train/val/test splits in YOLO format
- `runs/detect/train/weights/best.pt` — best trained model
- `runs/detect/train/` — all plots, confusion matrices, results CSV
- `training_results.png` — combined training curves plot

---

## Author

**Amanuel D**

Project: Military Aircraft Detection from Satellite Imagery
Course: Computer Vision and Image Processing
Platform: Google Colab
Model: YOLO12 (Ultralytics)

---

## License

This project is licensed under the MIT License. The MAR20 dataset is subject to its own usage terms — please refer to the original dataset source for licensing details.
