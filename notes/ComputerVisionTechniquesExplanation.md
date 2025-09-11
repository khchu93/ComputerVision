Dying ReLU: 
- A situation when neurons get stuck outputting 0 forever, because their inputs fall into the negative side of ReLU and gradients vanish there.
- Solution:
  1. lower learning rate: Reduces the chance of large jumps into the dead zone.
  2. Other activation functions like Leaky ReLU
