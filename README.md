### 💻 Computer Vision <br />
Learning Journey: Computer Vision

### 🔍 Description <br />
A personal exploration of foundational Computer Vision Models (CV).
- Documented each model’s motivation, architecture, key contributions, and limitations.
- Rebuilt the models from scratch based on academic and open-source references.
- Analyzed and compared results to deepen understanding and share practical insights.

📘 Related project: [Large Language Models Journey](https://github.com/khchu93/LLMs/edit/main/README.md)

### 📺 Projects
| Tag | Name | Description
|:---|:---|:---
|ML|[🔥 Fire smoke detection](https://github.com/MorganPeju/inf8225_project) | Python - Forest fire smoke detection with YOLOv5 model


### 📚 CV Models<br />

#### Object Detection
- [Faster R-CNN](https://github.com/khchu93/ComputerVision/blob/main/notes/Faster%20R-CNN.md) [(Code)](https://github.com/khchu93/ComputerVision/blob/main/models/Faster_R_CNN_implementation.ipynb), 2015 (keyword: end-to-end, RPN, shared feature map, Fast R-CNN detector module)
- [Fast R-CNN](https://github.com/khchu93/ComputerVision/blob/main/notes/Fast%20R-CNN.md), 2015 (keyword: single stage, CNN only once, shared feature map, ROI pooling layer, 2 head FC layer output, NMS, region proposal)
- [R-CNN](https://github.com/khchu93/ComputerVision/blob/main/notes/R-CNN.md) [(Code)](https://github.com/khchu93/ComputerVision/blob/main/models/rcnn_implementation.ipynb), 2014 (keyword: region proposal, selective search, Bounding box regression, NMS)

#### Image Classification
- CNN Era
  - [GoogleNet](https://github.com/khchu93/ComputerVision/blob/main/notes/GoogLeNet.md), 2015 (keyword: Inception Module, GAP, Auxiliary Classifier, Multi-scales, 1 x 1 conv.)
  - [ResNet](https://github.com/khchu93/ComputerVision/blob/main/notes/ResNet.md), 2015 (keyword: Residual Learning, Skip connection, GAP, bottleneck)
  - [VGG](https://github.com/khchu93/ComputerVision/blob/main/notes/VGG.md), 2014 (keyword: very deep layers, very small convolutional filter 3 x 3)
  - [AlexNet](https://github.com/khchu93/ComputerVision/blob/main/notes/AlexNet.md), 2012 (keyword: ReLU, Dropout)
- Transformer Era
  - Vision Transformer (ViT), 2020 (TBD)
- Hybrid/Modern CNNs
  - ConvNeXt, 2022 (TBD)

#### Instance Segmentation
- [Mask R-CNN](https://github.com/khchu93/ComputerVision/blob/main/notes/MaskedR-CNN.md), 2017 (keyword: RoIAlign, Mask Head, FPN, RPN, NMS)

#### Semantic Segmentation
- [U-Net](https://github.com/khchu93/ComputerVision/edit/main/notes/U-Net.md), 2015 (keyword: Symmetric “U” Shape, Skip Connections/Feature Concatenation, Precise Localization, Encoder–Decoder Structure)

#### Real-time Detection
- [YOLOv8](https://github.com/khchu93/ComputerVision/blob/main/notes/YOLOv8.md), 2023 (keyword: Anchor-Free Detection Head, Decoupled Head, C2f Backbone Module, Unified Multi-Task Framework, Varifocal + CIoU Loss)
- [YOLOv5](https://github.com/khchu93/ComputerVision/blob/main/notes/YOLOv5.md), 2020 (keyword: CSPDarknet Backbone, FPN + PAN Neck, Anchor-Based Multi-Scale Detection, Mosaic & MixUp Augmentation)
- [YOLOv1](https://github.com/khchu93/ComputerVision/blob/main/notes/YOLOv1.md), 2016 (keyword: Unified Detection / Spatial Regression, Real-Time Detection, S×S Grid)

#### Vision State Space Models (VSSMs)
- DAMamba, 2024 (TBD)
- [Vision Mamba](https://github.com/khchu93/ComputerVision/blob/main/notes/Vision%20Mamba.md) (Vim), 2024 (keyword: Attention-free, Cross-scan, Stacked SSM blocks, Hardware-aware parallel scan, Dynamic parameterization)

#### Foundation Models
- [Segment Anything Model](https://github.com/khchu93/ComputerVision/blob/main/notes/SAM.md) (SAM), 2023 (keyword: Promptable Segmentation Task, Data Engine, Amortized Real-Time Interactive Use, Ambiguity-Aware Model/Design)
- CLIP, 2021 (TBD)
