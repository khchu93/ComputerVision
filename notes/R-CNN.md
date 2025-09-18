
# R-CNN

## Motivation
- Prove that the success of CNNs in ImageNet classification could extend to object detection tasks, specifically on Pascal VOC.
- To achieve that, there are two problems:
- 1. Localizing objects with a deep network
     - Previous solution: Sliding Windows
     - - But due to its very large receptive field (195 x 195) and stride (32 x 32) (referring to the AlexNet here), it is prone to make precise localization of the boundaries very challenging.
       - Of course, it is possible to use smaller receptive fields and stride = 1, but the computational cost would explode 
     - Proposed solution: Region proposal method that generates a fair amount (around 2000) of region proposals, then applies CNN on each region proposal and classifies each with category-specific linear SVMs.
  3. Training a high-capacity model with only a small quantity of annotated detection data
     - Previous solution: use unsupervised pre-training to learn the initial weights, followed by supervised fine-tuning
     - Proposed solution: supervised pre-training on a large auxiliary dataset (ImageNet), followed by fine-tuning on the small detection dataset (VOC) = Transfer learning
## Architecture
![alt text](https://github.com/khchu93/NoteImage/blob/main/rcnn.png?raw=true) <br>

- R-CNN consists of three modules
- 1. Generates category-independent region proposals
    - Region proposal method: **Selective search**<sup>[1]</sup>
  3. Large convolutional neural network that extracts a fixed-length feature vector from each region
    - Expand the bounding box by a fixed pixel size (e.g., p=16) in all directions equally to include context
    - Reshape it to match the expected input size of the corresponding model (e.g., AlexNet 277 x 277 pixels, VGG 224 x 224 pixels) to use the pretrained weights.
    - - Upsampling: Bilinear Interpolation, Downsampling: Interpolation
    - Pass through the selected model to extract a 4096-dimensional feature vector
  4. Set of class-specific linear SVMs
    - 

<sup>[1]</sup> Selective Search = Segments the input image based on metrics such as color and then hierarchically groups the small segments according to similarity metrics (for example, segments with identical colors are grouped) until the objects in the image are fully grouped. Irregular regions are turned into a rectangular region proposal by taking the min/max of the region's pixel coordinates. All the regions that appear during the merge become the **candidate proposals**.

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
