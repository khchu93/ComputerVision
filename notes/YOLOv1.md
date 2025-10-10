
# YOLOv1

## Motivation
Object detection models before YOLO relied on multi-stage pipelines (e.g., R-CNN family), which were accurate but slow and hard to optimize end-to-end.
YOLO was proposed to unify detection into a single-stage, real-time network by reframing detection as a regression problem that directly predicts bounding boxes and class probabilities from full images.

Goals:
- Replace region proposal + classification pipelines with a single convolutional network.
- Enable real-time detection for tasks like self-driving cars and robotics.
- Simplify training with a fully end-to-end system.

Result:
- Achieved real-time performance (45 FPS) with competitive accuracy (63.4% mAP on PASCAL VOC 2007).
- One of the first detectors trained entirely end-to-end.

## Architecture
YOLO’s backbone was inspired by GoogLeNet, but designed for detection rather than classification.
1. Input Processing
   - Resize input image to 448×448, normalize pixel values, and apply data augmentation (scaling, translation, color jitter).
2. Feature Extraction Backbone (Conv Layers)
   - A 24-layer convolutional network inspired by GoogLeNet, composed of alternating 1×1 reduction layers and 3×3 conv layers with max pooling in between.
3. Pretraining + Fine-tuning
   - Pretrain the first 20 conv layers on ImageNet classification (224×224). For detection, increase input size to 448×448, add 4 more conv + 2 fully connected layers, and fine-tune on detection data.
4. Detection Head (Fully Connected Layers)
   - Flatten feature map → two fully connected layers → output vector of S×S×(B×5 + C) (e.g., 7×7×30).
5. Grid-Based Prediction
    - Divide image into S×S grid (S=7). Each cell predicts B=2 boxes (x, y, w, h, confidence) and C class probabilities shared across boxes.
6. Loss Function (Training Objective)
    - Custom sum-squared error loss with balancing terms:
      - λ<sub>coord</sub> = 5 (emphasize localization)
      - λ<sub>noobj</sub> = 0.5 (suppress background noise)
    - Predict √w, √h for small-box sensitivity.
Each object assigned to one “responsible” predictor (highest IOU).
7. Inference & Post-Processing
    - Compute class-specific confidence: Pr(Class) × IOU(pred, truth). Apply Non-Maximum Suppression (NMS) to remove overlapping boxes.

## Key Achievements
- Introduced Unified Detection via Spatial Regression
  - YOLO treats detection as a single regression task from image pixels → bounding box coordinates + class probabilities, eliminating the need for external region proposals.
  - This end-to-end design allows real-time detection while maintaining global reasoning over the entire image.
- Real-Time Speed (Single-Shot Detection)
  - Achieved 45 FPS on PASCAL VOC — over 10× faster than R-CNN — with competitive accuracy.
- Introduced Grid-Based Prediction Mechanism
  - Introduced the S×S grid system, where each cell predicts bounding boxes and class probabilities. It provided a structured, fixed-size output format enabling efficient detection across the entire image.
## Pros & Cons

Pros
- Real-time performance: Runs at 45 FPS (YOLOv1) and 155 FPS (Fast YOLO) on standard GPUs.
- Simple pipeline: No region proposal, feature cropping, or multi-stage processing.

Cons
- Rigid grid design: Each grid cell can detect only one class/object — struggles when multiple small objects fall into one cell.
- Fast but sometimes sacrifices accuracy, especially for small or overlapping objects.
- Coarse grid and fixed output size limit detection precision for dense scenes.

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
