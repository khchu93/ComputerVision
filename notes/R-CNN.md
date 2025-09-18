
# R-CNN

## Motivation
- Prove that the success of CNNs in ImageNet classification could extend to object detection tasks, specifically on Pascal VOC.
- To achieve that, there are two problems:
- 1. Localizing objects with a deep network
     - Previous solution: Sliding Windows
     - - But due to its very large receptive field (195 x 195) and stride (32 x 32) (referring to the AlexNet here), it is prone to make precise localization of the boundaries very challenging.
       - Of course, it is possible to use smaller receptive fields and stride = 1, but the computational cost would explode 
     - Proposed solution: Region proposal method that generates a fair amount (around 2000) of region proposals, then applies CNN on each region proposal and classifies each with category-specific linear SVMs.
  2. Training a high-capacity model with only a small quantity of annotated detection data
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
    - - Upsampling: Bilinear Interpolation
      - Downsampling: Interpolation
    - Pass through the selected model to extract a 4096-dimensional feature vector
  4. Set of class-specific linear SVMs
    - The extracted feature vectors are then fed into a separate machine learning classifier for each object class of interest. T-CNN typically uses SVMs for classification. For each class, a unique SVM is trained to determine whether or not the region proposal contains an instance of that class.
    - During training, positive samples are regions that contain an instance of the class. Negative samples are regions that do not.
  5. Bounding box regression
    - To improve the localization of the detected objects, a bounding box regressor is used to refine the coordinates of the region proposals.
    - For each class, R-CNN learns a linear regression model that predicts a refined bounding box
  6. Apply Non-Maximum Suppression (NMS)
    - Eliminate duplicate or highly overlapping bounding boxes
    - NMS ensures that only the most confident and non-overlapping bounding boxes are retained as final object detections by keeping the most confident box for each object and removing boxes that overlap too much with it (if IoU > threshold)

<sup>[1]</sup> Selective Search = Segments the input image based on metrics such as color and then hierarchically groups the small segments according to similarity metrics (for example, segments with identical colors are grouped) until the objects in the image are fully grouped. Irregular regions are turned into a rectangular region proposal by taking the min/max of the region's pixel coordinates. All the regions that appear during the merge become the **candidate proposals**.

## Key Achievements
- First successful application of CNNs to object detection
- Application of transfer learning for detection: supervised pretraining on a large dataset (ImageNet) followed by fine-tuning on small detection
- Introduced the Bounding box regression module to refine localization

## Pros & Cons

Pros
- Accurate object detection: excels in scenarios where precise object localization and recognition are crucial
- Robustness to object variations: translation (small shift), scale (through region proposal + reshape), illumination (somewhat) invariance
- Flexibility: By modifying the final layers of the network, you can tailor R-CNN to suit your specific needs

Cons
- Computational complexity: lots of steps, and it can be slow and resource-demanding
- Slow inference: due to the sequential processing of region proposals
- Not end-to-end/complex: it involves separate modules for region proposal and classification, which can lead to suboptimal performance compared to models that optimize both tasks jointly

## When to use
- Educational purpose
- Object detection
- Baseline model

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
[What is R-CNN?](https://blog.roboflow.com/what-is-r-cnn/)
[Rich feature hierarchies for accurate object detection and semantic segmentation Tech report (v5)](https://arxiv.org/pdf/1311.2524)
