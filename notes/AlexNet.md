# AlexNet

## Architecture

![alt text](https://github.com/khchu93/NoteImage/blob/main/AlexNet_Architecture.PNG?raw=true)

[AlexNet](https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf) is an 8-layer CNN model which consists of : five convolutional layers followed by 3 fully connected layers (2 hidden layers + 1 output  layer). 

The first convolutional layer has a [convolution window shape]() of 11 x 11, then in the second layer, it is reduced to 5 x 5, followed by 3 x 3 for the rest of the convolutional layers. Moreover, the first convolutional layer has a [stride]() of 4, then reduced to 1 for the rest of the convolutional layers. 

The reason for using an 11 x 11 filter with stride 4 is to aggressively reduce the spatial size of the input, which in turn lowers the number of multiply-add operations, keeping the computational cost within a manageable range for the GPUs available at the time.

AlexNet used dual GPUs because running all 96 filters in the first convolutional layer on a single GPU exceeded its memory capacity. To solve this, a channel-wise split was applied, with each GPU computing 48 feature maps. The outputs were then merged before the fully connected layers, allowing the classifier to see all extracted features.

P.S. Max pooling is not counted as a layer because it is not a learnable layer.

## Key Achievements
- Introduced a very deep CNN with 8 layers and ~60M parameters, trained on a large-scale dataset (ImageNet)
- Used a ReLU activation function instead of sigmoid/tanh
  - Fixed the vanishing gradient and slow convergence issue by not saturating for large positive inputs[1] = faster training
- Used Dropout in fully connected layers
  - Prevented co-adaptation[2] of weights by randomly deactivating neurons during training = reduced overfitting
-  Used of Data Augmentation
  - Artificially increased the dataset diversity (random crops, flips, color jitter, etc) = improved generalization and robustness of the model

[1] Sigmoid saturation: When the input saturates, the output approaches its maximum, causing the gradient to become very small and close to 0 = vanishing gradient <br>
[2] Co-adaptation: Situation when neurons/weights reply too much on specific other neurons to produce the correct output. Less robust netwoek because one neuron fails, others cannot compensate = lead to overfitting
## Pros & Cons

## When to use

## Implementation

## Results

## References
[ImageNet Classification with Deep Convolutional Neural Networks](https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf) <br>
[alexnet](https://colab.research.google.com/github/d2l-ai/d2l-en-colab/blob/master/chapter_convolutional-modern/alexnet.ipynb#scrollTo=1a22e154) <br>
