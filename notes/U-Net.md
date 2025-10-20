
# U-Net

## Motivation
Before U-Net, segmentation in biomedical imaging was often done using patch-based methods like **sliding-window** CNNs. These models could **only classify small image regions one at a time**, which caused two major issues: they were **slow** because the network had to process many overlapping patches, and they **struggled to balance context and precision**—`large patches gave more context but hurt localization, while small patches improved precision but missed the bigger picture`. On top of that, deep learning models typically **require thousands of labeled images** to train well, but biomedical datasets are usually **very small** because annotations are **expensive and time-consuming**. Another recurring problem was that previous methods **struggled to separate touching or overlapping objects**, like cells in microscopy images.

To solve these challenges, the authors proposed **`U-Net`**, a fully convolutional architecture designed for **pixel-level prediction** instead of whole-image classification. It combines a **contracting path** to capture context with a **symmetric expanding path** to precisely localize objects, eliminating the need for slow patch-by-patch processing. To deal with limited training data, U-Net **relies heavily on data augmentation**, allowing it to learn effectively from just a few annotated samples. The authors also introduced a **weighted loss function** that forces the network to pay extra attention to the **narrow borders between touching objects**, helping it separate them more accurately. Overall, U-Net was designed to be fast, data-efficient, and precise—making it especially effective for **biomedical image segmentation** where **labeled data is scarce and pixel-level accuracy is essential**.

