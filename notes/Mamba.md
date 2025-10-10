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
Mamba’s architecture replaces the Transformer’s attention-MLP stack with a unified, input-aware state space module that processes sequences in linear time. Instead of using fixed Linear Time-Invariant (LTI) dynamics like earlier SSMs, Mamba makes core parameters — such as the state update, input mapping, and output projection — dependent on the current token embedding, enabling selective information propagation and content-based reasoning. These selective SSM blocks are stacked in a homogeneous fashion and computed efficiently using a parallel scan algorithm, which replaces the quadratic cost of attention with linear-time recurrence while fully leveraging GPU memory hierarchy.

Steps:
1. Embed the Input
   - The model starts by converting raw tokens (like words, bytes, or audio frames) into dense vectors using an embedding layer. This is necessary because the rest of the architecture works in continuous vector space rather than on raw IDs. After embedding, the tensor typically has shape (**batch, sequence length, hidden dimension**) — this becomes the main representation passed through the model.
2. Set Up the Base State Space Dynamics
   - Mamba builds on state space models, so it defines core components like:
      - A: how hidden states evolve over time
      - B: how input influences the state
      - C: how the state is read back out
      - Δ: a time-step factor
    - These define how information flows along a sequence, similar to RNNs but more structured and efficient. They form the “skeleton” that all later processing depends on, and their dimensions must match the hidden/state sizes used throughout the model.
3. Make Parameters Depend on the Input
   - Unlike traditional LTI (Linear Time-Invariant) state space models where parameters are fixed, Mamba makes key parameters — $\Delta$, $\mathbf{B}$ and $\mathbf{C}$ — functions of the current token’s embedding. This means each position in a sequence can influence how the system updates and reads memory. It allows the model to be selective and context-aware, something older SSMs and RNNs struggled with. These input-conditioned parameters are produced via learned projections, so their shapes must align correctly across the batch and sequence.
4. Filter and Retain Relevant Information
   - As the model processes each token, it uses the input-conditioned parameters to decide what to keep, update, or ignore in the state. This acts like a content-aware gate that helps the model remember important parts of the sequence and skip irrelevant information. This replaces attention’s “focusing” ability without the quadratic cost and is crucial for working with long texts while avoiding noise.
5. Process Sequences with a Fast Parallel Scan
   - Once parameters are dynamic, Mamba can’t use the old convolution trick from earlier SSMs. Instead, it uses a hardware-friendly parallel scan algorithm to update states efficiently across the sequence. This keeps computation linear in sequence length — $O(L)$ — and enables up to 5× faster inference than Transformers, while scaling to sequences as long as 1 million tokens. This design also makes better use of GPU cache and memory.
6. Replace Attention and MLP with a Unified Block
   - Instead of alternating between attention layers and feedforward networks (like Transformers do), Mamba stacks a single type of block: a selective SSM layer plus a small projection (replacing the MLP). Residual connections and normalization keep training stable. This simplification removes the need for attention entirely while keeping the expressiveness needed for language and other dense data. Input and output shapes are preserved so multiple blocks can be stacked easily.
7. Generate the Final Output
   - These outputs let us measure performance directly against Transformers. In practice, Mamba matches or outperforms Transformer models that are twice its size, while being faster and handling longer context windows.
   
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
