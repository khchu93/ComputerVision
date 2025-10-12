
# U-Net

## Motivation
Before U-Net, segmentation in biomedical imaging was often done using patch-based methods like **sliding-window** CNNs. These models could **only classify small image regions one at a time**, which caused two major issues: they were **slow** because the network had to process many overlapping patches, and they **struggled to balance context and precision**—large patches gave more context but hurt localization, while small patches improved precision but missed the bigger picture. On top of that, deep learning models typically **require thousands of labeled images** to train well, but biomedical datasets are usually **very small** because annotations are **expensive and time-consuming**. Another recurring problem was that previous methods **struggled to separate touching or overlapping objects**, like cells in microscopy images.

To solve these challenges, the authors proposed **`U-Net`**, a fully convolutional architecture designed for **pixel-level prediction** instead of whole-image classification. It combines a **contracting path** to capture context with a **symmetric expanding path** to precisely localize objects, eliminating the need for slow patch-by-patch processing. To deal with limited training data, U-Net relies heavily on **data augmentation**, allowing it to learn effectively from just a few annotated samples. The authors also introduced a **weighted loss function** that forces the network to pay extra attention to the **narrow borders between touching objects**, helping it separate them more accurately. Overall, U-Net was designed to be fast, data-efficient, and precise—making it especially effective for **biomedical image segmentation where labeled data is scarce and pixel-level accuracy is essential**.

## Architecture
The overall network is fundamentally divided into two symmetrical parts: a **contracting path** on the left side, which is designed to <u>progressively capture context from the input image</u>, and an **expansive path** on the right side, which enables precise localization of the segmentation boundaries. A critical feature of U-Net is that the expansive path **incorporates a large number of feature channels**, allowing the network to effectively propagate context information to higher resolution layers. 

The entire network avoids the use of any fully connected layers, relying solely on convolutions, and leverages an overlap-tile strategy to enable the seamless segmentation of arbitrarily large images. In total, the U-Net architecture consists of **23 convolutional layers**.

Here is the step-by-step description of the U-Net architecture:

### Contracting Path (Encoder)
The contracting path follows the typical architecture of a convolutional network, designed to extract high-level feature maps and context.
1. Repeated Convolution and Activation (Process input and build feature maps)
• This step involves the repeated application of two  convolutions (which are unpadded convolutions, resulting in a loss of border pixels).
• Each convolution is immediately followed by a Rectified Linear Unit (ReLU) activation function.
2. Downsampling (Reduce resolution and double feature channels)
• Following the two convolution/ReLU operations, a  max pooling operation with a stride of 2 is applied for downsampling.
• At every downsampling step, the number of feature channels is doubled.
3. Context Capture (Repeat sequence)
• Steps 1 and 2 are repeated four times (corresponding to four resolution levels) to progressively capture higher-level context, leading to the bottom layer of the 'U'.
### Expansive Path (Decoder)
The expansive path is designed to combine low-resolution context information with high-resolution features from the contracting path to achieve precise localization.
4. Upsampling and Halving Channels (Increase resolution)
• Every step in the expansive path begins with an upsampling of the feature map.
• This upsampled map is followed by a  convolution (referred to as an “up-convolution”) which serves to halve the number of feature channels.
5. Concatenation and Feature Transfer (Reintroduce high-resolution detail)
• The resulting feature map from Step 4 is concatenated with the correspondingly cropped feature map copied from the contracting path. This transfer of features (represented by white boxes in the architecture diagram) is crucial for localization.
• Cropping is necessary because the use of unpadded convolutions in the contracting path causes a loss of border pixels, meaning the corresponding feature maps do not have identical dimensions.
6. Refined Convolution and Activation (Combine context and detail)
• The concatenated output then undergoes two successive  convolutions, each followed by a ReLU.
7. Localization and Feature Assembly (Repeat sequence)
• Steps 4, 5, and 6 are repeated four times (corresponding to four resolution levels) as the network progresses back towards the original input resolution.
8. Final Classification (Map features to class labels)
• At the final layer of the network, a  convolution is used.
• This convolution maps each multi-component feature vector (for instance, a 64-component vector) to the desired number of classes for the final pixel-wise segmentation output.

## Key Achievements
- 

## Pros & Cons

Pros
- 

Cons
-

<!--
## Implementation
- Framework: 
- Dataset: 
- Colab Notebook: [link]()

## Results
Training

Validation

Examples:
-->

## References
[U-Net: Convolutional Networks for Biomedical Image Segmentation](https://arxiv.org/pdf/1505.04597)
