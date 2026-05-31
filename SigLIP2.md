# Intro

A model that takes in images and text and generates embeddings in a common space.

Task: [[Vision Language Encoder]]

# Training

Text and images are completely separate domains.

Therefore use contrastive learning and InfoNCE loss
- [[Contrastive Learning]]
- [[InfoNCE Loss]]

### References
- https://github.com/google-research/big_vision/blob/main/big_vision/configs/proj/image_text/README_siglip2.md