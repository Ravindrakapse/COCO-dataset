
# 📝 COCO Segmentation: Task 1 - Dataset Preparation + Model Evaluation

## 🌀 Task 1: Dataset Preparation

We processed the **COCO val2017 dataset** to extract segmentation masks for a selected subset of classes:

### 🔹 Selected Classes:

- Person
- Car
- Bus
- Dog

### ⚖️ Mask Generation Script Highlights:

- Uses `pycocotools` to parse COCO annotations
- Generates two types of masks:
  - `masks_raw/`: grayscale masks with class indices (used for training)
  - `masks_colored/`: RGB visualization masks with per-class color
- Handles overlapping annotations using class priority

---

## 💡 Task 2: Model Training and Evaluation

We trained segmentation models on the generated masks using different architectures and techniques:

### 🏋️ Models Trained:

- **U-Net** (baseline)
- **DeepLabV3 + ResNet50**
- **DeepLabV3 + ResNet50** with:
  - Class Weighting
  - Oversampling
  - Focal Loss
 
### ⚙️ Techniques Used:

- **Class Weighting**: Addressed pixel imbalance by assigning higher weights to rare classes (like bus, dog) and lower weight to dominant ones (person).
- **Oversampling**: Rare-class images were duplicated in the dataset to improve representation during training.
- **Focal Loss**: Modified loss function that down-weights easy examples and focuses on hard-to-classify pixels.

### 📃 Why DeepLabV3?

- DeepLabV3 is a state-of-the-art semantic segmentation architecture with an atrous spatial pyramid pooling (ASPP) module that helps in capturing multiscale context.
- It outperforms U-Net in complex scenes by learning high-resolution details through its deeper ResNet backbone.
- By fine-tuning DeepLabV3 on a COCO subset, we leveraged pretrained weights while adapting the classifier for our specific task.


### 🔢 Evaluation Metric:

- **Mean Intersection over Union (mIoU)**
- Class-wise IoUs (class 0 = background)

---

## 📊 Model Results:

![Original image, masked image, predicted](image.png)

### 🔍 U-Net

```
Class 0: IoU = 0.8980
Class 1: IoU = 0.3620
Class 2: IoU = 0.0000
Class 3: IoU = 0.0000
Class 4: IoU = 0.0003
```

### 🔍 DeepLabV3-ResNet50 (Baseline)

```
Class 0: IoU = 0.9364
Class 1: IoU = 0.5739
Class 2: IoU = 0.1814
Class 3: IoU = 0.2083
Class 4: IoU = 0.0792
```

### 🔍 DeepLabV3 + Class Weights

```
Class 0: IoU = 0.9229
Class 1: IoU = 0.5642
Class 2: IoU = 0.1218
Class 3: IoU = 0.1469
Class 4: IoU = 0.0543
```

### 🔍 DeepLabV3 + Oversampling

```
Class 0: IoU = 0.9390
Class 1: IoU = 0.5850
Class 2: IoU = 0.1886
Class 3: IoU = 0.3040
Class 4: IoU = 0.0714
```

### 🔍 DeepLabV3 + Focal Loss

```
Class 0: IoU = 0.9382
Class 1: IoU = 0.5991
Class 2: IoU = 0.1931
Class 3: IoU = 0.2463
Class 4: IoU = 0.0518
```

---

## 📊 Wandb UI:
-For training metrics
![wandb ui](wandb_ui.png)

---

## 🛠️ How to Run This Project

### 📁 Project Structure
```
📦 COCO-dataset
├── 📄 dataset.ipynb        # Notebook to generate masks from COCO annotations
├── 📄 model_train.ipynb    # Notebook to train and evaluate models (U-Net, DeepLabV3)
├── 📄 image.png            # Original, masked, predicted image
├── 📄 wandb_ui.png         # wandb UI snapshot
├── 📄 pyproject.toml       # Project dependencies (used by uv)
├── 📄 uv.lock              # Locked versions of all installed packages
├── 📄 README.md            # Documentation and results
```

---

### 🐍 Step 1: Create Virtual Environment using `uv`

If you haven't already installed `uv`, install it with:

```bash
pip install uv
```

Then initialize and activate the environment:

```bash
uv venv
#activate
#for mac/linux
source .venv/bin/activate

#for windows
.venv\Scripts\activate
```

---

### 📦 Step 2: Install Dependencies

Install all required packages :

```bash
uv sync
```

> ✅ This ensures that you have all necessary libraries like `torch`, `opencv-python`, `albumentations`, `pycocotools`, `wandb`, etc.

---
### 📓 Step 3: get the COCO dataset

```bash
# Download validation images (5,000 images)
wget http://images.cocodataset.org/zips/val2017.zip
unzip val2017.zip -d images

# Download annotations (contains segmentations)
wget http://images.cocodataset.org/annotations/annotations_trainval2017.zip
unzip annotations_trainval2017.zip -d annotations

```
structure
```
coco_dataset/
├── images/
│   └── val2017/                    <-- All validation images
├── annotations/
│   └── instances_val2017.json      <-- Annotation file
```
---

### 📓 Step 3: Run the Notebooks

Now you can open the notebooks using Jupyter:

```bash
jupyter notebook
```

Then open:
- `dataset.ipynb` to generate training masks
- `model_train.ipynb` to train and evaluate models

---

### ✅ Tips

- Make sure you have the COCO `annotations` and `images` folders correctly set in your notebook paths.

---
