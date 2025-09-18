
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
1. The network takes as input an entire image and a set of object proposals (that come from an external algorithm: selective search)

## Key Achievements
- Introduce a single-stage training algorithm for object detection to replace the slow and inelegant multi-stage pipelines

## Pros & Cons

Pros
- Higher detection quality (mAP)
- Training is single-stage, using a multi-task loss
- Training can update all network layers
- No disk storage is required for feature caching

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
