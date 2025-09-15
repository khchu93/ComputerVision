# GoogLeNet (/Inception v1)

## Motivation
- The most straightforward way of improving the performance of deep neural networks is by increasing their size: the depth (number of layers) and the width (number of units at each level). However, this solution comes with 2 major drawbacks:
- 1. Bigger size = Larger number of parameters = more prone to overfitting, especially if the number of labeled examples in the training set is limited
  2. Increased size = dramatically increased use of computational resources = takes lots of time to train and deploy
- The main hallmark of this GoogLeNet is the improved utilization of the computing resources inside the network by introducing the Inception architecture, designed to capture multi-scale features efficiently while keeping the parameter count small.
  
## Architecture
![alt text](https://github.com/khchu93/NoteImage/blob/main/googlenet.jpg?raw=true) <br>

![alt text](https://github.com/khchu93/NoteImage/blob/main/googlenetArchitecture.PNG?raw=true) <br>



## Key Achievements
- Introduced **Inception Module**<sup>[1]</sup> with learnable filters to handle multiple scales to **deal with the vanishing gradient (multi scales) and the explosion in computational requirements problem (dimension reductions)**.
- Used of **1 x 1 Convolution** as a dimension reduction module to **remove computational bottlenecks and also the depth of the networks** without a significant performance penalty
- Applied **[Global average pooling](https://github.com/khchu93/ComputerVision/blob/main/notes/ResNet.md?plain=1)** to **reduce the total number of parameters and to minimize overfitting**.
- Applied **auxiliary classifiers**<sup>[2]</sup> only during training to **mitigate the vanishing gradient problem**.

<sup>[1]</sup> Inception module: a module that allows for the **parallel processing of data at multiple scales**, which allows the network to **capture features at different scales more efficiently** than previous architectures.
- The inception module (naive) is made up of 4 paths:
- 1. **1 x 1 convolution**: a feature reducer, and captures local information 
  2. **3 x 3 convolution**: capture medium-range spatial patterns
  3. **5 x 5 convolution**: capture large-range spatial patterns
  4. **Max pooling**: provides translation invariance and reduces sensitivity to exact position
  5. (Optional) **Extra 1 x 1 convolution** (with dimension reduction) can be added to **reduce the increased computations**
<img src="https://github.com/khchu93/NoteImage/blob/main/inceptionNaive.webp?raw=true" alt="Inception Naive" width="600"/> <br>
<img src="https://github.com/khchu93/NoteImage/blob/main/inceptionDimentionReduc.webp?raw=true" alt="Inception Dimention Reduction" width="600"/> <br>

<sup>[2]</sup> Auxiliary classifiers: intermediate classifiers that are only used during training to overcome the vanishing gradient problem.
- They are placed strategically, where the depth of the feature extracted is sufficient to make a meaningful impact, but before the final prediction from the output classifier.
- The structure of the classifiers is:
- 1. An average pooling layer with a 5 x 5 window and a stride of 3
  2. A 1 x 1 convolution layer for dimension reduction with 128 filters/channels
  3. A fully connected layer with 1024 units/neurons
  4. A dropout layer with a 50% rate
  5. A fully connected layer with the number of classes in the task
  6. A softmax layer to output the prediction probabilities
- During training, their loss gets added to the total loss of the network with a discount weight (weighted by 0.3). Thus, these auxiliary classifiers help the gradient flow and do not diminish too quickly as they propagate back through the deeper layers.
- It also encourages the network to distribute its learning across different parts of the network, preventing the network from relying too heavily on specific features or layers, which reduces the chances of overfitting, thus helping with model regularisation.
  
## Pros & Cons

Pros
- Parameter efficient, only ~5M parameters (vs VGG ~138M), thanks to 1×1 convolutions + GAP
- Multi-scale feature extraction, captures both fine and coarse features simultaneously
- Deep network with reduced vanishing gradient issues

Cons
- Complex architecture, multiple branches per inception module = harder to implement/debug
- Not ideal for tasks requiring spatial precision (ex. segmentation, detection without modification)

## When to use
- Large scale image classification where efficiency matters
- When you want multi-scale feature extraction without exploding parameters

## When NOT to use
- Precise spatial localization (ex. segmentation)
- Extremely deep networks (>50–100 layers) — ResNet is easier to train

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
[Going Deeper with Convolutions](https://arxiv.org/abs/1409.4842) <br>
[GoogLeNet: A Deep Dive into Google’s Neural Network Technology](https://medium.com/@siddheshb008/googlenet-a-deep-dive-into-googles-neural-network-technology-f588d1b49e55)
