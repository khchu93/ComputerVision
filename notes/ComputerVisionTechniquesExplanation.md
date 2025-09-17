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
- Encode local image information into a **compact, distinctive, and invariant representation**, so we can reliably match or **recognize objects across different scenes**.
- Feature descriptors are used to **reduce the data to process** and to **achieve invariance** (scale, rotation, illumination, and viewpoint invariances).

Invariance
- The ability of the model or descriptor to **“see through” transformations** and still **recognize the underlying structure** or object as if it were the original image.

[HOG](https://www.analyticsvidhya.com/blog/2019/09/feature-engineering-images-introduction-hog-feature-descriptor/) (Histogram of Oriented Gradients)
- A feature descriptor that extracts the **gradient orientation** (magnitude + orientation) at each pixel of the input images for image comparison
- 1. Magnitude = the size of the intensity change. The larger the change = the larger the intensity.
  2. Orientation = the direction of the change of pixel intensity = perpendicular to the edge and points toward brighter regions.
- When to use
  - **Much faster and lighter** than SIFT/ORB/SURF
  - When images don't vary too much in rotation or scale (robust with small shifts)
  - Works well when objects are roughly the same size and orientation
  - Partially invariant to illumination (norm), translation (sliding block), and scale (if computed at multiple scales)
- Steps
- 1. Reshape the image into a size where the width and height can be divided by the selected cell size (e.g. 8 x 8, 16 x 16 pixels)
  2. Calculate the magnitude and orientation for each pixel with the immediate neighbor or 3 x 3 sobel operator.
  - 1. Immediate neighbor: Gx = (x+1, y) - (x-1, y), Gy = (x, y+1) - (x, y-1), magnitude = sqrt(Gx<sup>2</sup>, Gy<sup>2</sup>), orientation = atan(Gy/Gx)
    2. Sobel: Gx kernel: [-1 0 1; -2 0 2; -1 0 1], Gy kernel: [-1 -2 -1; 0 0 0; 1 2 1], Gx = pixel window x Gx kernel, Gy = pixel window x Gy kernel, magnitude = ...
  3. Divide image into cells: small, connected regions (e.g., 8 x 8 pixels)
  4. Calculate the histogram of gradients in the selected cell size (e.g., 8 x 8 pixels) with a bin size of 20° (0°–180° split into 9 bins, only up to 180° because the direction doesn't matter): 8 x 8 inputs => 9 x 1 inputs
  - - There are multiple ways to do it, a simple way would be to sum up the magnitude of gradients that fall into the same bin.
  5. Normalize across blocks (with block of cells varying from 2 x 2 cells (= 16 x 16 pixels) to larger) and slide through the image with step of 1, to reduce the lighting variation from the image that affects the gradient of the image
  - - There are multiple ways to do it, a simple way would be to divide each of these values by the square root of the sum of squares of the values.
  6. Concatenate all block vectors and flatten them into one long **HOG feature vector**.

[SIFT](https://www.analyticsvidhya.com/blog/2019/10/detailed-guide-powerful-sift-technique-image-matching-python/) (Scale Invariant Feature Transform) Algorithm
- A feature detection algorithm that detects distinctive key points or features in an image that are robust to changes in scale, rotation, and affine transformation.
- It works by identifying **keypoints** based on their **local intensity extrema** and computing **descriptors** that capture the local image information around those key points.
- When to use
  - When rotation or scale invariants are needed
- Steps
- 1. Constructing a scale space while ignoring any noise and ensuring that the features are not scale-dependent
  - - Create ideally up to four **octaves**, and for each octave, use **Gaussian blur** to create four blur images, each with a different σ value.
    - Apply **DoG (difference of Gaussian)** that creates another set of images with enhanced features by subtracting every image from the previous image in the same scale.
  3. Keypoint localization that can be used for feature matching
  - 1. Find the local maxima and minima
    - - Find the **extrema**/potential keypoints by going through every pixel in the images and comparing its neighboring pixels (not only neighbors on the same image, but also the previous and next image in the same octave)
    2. Remove low contrast keypoints (keypoint selection) or keypoints that have low contrast (these keypoints are not robust to noise, hence eliminate)
    - - use second-order Taylor expansion to eliminate those that are close to the edge and have a high edge response but may not be robust to a small amount of noise
      - use second-order Hessian matrix to eliminate those may not be robust to a small amount of noise
  4. Orientation assignment
  - 1. Take a patch aroundd the keypoint(size proportional to keypoint scale σ: 3 x σ)
  - 2. Calculate magnitude and orientation for each pixel in the patch
    3. Create a histogram of 36 bins with a bin size of 10° (0°–360° split into 36 bins) and assign dominant orientation
    - - The bin with the highest peak = dominant orientation (exact orientation is calculated with interpolation equation)
      - if other peaks >= 80% of the max peak height, it creates a new keypoint at the same location & scale, but assigned that secondary orientation
  5. keypoint descriptor
  - 1. Take a 16×16 window around the keypoint (scaled according to σ and rotated to keypoint orientation).
    2. Divide the patch into 4×4 subregions → 16 subregions total.
    3. For each subregion:
    - - Compute gradient magnitude and orientation at each pixel
      - Build a 8-bin orientation histogram (covering 360°)
    4. Concatenate the 16 subregion histograms (16×8) → 128-D descriptor vector
    5. Normalize the vector to unit length → reduces effect of illumination changes
    6. Threshold large values (e.g., >0.2) and renormalize → increases robustness to non-linear lighting changes

Keypoints
- Local features in an image that are scale & rotation invariants.

Octave
- a group of images created at progressively doubled scales/resolution
- So:
  - Octave = set of images of the same size but different blurs (σ values)
  - Next octave = image downsampled by half (reduced resolution)
 
DoG (difference of Gaussian)
- a feature enhancement algorithm that creates another set of images, for each octave, by subtracting every image from the previous image in the same scale.

Descriptor
- a vector describing local patch around a keypoint, designed so that it’s distinctive, compact, and robust for matching or recognition.
