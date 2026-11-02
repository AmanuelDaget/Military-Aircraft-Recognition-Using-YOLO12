# Dataset

## MAR20 — Military Aircraft Recognition Dataset

The dataset used in this project is **MAR20**, containing 20 categories
of military aircraft annotated with horizontal bounding boxes.

## Download

The dataset is hosted on Google Drive:

👉 [Download MAR20.zip](https://drive.google.com/file/d/1UZJssGTEM_a1vxnfl5o5LAmferQIlj1g/view?usp=sharing)


## How to Use

1. Download `MAR20.zip` from the link above
2. Upload it to your Google Drive
3. Update the path in the notebook:

\```python
!cp "/content/drive/MyDrive/YOUR_FOLDER/MAR20.zip" /content/
\```

## Auto-Download in Colab (Alternative)

Instead of manually uploading, you can download it directly
inside the notebook using gdown:

\```python
!pip install gdown
import gdown

gdown.download(
    f"https://drive.google.com/file/d/1UZJssGTEM_a1vxnfl5o5LAmferQIlj1g/view?usp=sharing",
    "/content/MAR20.zip",
    quiet=False
)

!unzip -q /content/MAR20.zip -d /content/MAR20
\```

