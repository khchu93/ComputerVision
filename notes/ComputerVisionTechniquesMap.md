[Techniques Explanation](https://github.com/khchu93/ComputerVision/blob/main/notes/ComputerVisionTechniquesExplanation.md)

- Normalized initialization
- intermediate normalization layers

- Activation function
    - Sigmoid
        - Exploding/Vanishing gradient
    - Tanh
        - Exploding/Vanishing gradient
    - ReLU (Rectified Linear Unit): fixed vanishing gradient, but caused dying ReLU
        - Dying ReLU
    - Leaky ReLU: fixed dying ReLU, but possibly reduces the "sparse activation" regularization effect of ReLU

- Feature extraction technique
    - [HOG](https://www.analyticsvidhya.com/blog/2019/09/feature-engineering-images-introduction-hog-feature-descriptor/) (Histogram of oriented gradients): a feature descriptor that produces a histogram of gradient directions in an image
    - SIFT (TBD)
