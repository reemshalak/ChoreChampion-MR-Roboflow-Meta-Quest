# 🧺 Laundry Recognition System  
### Roboflow + YOLOv8s + Unity Sentis (Meta Quest)

## 📌 Overview

This module implements a real-time **crumpled laundry recognition system** used in the **Tidy Toss** mini-game inside *ChoreChampion*.

The goal was to detect crumpled clothing items in Mixed Reality and extract their bounding box positions to enable interaction and physics-based gameplay in Unity (Meta Quest compatible).

### 🔧 Pipeline

- Roboflow (dataset labeling & augmentation)
- YOLOv8s (Ultralytics) for training
- Google Colab (GPU training)
- ONNX export
- Unity Sentis conversion
- Unity URP (Meta Quest deployment)


## 🗂 Dataset Preparation (Roboflow)

The dataset was labeled using **Roboflow**, focusing specifically on **crumpled clothing items**.

- **412 base images**
- Augmented ×3
- Final dataset size: **~1236 images**

### Augmentation Techniques

- Rotation
- Scaling
- Brightness variation
- Horizontal flipping

### Why Augmentation?

Because the dataset size was moderate, augmentation helped:

- Improve generalization
- Reduce overfitting
- Increase robustness under different lighting conditions
- Improve real-world MR detection stability


## 🧠 Model Training

Training was performed using:

- **Model:** YOLOv8s  
- **Framework:** Ultralytics  
- **Environment:** Google Colab (GPU)  
- **Epochs:** 150  

### Why YOLOv8s?

YOLOv8s was selected because it is:

- Lightweight
- Optimized for real-time inference
- Suitable for standalone VR hardware (Meta Quest)

Larger variants (YOLOv8m, YOLOv8l) provide higher accuracy but are heavier and less efficient for real-time VR deployment.

## 🏷 Class Design

The model was trained using two classes:

- `clothes`
- `others`

Adding an **"others"** class significantly reduced false positives.

Initially, using only one class caused the model to attempt matching clothing patterns in nearly all objects. Including a negative class improved classification reliability.



## 🔄 Model Export & Unity Integration

After training:

1. The trained `.pt` model was exported to **ONNX**.
2. The ONNX model was converted to Unity’s **Sentis (.sentis)** format.
3. Integration was implemented using the QuestCameraKit sample:

🔗 https://github.com/xrdevrob/QuestCameraKit?tab=readme-ov-file#2-object-detection-with-unity-sentis

### The sample was used for:

- Real-time camera inference
- Bounding box rendering
- Label handling
- ONNX → Sentis conversion via `SentisModelEditorConverter`

The converted `.sentis` model was integrated into a **Unity URP project** optimized for Meta Quest.


## 🎮 In-Game Usage (Tidy Toss)

Detected clothing items:

- Generate bounding boxes
- Provide positional data
- Enable collider placement
- Support physics-based tossing mechanics

### Performance Notes

- ~0.2s detection delay observed on-device
- Closer proximity improves detection reliability
- Bounding box tracking in Unity differs slightly from raw inference preview

## 🚧 Challenges & Learnings

- Training initially used generic clothing instead of crumpled clothing, reducing detection accuracy.
- Single-class training caused high false positives.
- Model size directly impacts standalone VR performance.
- Real-time inference requires balancing accuracy and efficiency.

## 🧩 Tech Stack

- Roboflow
- Ultralytics YOLOv8s
- Google Colab
- ONNX
- Unity Sentis
- Unity URP
- Meta Quest

## 📎 Summary

This project demonstrates an end-to-end computer vision pipeline from dataset preparation to real-time Mixed Reality deployment on standalone VR hardware.
