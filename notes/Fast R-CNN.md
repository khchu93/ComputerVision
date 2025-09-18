
# Fast R-CNN

## Motivation
Due to the complexity of the object detection, current approaches train models in multi-stage pipelines that are slow and inelegant.
The complexity arises because detection requires the accurate localization of objects, creating two primary challenges:
1. Numerous candidate object locations must be processed
2. These candidates provide only rough localization that must be refined to achieve precise localization

Proposed method: a single-stage training algorithm that jointly learns to classify object proposals and refine their spatial locations
Result: (9 times faster than R-CNN and 3 times faster than SPPnet, achieved a mAP of 66% (vs 62% for R-CNN) on PASCAL VOC 2012.

## Architecture

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
