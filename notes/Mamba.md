# Mamba

## Motivation
Current large language models are dominated by the <u>Transformer</u> architecture, which provides **strong performance but struggles with long sequences due to the quadratic cost of self-attention and limited context windows**. Attempts to replace attention with faster alternatives, such as <u>**Linear Time-Invariant (LTI)**</u> state space models, achieve **linear scaling but fail to match Transformer-level accuracy on dense modalities** like language because their **fixed dynamics prevent content-based reasoning and selective information propagation**. The authors argue that a next-generation model should combine three key qualities:

1. The expressive power of Transformers to handle complex dependencies.
2. True linear-time scalability for very long sequences.
3. The ability to selectively focus on relevant inputs while filtering out irrelevant information.

To address these requirements, the authors propose Mamba, a streamlined architecture built on the Selective State Space Model (S6). The core innovation is that key SSM parameters (Δ, B, C) are input-dependent rather than fixed, enabling content-aware filtering, long-term memory retention, and dynamic, attention-like behavior. Because this breaks the convolutional shortcut used in earlier SSMs, the authors design a hardware-aware parallel scan algorithm that preserves linear-time complexity, leverages GPU memory efficiently, and significantly boosts throughput.

The resulting Mamba architecture replaces both attention and MLP blocks with a unified selective SSM module in a simple, homogeneous block design. Performance highlights include:

- Up to 5× faster inference than Transformers
- Ability to process sequences up to 1 million tokens
- Matches or surpasses Transformers twice its size on language modeling

This combination of efficiency, scalability, and content-aware reasoning makes Mamba a strong alternative to traditional Transformer-based LLMs.

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
