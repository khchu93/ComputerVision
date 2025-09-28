
# Masked R-CNN

## Motivation

- At the time, CNN-based models were already good at classification, object detection, and even semantic segmentation, but they couldn’t distinguish between different instances of the same class.
- To solve this, the authors proposed Mask R-CNN, extending Faster R-CNN with a mask prediction branch to enable instance segmentation.

## Architecture
![alt text](https://github.com/khchu93/NoteImage/blob/main/MaskedRCNN.PNG?raw=true)<br>
[Image source](https://developers.arcgis.com/python/latest/guide/how-maskrcnn-works/)

Mask R-CNN is an improvement based on Faster R-CNN. Just like Faster R-CNN, Mask R-CNN starts with:
1. Feed the image into a ConvNet backbone model (e.g., VGG) to extract a shared convolutional feature map
2. Applied a Region Proposal Network to propose potential object regions using predefined anchor boxes
3. Use RoIAlign (instead of RoIPool) to precisely extract fixed-size feature representations for each proposed region, avoiding misalignment caused by quantization.
4. Pass the aligned features into three parallel heads:
- Classification head — predicts the object category
- Bounding box regression head — refines the coordinates of the detected box
- Mask head (new branch in Mask R-CNN) — generates a pixel-level binary segmentation mask for each object instance.
5. Select the mask corresponding to the predicted class and upsample it to the final bounding box size.
6. Apply Non-Max Suppression (NMS) to remove duplicate detections, resulting in final outputs: class label, bounding box, and instance segmentation mask for each detected object.

## Key Achievements
- Introduced a Parallel Mask Prediction Branch, enabling precise object outline extraction
- Proposed RoIAlign (Replacing RoIPool), improved spatial alignment
- Unified Detection and Segmentation in a Single Framework
- Achieved State-of-the-Art Results on COCO Benchmark

## Pros & Cons

Pros
- Simple architecture
- Outperforms all existing single models
- Flexible to apply to other tasks like human pose estimation

Cons
- slower than single-stage models like YOLO
- high memory consumption
- small object detection can still be challenging

## When to use

## Implementation
- Framework: 
- Dataset: 
- Colab Notebook: [link]()

<!--
## Results
Training

Validation

Examples:
-->

## References
[Mask R-CNN](https://arxiv.org/pdf/1703.06870)
[Mask R-CNN - Everything explained](https://www.picsellia.com/post/mask-r-cnn-everything-explained)
