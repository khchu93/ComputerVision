
# Faster R-CNN

## Motivation

While Fast R-CNN **greatly speeds up** object detection by **sharing convolutional computation**, it still depends on **slow, hand-crafted region proposals** like Selective Search. It has become the test-time computational bottleneck in the SOTA detection system. <br>
To overcome it, the authors proposed a **Region Proposal Network (RPN)** that learns to **generate proposals directly from the feature map**, making the whole pipeline **end-to-end trainable**, faster, and more accurate.

## Architecture
![alt text](https://github.com/khchu93/NoteImage/blob/main/fasterRCNN.jpg?raw=true) <br>
- Faster R-CNN is composed of two modules:
  1. A deep convolutional network that proposes regions (Region Proposal Network (RPN))
  - - Feed the input image through a ConvNet backbone to produce a shared feature map (H x W x 512) (shared between RPN and detector): reduce spatial dimensions and encode rich semantic representations
    - Slide a small n x n convolutional layer (e.g., n = 3) with padding over the shared feature map: add local context around each spatial location and create a more refined feature map of the same size (H x W x 512)
    - Place k **anchor<sup>[1]</sup> reference boxes** (k = scale x aspect ratio) at each spatial location of the refined feature map (number of anchors = k x H x W boxes)
    - Feed the refined feature map into two parallel 1×1 conv layers:
    - 1. A box-regression layer (reg): outputs 4 offsets (dx, dy, dw, dh) per anchor (= 4k offsets per spatial location)
      2. A box-classification layer (cls): outputs objectness score (probability of object and probability of background) per anchor (= 2k scores per spatial location)
    - Generate region proposals by applying the predicted offsets to each anchor
    - Keep the top N proposals by score and apply NMS: reduce the number of proposals
  2. The Fast R-CNN detector that uses the proposed regions (= Fast R-CNN without external region proposal)

<sup>[1]</sup> Anchor: A predefined reference box at a given location on the feature map, based on scale and aspect ratio: (x, y, w, h) - center, width, height.

## Key Achievements
- Introduce the Region Proposal Network (RPN) to replace the external region proposal method
- Use shared feature map between RPN and fast R-CNN module to reduce computation
- Introduce anchors at each spatial location to handle multiple scales and aspect ratios efficiently
- End-to-end trainable detection framework with the RPN

## Pros & Cons

Pros
- The model is now end-to-end trainable, unlike the previous R-CNN and Fast R-CNN, which relied on an external region proposal method
- Higher quality proposals with the learnable RPN that proposes and adjusts at the same time the localization of the proposals
- Much faster by replacing the slow external region proposal method (CPU-bound, ~2s per image) with a module that can utilize GPU during the forward pass (~0.2s per image)

Cons
- Still two-stage (RPN + Fast R-CNN): slower than modern single-stage detectors
- More memory and computation usage with higher resolution images

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
[Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks](https://arxiv.org/pdf/1506.01497) <br>
[Faster R-CNN: Object Detection](https://medium.com/thedeephub/faster-r-cnn-object-detection-5dfe77104e31)
