# VGG

## Architecture
There are two types of VGG: VGG16 and VGG19, with a different number of corresponding convolution layers.



P.S. VGG stands for Visual Geometry Group

## Key Achievements
- Proposed a network of increasing depth using an architecture with very small receptive field (= convolution filter): 3 x 3
    - A stack of very small conv. layer (without spatial pooling in between) has an effective receptive field of a large conv. layer (ex. A stack of two 3 x 3 = receptive field of 5 x 5, three 3 x 3 = receptive field of 7 x 7)
    - With more rectification layers instead of a single one, the decision function becomes more discriminative
    - Decreased the number of parameters while maintaining the same performance
        - three 3 x 3 has 3(3<sup>2</sup>C<sup>2</sup>) = 27C<sup>2</sup> weights
        - one 7 x 7 has 7<sup>2</sup>C = 49C weights

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
[VERY DEEP CONVOLUTIONAL NETWORKS FOR LARGE-SCALE IMAGE RECOGNITION](https://arxiv.org/pdf/1409.1556)
