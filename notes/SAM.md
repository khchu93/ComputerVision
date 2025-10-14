# Segment Anything Model (SAM)

## Motivation
The motivation for the **Segment Anything (SA)** project stemmed directly from a **deficiency in data abundance within computer vision (CV)** compared to Natural Language Processing (NLP). While foundation models (FMs) for NLP achieved strong zero-shot generalization by training on abundant web-scale datasets, **a natural web-scale data source for segmentation masks simply did not exist**. CV includes a wide range of problems beyond vision and language encoders, and for many of these, including segmentation, the necessary abundant training data was missing. Therefore, the central problem was **how to build a foundation model for image segmentation that generalizes powerfully via prompt engineering without an existing massive dataset of segmentation masks**.

To fix this issue, the Segment Anything project proposed **three interconnected components**: 
- A new Task
  -  The promptable segmentation task is introduced. The goal is to return a valid segmentation mask given any segmentation prompt (e.g., points, boxes, text). This task serves as a general pre-training objective that enables zero-shot transfer via prompt engineering.
- A new Model
  - The **Segment Anything Model (SAM)** is designed to be efficient, support flexible prompts, and allow for real-time interactive use. Its architecture separates a heavy image encoder from a fast prompt encoder and lightweight mask decoder, allowing the image embedding cost to be amortized across multiple prompts.
- A Data Engine/Dataset
  - To overcome the lack of web-scale mask data, the authors built a data engine. This engine co-developed the model with model-in-the-loop dataset annotation. By iterating through assisted-manual, semi-automatic, and fully automatic stages, the team collected the Segment Anything 1 Billion (SA-1B) dataset, which contains over 1 billion masks on 11 million licensed images.

These components together enable SAM to transfer zero-shot to new image distributions and tasks, often competitively or superiorly compared to prior fully supervised results.

## Architecture
![alt text](https://github.com/khchu93/NoteImage/blob/main/segment-anything-model.png) <br>

The Segment Anything Model (SAM) is an efficient, promptable segmentation model designed to transfer zero-shot to new tasks via prompt engineering. Its architecture is explicitly designed to support flexible prompts, handle ambiguity, and allow for amortized real-time interactive use. The unique design separates the computational burden: a heavyweight image encoder is run once per image to create an embedding, and this cost is amortized across multiple prompts that are processed by a much faster prompt encoder and lightweight mask decoder. This separation allows the model to predict a mask from a prompt in approximately 50ms in a web browser once the image embedding is computed. SAM is also ambiguity-aware, predicting multiple valid masks for a single, potentially ambiguous, prompt.

Here is the step-by-step description of the U-Net architecture:
1. Image Encoder
   - A **Vision Transformer (ViT)** pre-trained with **Masked Autoencoders (MAE)** is used to compute a high-resolution image embedding. This component is computationally expensive, but is run only once per image, and its output is a 16× downscaled embedding of the input image.
2. Prompt Encoder (Sparse Prompts): This module handles sparse inputs (points, boxes, text).
    - Points and Boxes are represented by positional encodings combined with learned embeddings specific to the prompt type (e.g., foreground/background for points, or top-left/bottom-right corners for boxes).
    - **Free-form Text** is encoded using an off-the-shelf text encoder, such as the one from CLIP.
3. Prompt Encoder (Dense Prompts)
   - Dense inputs (i.e., masks) are embedded using convolutions and then summed element-wise with the image embedding.
4. Lightweight Mask Decoder
   - This component efficiently maps the image embedding and prompt embeddings to output masks. The decoder uses a modification of a Transformer decoder block that includes prompt self-attention and cross-attention in two directions (token-to-image and image-to-token) to update all embeddings.
5. Ambiguity Resolution
   - The model is designed to predict multiple output masks (typically three) for a single ambiguous prompt, and it also predicts a confidence score (estimated IoU) for each mask to rank them.
  
This efficient, component-based design is what allows SAM to achieve real-time mask prediction and robust zero-shot generalization.

## Key Achievements
- Proposed the Web-Scale Segmentation Dataset (SA-1B)
  - The project fundamentally addressed the lack of web-scale data for segmentation masks by successfully building a Data Engine that co-developed the model with model-in-the-loop annotation. This massive effort resulted in the Segment Anything 1 Billion (SA-1B) dataset, which contains over 1 billion masks on 11 million licensed images. SA-1B is the largest segmentation dataset by a wide margin, containing 400× more masks than the largest prior dataset, Open Images.
- Powerful Zero-Shot Generalization via Prompting
  - SAM's core achievement is its impressive zero-shot transfer ability, allowing it to perform segmentation on novel data distributions and tasks without fine-tuning. This capability is enabled by training on the promptable segmentation task, making SAM respond appropriately to any prompt, such as points or boxes. Human studies confirmed that SAM’s mask quality, even from a single foreground point, was consistently rated substantially higher than the leading interactive segmentation baseline, RITM. This zero-shot protocol successfully transfers to diverse tasks like instance segmentation and edge detection via prompt engineering.
- Highly Efficient, Ambiguity-Aware Model Architecture
  - SAM's design achieves amortized real-time interactive use through a unique separation of computation. It uses a heavyweight Image Encoder once per image to compute an embedding, and then a lightweight Prompt Encoder and Mask Decoder quickly process subsequent prompts. This efficiency allows SAM to predict a mask from a prompt in approximately 50ms in a web browser. Additionally, SAM is built to be ambiguity-aware, predicting multiple valid masks (typically three) and a confidence score for each input to handle complex or vague prompts.


## Pros & Cons

Pros
- Massive Data Scale and Diversity
- Powerful Zero-Shot Generalization

Cons
- Misses fine structures, occasionally hallucinates small disconnected components
- Zero-Shot Biases and AP Gaps

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
