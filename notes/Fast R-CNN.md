
# Fast R-CNN

## Motivation
Due to the complexity of object detection, current approaches train models in multi-stage pipelines that are slow and inelegant.
The complexity arises because detection requires the accurate localization of objects, creating two primary challenges:
1. Numerous candidate object locations must be processed
2. These candidates provide only rough localization that must be refined to achieve precise localization

Proposed method: a single-stage training algorithm that jointly learns to classify object proposals and refine their spatial locations
Result: (9 times faster than R-CNN and 3 times faster than SPPnet, achieved a mAP of 66% (vs 62% for R-CNN) on PASCAL VOC 2012.

## Architecture
![alt text](https://github.com/khchu93/NoteImage/blob/main/fastRCNN.PNG) <br>

Steps
1. The network takes as input an entire image and a set of object proposals (that come from an external region proposal algorithm: selective search)
2. Feed the entire image into a ConvNet to produce a conv feature map
3. For each object proposal, apply a **region of interest (ROI)<sup>[3]</sup>  pooling layer** to extract a fixed-length feature vector from the feature map
4. Feed the extracted feature vector into a sequence of fully connected layers to predict two parallel outputs:
- 1. Softmax classifier (size = C + 1, C = number of object classes, 1 = background)
     - Probability distribution over object classes + background
  2. Bounding box regressor (size = 4 x C)
     - 4 values (dx, dy, dw, dh) to refine/adjust the bounding-box positions

<sup>[1]</sup>Region of interest (ROI) pooling layer
- Divide the bounding box region into a fixed output size grid of bins (e.g., 7×7)
- Max pooling in each bin over the feature map values inside that bin
- Output a fixed-size feature map for each RoI<sup>[3]</sup>, regardless of the original region size<br>

<sup>[2]</sup> Region Proposal: raw bounding box candidate (coordinates only)<br>
<sup>[3]</sup> RoI (Region of Interest): the same box, but expressed on the feature map (contains the actual feature values inside that box (from the convolutional feature map))

## Key Achievements
- Introduce a single-stage training algorithm for object detection to replace the slow and inelegant multi-stage pipelines
- Combine the softmax classifier, SVMs, and regressors into one fine-tuning stage
- Shared convolutional feature map for all region proposals instead of each region proposal running its own CNN
- RoI Pooling replaces the interpolation-based resizing step in R-CNN, and reshapes it efficiently on feature maps instead of raw images

## Pros & Cons

Pros
- Higher detection quality (mAP)
- Training is single-stage, using a multi-task loss
- Training can update all network layers
- No disk storage is required for feature caching

Cons
- Still relies on external region proposals (selective search)
- With a large number of proposal regions
- Localization accuracy suffers from RoI Pooling
- Takes time to process (~2s per image) = not suitable to real time tasks

## When to use
- Educational purpose

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
[Fast R-CNN](https://arxiv.org/pdf/1504.08083)
