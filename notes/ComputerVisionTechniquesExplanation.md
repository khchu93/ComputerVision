Exploding/Vanishing gradient
- A problem where gradients vanish/explode when inputs are in the saturated regions (close to 0 or 1 for sigmoid, ±1 for tanh).
- This affects all earlier layers globally — the signal decays as it propagates backward (the whole layers receive almost no gradient).
- Solution:
  1. Regularization
  2. Other activation functions like ReLU

Dying ReLU: 
- A problem when neurons get stuck outputting 0 forever (for every training input), because their inputs fall into the negative side of ReLU and gradients vanish there.
- This mainly affects the neuron itself (it gets stuck at 0 output and 0 gradient), and it cuts off gradient flow through that neuron’s connections.
- Solution:
  1. lower learning rate: Reduces the chance of large jumps into the dead zone.
  2. Other activation functions like Leaky ReLU
