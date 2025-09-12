# 

## Architecture
![alt text](https://github.com/khchu93/NoteImage/blob/main/resnet.jpg?raw=true) <br>



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
And in practice, they often perform better, because extra layers can learn useful residuals on top of the identity.




<sup>[1]</sup> Residual learning: a concept that **learn the residual (difference) from the identity instead of the full mapping directly**, which is more difficult to learn. <br>
<sup>[2]</sup> Residual blocks $\frac{\partial L}{\partial x} = \frac{\partial L}{\partial y} \Big( \frac{\partial F(x)}{\partial x} + I \Big)$
: the practical unit (layers + skip connection<sup>[3]</sup> packaged together). <br>
<sup>[3]</sup> Skip connection: the direct connection that “skips over” some layers and adds the input back to the output of those layers.

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
[Deep Residual Learning for Image Recognition](https://arxiv.org/pdf/1512.03385)

