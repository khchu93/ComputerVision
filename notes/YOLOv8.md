# YOLOv8

## Motivation
YOLOv8 was developed by Ultralytics to further improve real-time object detection by simplifying training, eliminating anchors, and unifying multiple tasks (detection, segmentation, classification, and pose estimation) under one consistent framework.

Goals:
- Overcome limitations of YOLOv5’s anchor-based design.
- Enhance generalization and ease of use for custom datasets.
- Boost performance on small and dense objects while maintaining real-time inference.
- Provide a production-ready, modular PyTorch implementation for research and deployment.

## Architecture
1. Input Preprocessing
    - The input image is first resized to a standard dimension (commonly 640×640) while maintaining the aspect ratio using letterboxing to avoid distortion. Pixel values are normalized to the [0,1] range. YOLOv8 also supports dynamic shape resizing, which allows variable image sizes within batches, improving efficiency during training.
2. Data Augmentation
    - To enhance generalization and robustness, YOLOv8 employs a rich data augmentation pipeline. This includes Mosaic augmentation (combining four images into one), MixUp (blending two images and their labels), HSV color jittering, random flips, and scaling. During later training stages, the intensity of augmentation is reduced automatically to stabilize convergence.
3. Backbone – C2f-based CSP Network
    - The backbone is based on CSPDarknet, enhanced with a new C2f module (Cross Stage Partial with feature fusion). The C2f block replaces the older C3 structure used in YOLOv5. It divides feature maps into partial gradients for cross-stage connections while fusing intermediate features efficiently. The input stem also replaces the old Focus layer with a simpler convolutional layer, reducing overhead.
4. Neck – FPN + PAN Feature Fusion
    - The neck integrates a combination of Feature Pyramid Network (FPN) and Path Aggregation Network (PAN) structures to merge features across multiple scales. The FPN performs top-down fusion to pass semantic information from deeper layers to shallower ones, while PAN adds a bottom-up path that refines localization features.
5. Detection Head – Decoupled and Anchor-Free
    - YOLOv8 abandons the traditional anchor-based approach and adopts an anchor-free, decoupled detection head. Each detection layer outputs object centers, width, height, and confidence directly, without predefined anchor boxes. The head is split into two independent branches — one for classification and one for bounding box regression — rather than sharing weights.
6. Loss Function and Optimization
    - The training objective combines multiple loss components. Varifocal Loss is used for classification and confidence, aligning prediction confidence with IoU quality. CIoU Loss (Complete IoU) is used for bounding box regression, as it considers overlap area, center distance, and aspect ratio simultaneously. Binary Cross-Entropy (BCE) Loss handles objectness prediction.
7. Inference and Post-processing
    - During inference, the network produces bounding box predictions and class scores directly from the decoupled head outputs. Predictions are filtered using a confidence threshold and Non-Maximum Suppression (NMS) to remove redundant boxes. YOLOv8 also provides seamless export to deployment formats such as ONNX, TensorRT, CoreML, and TFLite.

## Key Achievements
- Fully Anchor-Free Detection Head
  - Removed all predefined anchor boxes and predicted object centers directly. By eliminating anchor tuning, it greatly reduces hyperparameter complexity, and improves adaptability to new datasets.
- Decoupled Head (Separate Classification & Regression)
  - Detection head split into independent branches for class probability and bounding box regression. It stabilizes training, improves precision, and allows better convergence.
- C2f Backbone Module
  - Replaces YOLOv5’s C3 block with a Cross Stage Partial module with feature fusion (C2f). This replacement enhances gradient flow, increases efficiency, and improves feature extraction for accurate detection.

## Pros & Cons

Pros
- Single architecture supports detection, segmentation, classification, and pose estimation
- Fully anchor-free — no anchor tuning required.
- Decoupled head improves classification and regression accuracy.
- C2f backbone enhances gradient flow and feature extraction efficiency.

Cons
- Slightly higher memory use due to decoupled head.
- More complex backbone than YOLOv5, potentially heavier for edge deployment.
- Training large models still computationally expensive.
  
<!--
## Implementation
- Framework: 
- Dataset: 
- Colab Notebook: [link]()

## Results
Training

Validation

Examples:
-->

## References
