# 

## Architecture
![alt text](https://github.com/khchu93/NoteImage/blob/main/resnet.jpg?raw=true) <br>



P.S. ResNet stands for Residual Network

## Key Achievements
- Enabled training of extremely deep neural networks (up to 152 layers at the time) by introducing Residual learning<sup>[1]</sup>, which fixed the vanishing/exploding gradients problem.
- Proof that the degradation problems of previous deep neural networks are not caused by overfitting or by vanishing/exploding gradients, but by optimization difficulty.
    - With simple network + identity-mapping layers (which should simply pass the input forward)
      - Theory: The deeper network should be at least as good as the shallow one, because the added layers only pass forward the input.
      - Result: The training and testing error got worse than that of the shallow network.
      - Cause:
          - Is it a vanishing gradient problem? No, since the added layers did nothing but pass forward the input.
          - Is it an overfitting issue? No, because both training and testing errors are terrible.
          - Hence, the author concluded that this issue was caused by the optimization difficulty in learning identity mappings.
- To solve this optimization difficulty issue, the authors introduced the residual blocks <sup>[1]</sup>.

<sup>[1]</sup> Residual blocks: the practical unit (layers + skip connection<sup>[2]</sup> packaged together).
<sup>[2]</sup> Skip connection: the direct connection that “skips over” some layers and adds the input back to the output of those layers.

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

