# 

## Architecture
![alt text](https://github.com/khchu93/NoteImage/blob/main/resnet.jpg?raw=true) <br>

The ResNet34 uses a 34-layer plain network architecture inspired by VGG-19 in which the shortcut connection is added. It uses a convolutional filter of 7 x 7 with a stride of 2 at the first layer for a larger receptive field at the start, then switches to a 3 x 3 filter with a stride of 1 for the remaining convolutional layers. The number of fully connected layers is also reduced from 3 to 1 with the help of the global average pooling layer<sup>[4]</sup> right before the fully connected layer.


![alt text](https://github.com/khchu93/NoteImage/blob/main/res.png?raw=true) <br>
The authors also proposed different depths of ResNet from 18, 34, 50, 101, and 152 layers.

P.S. ResNet stands for Residual Network

## Key Achievements
- Enabled training of **extremely deep neural networks** (up to 152 layers at the time) by introducing **Residual learning**<sup>[1]</sup>, which fixed the vanishing/exploding gradients problem.
- Proof that the degradation problems of previous deep neural networks are not caused by overfitting or by vanishing/exploding gradients, but by optimization difficulty.
    - With simple network + identity-mapping layers (which should simply pass the input forward)
      - Theory: The deeper network should be **at least as good** as the shallow one, because the added layers only pass forward the input.
      - Result: The training and testing error **got worse** than that of the shallow network.
      - Cause:
          - Is it a **vanishing gradient** problem? No, since the added layers did nothing but pass forward the input.
          - Is it an **overfitting** issue? No, because both training and testing errors are terrible.
          - Hence, the author concluded that this issue was caused by the **optimization difficulty** in learning identity mappings.
- To solve this optimization difficulty issue, the authors introduced the **residual blocks** <sup>[2]</sup> with the use of **skip connection** <sup>[3]</sup>.

<img src="https://github.com/khchu93/NoteImage/blob/main/skipConnection.png?raw=true" alt="Skip Connection" width="500"/> <br>

**Skip connections** ensure that training deep networks won’t get worse than shallow ones due to optimization difficulty. Even when **the gradient $\frac{\partial F(x)}{\partial x}$ becomes very small, the identity term $I$ ensures gradients never vanish completely**. This will ensure a deeper model will perform at least as good as a shallower model.
And in practice, they often perform better, because extra layers can learn useful residuals on top of the identity <br> 

<sup>[1]</sup> Residual learning: a concept that **learn the residual (difference) from the identity instead of the full mapping directly**, which is more difficult to learn. <br>
<sup>[2]</sup> Residual blocks $\frac{\partial L}{\partial x} = \frac{\partial L}{\partial y} \Big( \frac{\partial F(x)}{\partial x} + I \Big)$
: the practical unit (layers + skip connection<sup>[3]</sup> packaged together). <br>
<sup>[3]</sup> Skip connection: the direct connection that “skips over” some layers and adds the input back to the output of those layers. <br>
<sup>[4]</sup> Global Average Pooling (GAP): a pooling layer that reduces each feature map to a single number by averaging over its spatial dimensions.
- H x W x C -> 1 x 1 x C (one value per channel), where H = height, W = width, C = number of channels

## Pros & Cons

Pros
- Easier gradient flow that mitigates vanishing gradient with skip connections

Cons
- Computational cost with deep ResNet(101-152 layers) are still very heavy in FLOPS and memory
- Many channels in residual blocks may be redundant
- Large initial convolutional filter (7 x 7) + stride 2 can lose some fine-grained spatial details early

## When to use
- serves as a baseline for research because it’s well-understood and reproducible
- with small to medium datasets where training extremely large models is not feasible (often used in medical imaging or industrial applications)
- default backbones for many computer vision tasks


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
[Deep Residual Learning for Image Recognition](https://arxiv.org/pdf/1512.03385)

