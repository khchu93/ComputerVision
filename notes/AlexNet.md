# AlexNet

## Architecture

![alt text](https://github.com/khchu93/NoteImage/blob/main/AlexNet_Architecture.PNG?raw=true)

[AlexNet](https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf), an 8-layer CNN consist of: five convolutional layers followed by 3 fully connected layers (2 hidden layers + 1 output  layer). 

The first convolutional layer has a [convolution window shape]() of 11 x 11, then in the second layer, it is reduced to 5 x 5, followed by 3 x 3 for the rest of the convolutional layers. Moreover, the first convolutional layer has a [stride]() of 4, then reduced to 1 for the rest of the convolutional layers. 

P.S. Max pooling is not counted as a layer because it is not a learnable layer.

## Key Achievements

## Pros & Cons

## When to use

## Implementation

## Results

## References
[ImageNet Classification with Deep Convolutional Neural Networks](https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf) <br>
[alexnet](https://colab.research.google.com/github/d2l-ai/d2l-en-colab/blob/master/chapter_convolutional-modern/alexnet.ipynb#scrollTo=1a22e154) <br>
