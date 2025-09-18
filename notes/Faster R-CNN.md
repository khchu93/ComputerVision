
# Faster R-CNN

## Motivation

While Fast R-CNN **greatly speeds up** object detection by **sharing convolutional computation**, it still depends on **slow, hand-crafted region proposals** like Selective Search. It has become the test-time computational bottleneck in the SOTA detection system. 
To overcome it, the authors proposed a **Region Proposal Network (RPN)** that learns to **generate proposals directly from the feature map**, making the whole pipeline **end-to-end trainable**, faster, and more accurate.

## Architecture
![alt text](https://github.com/khchu93/NoteImage/blob/main/fasterRCNN.jpg?raw=true) <br>
- Faster R-CNN is composed of two modules:
  1. A deep convolutional network that proposes regions (Region Proposal Network (RPN))
  - -
  2. The Fast R-CNN detector that uses the proposed regions (= Fast R-CNN without external region proposal)

## Key Achievements
- 

## Pros & Cons

Pros
- 

Cons
- 

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
[Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks](https://arxiv.org/pdf/1506.01497)
[Faster R-CNN: Object Detection](https://medium.com/thedeephub/faster-r-cnn-object-detection-5dfe77104e31)