## Architecture
![alt text](https://github.com/khchu93/NoteImage/blob/main/unet.png) <br>

The overall network is fundamentally divided into two symmetrical parts: a **contracting path** on the left side, which is designed to **progressively capture context** from the input image, and an **expansive path** on the right side, which **enables precise localization** of the segmentation boundaries. A critical feature of `U-Net` is that the **expansive path incorporates a large number of feature channels**, allowing the network to effectively propagate context information to higher resolution layers. 

The entire network **avoids the use of any fully connected layers, relying solely on convolutions**, and leverages an **overlap-tile strategy**<sup>[1]</sup> to enable the seamless segmentation of arbitrarily large images. In total, the U-Net architecture consists of **23 convolutional layers**.

[1] : **Overlap-tile strategy** is an implementation technique used when **processing images too large to fit in memory**. It allows U-Net to **handle arbitrarily large images while preserving accuracy at patch boundaries**.<br>
    - This strategy is used in applications like biomedical or satellite imaging, where input images can be much larger (e.g., 2048×2048) than U-Net’s fixed input size (512×512).<br>
    - Steps: Divide the image into **overlapping tiles**, process each tile **through U-Net**, and then merge them by **averaging or weighting the overlapping regions**.<br>
    
Here is the step-by-step description of the U-Net architecture:
#### Contracting Path (Encoder)
The contracting path follows the typical architecture of a convolutional network, designed to **extract high-level feature maps and context**.
1. **Repeated Convolution and Activation** (Process input and build feature maps)<br>
    - This involves the repeated application of **two unpadded convolutions**, each followed by a **`Rectified Linear Unit (ReLU)`**, where `the use of unpadded convolutions ensures that the segmentation map only contains the pixels for which the full context is available in the input image`, as border pixels are lost in every convolution.
2. **Downsampling** (Reduce resolution and double feature channels)<br>
    - Following the convolution sequence, a  **max pooling** operation with a **stride of 2** is applied for downsampling, which **progressively captures broader context by reducing spatial resolution**, while simultaneously **doubling the number of feature channels** at each step.
3. **Context Capture** (Repeat sequence)<br>
    - The sequence of **Steps 1 and 2 is repeated four times** to **condense the input into deep, coarse feature representations**.

#### Expansive Path (Decoder)
The expansive path is designed to **combine low-resolution context information with high-resolution features** from the contracting path to **achieve precise localization**.

4. **Upsampling and Halving Channels** (Increase resolution)<br>
    - Every step begins with an **upsampling of the feature map**, followed by a **convolution** (called an **“up-convolution”**), which **increases the resolution of the output** while serving to **halve the number of feature channels**.
5. **Concatenation and Feature Transfer** (Reintroduce high-resolution detail)<br>
    - The resulting upsampled feature map is **concatenated with the correspondingly cropped feature map copied from the contracting path**, a crucial step that `allows the network to localize precisely by combining the coarse context with fine-grained, high-resolution features from earlier layers`.
6. **Refined Convolution and Activation** (Combine context and detail)<br>
    - The concatenated output then undergoes **two successive convolutions**, each **followed by a `ReLU`**, allowing the network to **learn to assemble a more precise output** based on this newly combined information.
7. **Localization and Feature Assembly** (Repeat sequence)<br>
    -  The sequence of **Steps 4, 5, and 6 is repeated four times** as the network **restores the spatial resolution** towards the original input size.
8. **Final Classification** (Map features to class labels)<br>
    - At the ultimate layer, a  **convolution** is used to **map each multi-component feature vector (e.g., 64 components) to the desired number of classes**, thereby generating the final **pixel-wise segmentation map**.

## Key Achievements
- Introduced a **Symmetric U-Shaped Architecture** with Feature Combination
  - The architecture was modified and extended from the fully convolutional network, featuring a **contracting path to capture context** and a **symmetric expanding path that enables precise localization** through the **concatenation of high-resolution feature maps** from the contracting path with upsampled feature maps from the expansive path.
- Enabled **Seamless Segmentation** via **Overlap-Tile Strategy**
  - The network was designed **without fully connected layers**, using only the valid part of each convolution, which allows for the seamless segmentation of **arbitrarily large images** by employing an **overlap-tile strategy**.
- Implemented Strong **Data Augmentation** via **Elastic Deformations**
  - To cope with limited training data, U-Net uses excessive data augmentation by **applying random elastic deformations** to the available training images, which is the key concept that allows the network to **learn invariance to tissue deformations** without needing to see these transformations in the annotated image corpus.
- Proposed a **Weighted Loss Function** for **Border Separation**
  - To address the challenge of separating touching objects of the same class, the authors proposed using a weighted loss function that **introduces a pre-computed weight map**, $w(x)$, to the cross-entropy loss, **assigning a large weight to the background labels** separating touching cells to force the network to learn these small separation borders.
- Utilized **Specialized Weight Initialization**
  - The network uses an initialization scheme where weights are **drawn from a Gaussian distribution** with a standard deviation of $\sqrt{\dfrac{2}{n}}$ (where $N$ is the number of incoming nodes of one neuron) to **ensure that each feature map maintains approximately unit variance**, preventing excessive or non-contributing network activations in deep architectures.

## Pros & Cons

Pros
- Precise localization
  - The **skip connections** between encoder and decoder allow the network to **combine high-resolution spatial features with deep semantic features**, producing accurate segmentation boundaries.
- Works well with small datasets
  - **Extensive data augmentation** helps prevent overfitting, making it effective with **small datasets**.
- Fully convolutional
  - **No fully connected layers** → **arbitrary input sizes** (with patch-based strategies) and **fewer parameters** compared to networks with FC layers.
- Efficient feature reuse
  - The symmetric architecture and skip connections allow **features learned in the encoder to be reused in the decoder**, improving learning efficiency.

Cons
- Fixed input size
  - Standard U-Net is designed for **fixed-size inputs** (e.g., 512×512). Images of different sizes must be **resized** or **patched**, which can introduce artifacts or extra preprocessing steps.
- High memory usage
  - U-Net has **many feature channels** and **skip connections**, which can consume a lot of GPU memory, especially for high-resolution images.
- Limited receptive field
  - U-Net has a **limited receptive field** and **fixed input size** (e.g., 512×512). For large images, **overlapping patches** can be processed and merged, but this still **limits the network’s ability to capture global context across the full image**.
- Parameter-heavy
  - Deep U-Nets contain around **31.3 million parameters**. **Adding an extra convolutional layer at the bottleneck** with 1024 channels adds roughly 9.4 million parameters, **increasing the model size by about 30%**. This larger size can lead to longer training times and a **higher risk of overfitting**, especially on small datasets.
    
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
