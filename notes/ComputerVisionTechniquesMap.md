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

- Feature descriptor (feature extraction technique)
    - [HOG](https://www.analyticsvidhya.com/blog/2019/09/feature-engineering-images-introduction-hog-feature-descriptor/) (Histogram of oriented gradients)
    - [SIFT](https://www.analyticsvidhya.com/blog/2019/10/detailed-guide-powerful-sift-technique-image-matching-python/)  (Scale Invariant Feature Transform) Algorithm
    -  - DoG (difference of Gaussian)
       - Gaussian blur
       - Octave
       - Descriptor
    - SURF
 
- Problem
  - Vanishing gradient
  - Exploding gradient
  - Internal covariate shift
    - Normalization
      - Batch Normalization
      - Layer Normalization
