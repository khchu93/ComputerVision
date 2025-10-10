# Mamba

## Motivation
Current large language models are dominated by the **`Transformer`** architecture, which provides **strong performance but struggles with long sequences due to the quadratic cost of self-attention and limited context windows**. Attempts to replace attention with faster alternatives, such as **`Linear Time-Invariant (LTI)`** state space models, achieve **linear scaling but fail to match Transformer-level accuracy on dense modalities** like language because their **fixed dynamics prevent content-based reasoning and selective information propagation based on the input**. This prevents them from filtering out irrelevant context.

Recently, **`structured state space sequence models (SSMs)`** have emerged as a promising class of architectures for sequence modeling. These models can be interpreted as a **combination of RNNs and CNNs, with inspiration from classical state space models**. This class of models can be computed very efficiently as either a recurrence or a convolution, with **linear or near-linear scaling in sequence length**, especially for modeling long-range dependencies. Except, this new class is not without flaws. They have been **less effective at modeling discrete and information-dense data** such as text.

To achieve the modeling power of Transformers while scaling linearly in sequence length, the authors propose a new class of SSMs, **`Mamba`**.<br>

The core proposals are:
- **Selection Mechanism**: introduce a selection mechanism by allowing key SSM parameters ($\Delta$, $\mathbf{B}$, $\mathbf{C}$) to be functions of the input.
  - This crucial change removes the LTI constraint, enabling the model to filter out irrelevant information and remember relevant information indefinitely, thus allowing it to perform content-based reasoning essential for discrete modalities like language.
- **Hardware-aware Algorithm**: Because the input-dependent parameters mean the model can no longer rely on efficient convolutional mode computation, the authors developed a hardware-aware parallel algorithm that computes the model recurrently using a scan operation
  - This implementation is crucial for efficiency, scaling linearly in sequence length ($O(L)$ FLOPs), offering significantly faster throughput than Transformers (up to 5× higher inference throughput), and being memory-efficient by leveraging the GPU memory hierarchy (SRAM over HBM).
- **Simplified Mamba Architecture**: They integrate these selective SSMs into a simplified end-to-end neural network architecture that removes attention or MLP blocks (which typically interleave attention).
  - The Mamba architecture uses a simple, homogeneous block design that combines the function of prior SSM architectures with the MLP block of Transformers into a single, stacked unit
 
Mamba achieves state-of-the-art performance across several modalities (language, audio, genomics), demonstrating strong quality (matching Transformers twice its size on language modeling) and efficiency (linear scaling up to sequence length 1M).

## Architecture

## Key Achievements
- 

## Pros & Cons

Pros
- 

Cons
-

<!--
## Implementation
- Framework: 
- Dataset: 
- Colab Notebook: [link]()

## Results
Training

Validation

Examples:
-->

## References
[Mamba: Linear-Time Sequence Modeling with Selective State Spaces](https://arxiv.org/pdf/2312.00752)
