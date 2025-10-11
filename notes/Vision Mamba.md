# Vision Mamba (Vim)

## Motivation
The primary motivation for introducing Vision Mamba (Vim) was to develop a **generic and efficient visual backbone** built purely upon the advancements of State Space Models (SSMs), specifically the Mamba deep learning model, while overcoming the following limitation:
1. **ViT Inefficiency**: Vision Transformers (ViTs) have achieved great success by providing data/patch-dependent global context through self-attention. However, the **self-attention mechanism poses challenges in terms of speed and memory usage** when dealing with long-range visual dependencies, such as processing high-resolution images, because of its quadratic complexity.
2. **SSM Limitations for Vision**: While the Mamba deep learning model showed great potential for long sequence modeling in language, a generic pure-SSM-based backbone network had not been explored for visual data.

The authors propose Vision Mamba (Vim), a new generic vision backbone that integrates SSMs into a sequential, pure-SSM architecture optimized for visual understanding.


## Architecture
The **`Vision Mamba`** architecture extends the original **Mamba (S6) framework** from sequential language modeling to computer vision, introducing a new paradigm for efficiently processing long-range spatial dependencies in images. Unlike Mamba, which operates on 1D token sequences, Vision Mamba treats images as sequences of visual tokens (patches) and leverages **2D state space modeling** to capture both local and global context across the spatial dimensions. The key innovation lies in **adapting the S6 backbone with convolutional state transitions and selective gating tailored for visual data**, allowing the model to handle high-resolution inputs with linear computational complexity. By integrating these modifications, Vision Mamba preserves the memory and speed advantages of S6 while enabling effective feature extraction for vision tasks, distinguishing it from the original Mamba designed primarily for NLP.

Steps:
1. Embed the Input Image
   - Vision Mamba starts by dividing the raw input image into fixed-size patches and projecting each patch into a dense vector. This is necessary because the model operates on sequences of vectors rather than raw pixels, similar to how Mamba processes token embeddings for text. The output tensor has shape (batch, num_patches, hidden_dim), forming the main representation that passes through the SSM layers.
2. Set Up Base State Space Dynamics
   - The model defines core SSM components — A, B, C, and Δ — which govern how hidden states evolve, how patch inputs influence the state, and how outputs are read. This structured framework is the backbone for all subsequent processing, providing a linear-time mechanism to propagate information efficiently across the patch sequence, just like in Mamba.
3. Make Parameters Depend on the Input
   - Unlike older LTI SSMs, Vision Mamba makes key parameters (Δ, B, C) dependent on each patch embedding, so the model can selectively propagate, update, or ignore information based on the content of each patch. This input-aware mechanism is directly carried over from Mamba, enabling context-sensitive processing for visual sequences just as it does for text.
4. Filter and Retain Relevant Information
   - As each patch passes through the selective SSM, the input-conditioned parameters act as content-aware gates that determine what information to keep, update, or discard. This allows the model to focus on meaningful visual features while ignoring irrelevant noise, replacing attention’s “focusing” ability without incurring quadratic complexity.
5. Process Patch Sequences with Parallel Scan
   - Because the parameters vary per patch, Vision Mamba cannot rely on fixed convolution shortcuts and instead uses a hardware-aware parallel scan algorithm to compute the hidden states efficiently. This ensures linear-time processing across all patches, enabling fast inference on high-resolution images and maintaining the same efficiency principle as the original Mamba.
6. Capture Spatial Context with Cross-Scan Modules
   - Vision Mamba introduces **`Cross-Scan Modules`** that process patches along multiple directions (e.g., top→bottom, left→right) to capture global spatial relationships. This step is new compared to Mamba, as images have 2D spatial structure, and it allows the model to encode long-range dependencies across both height and width while still using linear-time SSM computation.
7. Replace Attention and MLP with Unified SSM Blocks
   - The selective SSM blocks are stacked with a small projection layer, residual connections, and normalization, forming a homogeneous unit that replaces separate attention and feedforward layers. This simplifies the architecture while retaining expressive power and makes it easy to stack multiple blocks for deeper modeling.
8. Generate Task-Specific Outputs
   - Finally, the processed patch representations are fed into heads tailored for visual tasks, such as image classification or segmentation. This is a modification compared to Mamba, which outputs language tokens; in Vision Mamba, the sequence-based SSM processing is reused, but the output is adapted to vision-specific objectives.

## Key Achievements
- **Adapted State Space Models to Vision** by enabling linear-time processing of visual inputs while preserving long-range spatial dependencies—something Transformers handle with quadratic cost.
- Introduced **Cross-Scan Mechanism** for 2D Structure, which achieves global receptive fields and long-range visual reasoning without self-attention, significantly reducing memory and computation costs.
- **State Space Blocks Replace Attention and MLP**, replace attention + feedforward layers with unified selective SSM blocks.

## Pros & Cons

Pros
- Linear-Time Scaling
- Handles Long-Range Dependencies Efficiently
- Strong Performance with Fewer Parameters

Cons
- Relatively New and Less Battle-Tested
- Implementation Complexity
- Limited Pretrained Models & Infrastructure
- Task-Specific Optimization May Be Needed

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
