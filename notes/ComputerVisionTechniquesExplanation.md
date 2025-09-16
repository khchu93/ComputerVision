Exploding/Vanishing gradient
- A problem where gradients vanish/explode when inputs are in the saturated regions (close to 0 or 1 for sigmoid, ±1 for tanh).
- This affects all earlier layers globally — the signal decays as it propagates backward (the whole layers receive almost no gradient).
- Solution:
  1. Regularization
  2. Initialization
  3. Normalization
  4. Residual connections
  5. Gradient clipping
  6. Other activation functions like ReLU

Dying ReLU: 
- A problem when neurons get stuck outputting 0 forever (for every training input), because their inputs fall into the negative side of ReLU and gradients vanish there.
- This mainly affects the neuron itself (it gets stuck at 0 output and 0 gradient), and it cuts off gradient flow through that neuron’s connections.
- Solution:
  1. Lower learning rate: Reduces the chance of large jumps into the dead zone.
  2. Other activation functions like Leaky ReLU
 
Convolutional layer
- A convolution (especially 1×1) learns how to re-weight and recombine the input channels, emphasizing the useful features and suppressing the irrelevant ones.
- A standard convolution (including 1×1), each filter always spans all input channels at the same time.

Feature descriptor
- Encode local image information into a compact, distinctive, and invariant representation, so we can reliably match or recognize objects across different scenes.
- Feature descriptors are used to reduce the data to process and to achieve invariance (scale, rotation, illumination, and viewpoint invariances).

Invariance
- The ability of the model or descriptor to “see through” transformations and still recognize the underlying structure or object as if it were the original image.

HOG (Histogram of Oriented Gradients)
- A feature descriptor that extracts the gradient orientation (magnitude + orientation) at each pixel of the input images.
- 1. Magnitude = the size of the intensity change. The larger the change = the larger the intensity.
  2. Orientation = the direction of the change of pixel intensity = perpendicular to the edge and points toward brighter regions.
- Steps
- 1. Reshape the image into a size where the width and height can be divided by the selected cell size (e.g. 8 x 8, 16 x 16 cell)
  2. Calculate the magnitude and orientation for each pixel with the immediate neighbor or 3 x 3 sobel operator.
  - 1. Immediate neighbor: Gx = (x+1, y) - (x-1, y), Gy = (x, y+1) - (x, y-1), magnitude = sqrt(Gx<sup>2</sup>, Gy<sup>2</sup>), orientation = atan(Gy/Gx)
    2. Sobel: Gx kernel: [-1 0 1; -2 0 2; -1 0 1], Gy kernel: [-1 -2 -1; 0 0 0; 1 2 1], Gx = pixel window x Gx kernel, Gy = pixel window x Gy kernel, magnitude = ...
  3. Calculate the histogram of gradients in the selected cell size (e.g. 8 x 8 cell) with a bin size of 20 (0°–180° split into 9 bins)
  4. 
