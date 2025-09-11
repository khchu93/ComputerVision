# VGG

## Architecture
There are two types of VGG: VGG16 and VGG19, with a different number of convolution layers.
![alt text](https://github.com/khchu93/NoteImage/blob/main/VGG.webp?raw=true) <br>
[Image source](https://medium.com/@siddheshb008/vgg-net-architecture-explained-71179310050f)

[VGG](https://arxiv.org/pdf/1409.1556) 16 is a 16-layer CNN model that consists of: 13 convolutional layers + 3 fully connected layers, ending with a softmax.
The convolutional layers have a stride of 1 pixel and the padding of 1 pixel as well to preserve the spatial resolution of the input after convolution.
Max pool is performed over a 2 x 2 pixel window, with a stride of 2.

Number of parameters: over 138 million

P.S.1. VGG stands for Visual Geometry Group.
P.S.2. Local Response Normalization (LRN)<sup>[1]</sup> is not used as it increases memory usage and training time without increasing overall accuracy.

[<sup>[1]</sup>](https://prateekvjoshi.com/2016/04/05/what-is-local-response-normalization-in-convolutional-neural-networks/) Local Response Normalization (LRN) layer is a method that mimics a neurobiology concept called "lateral inhibition". This refers to the capacity of an excited neuron to subdue its neighbors, which creates a form of local maxima, hence increasing the sensory perception in that area. So, the LRN layer is useful when we are dealing with ReLU neurons because the unbounded activations allow neurons to have large values everywhere. We want to detect high-frequency features with a large response, which can be done by normalizing around the local neighborhood of the excited neuron. There are two types of normalizations:
- 1. Normalization within the same channel: apply a 2D neighborhood of dimension N x N to normalize all values within, where N is the size of the normalization window
  2. Normalization across channels: apply a 3D neighborhood of dimension N x 1 x 1 to normalize all values within, where 1 x 1 is refers to a single value in a 2D matrix
  
## Key Achievements
- Proposed a network of increasing depth using an architecture with very small receptive field (= convolution filter): 3 x 3
    - A stack of very small conv. layer (without spatial pooling in between) has the same effective receptive field as a large conv. layer (ex. A stack of two 3 x 3 = receptive field of 5 x 5, three 3 x 3 = receptive field of 7 x 7)
    - With more rectification layers instead of a single one, the decision function becomes more discriminative
    - Decreased the number of parameters at the convolutional layers while maintaining the same performance (but it exploded in the fully connected layers)
        - Three 3 x 3 has 3(3<sup>2</sup>C) = 27C weights
        - One 7 x 7 has 7<sup>2</sup>C = 49C weights

## Pros & Cons

Pros
- Deeper depth with very small convolutional filters improves accuracy
- Simple design

Cons
- Very slow to train (the original model was trained on the NVIDIA Titan GPU for 2-3 weeks) due to its high number of parameters
- takes up a lot of disk space (528 MB) and bandwidth due to its high number of weights/parameters
- potential exploding gradient problem due to its high number of parameters

## When to use

- VGG is no longer SOTA, but it's still a reliable model in small/medium dataset tasks in transfer learning and feature extraction due to its interpretability and reproducibility
- Use for perceptual features <sup>[2]</sup>
- Educational purposes

<sup>[2]</sup> Perceptual features: a type of feature that, instead of comparing images pixel by pixel (which humans don’t do), captures how humans perceive similarity — in terms of content, texture, and style.
- A pixel-level loss (like MSE) punishes even tiny shifts (e.g., moving an object 1 pixel to the right = huge error).
- But perceptually, the two images look the same.
- 
## When NOT to

- Train from scratch on large datasets because it will take too much time to train for an accuracy that is inferior to modern networks
- Deploy to edge devices or mobile
  
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
[VGG-Net Architecture Explained](https://medium.com/@siddheshb008/vgg-net-architecture-explained-71179310050f)
