A model that takes in images and text and generates embeddings in a common space.

**What it is**
A model with two encoders:
- image encoder
- text encoder

Output of the encoders: an embedding vector. Image and text encoder are "aligned" or in the same space.

# Training

### Overview

Need a dataset of many (image, text caption) pairs.

Image and text embeddings from the same pair should be similar - high dot product.

Embeddings from different pairs should be different. 

Form a matrix, rows are image embeddings, columns are text embeddings.

Diagonal cells should have high value, off diagonal should be low value.

![[Pasted image 20260606000937.png|567]]
### Training Loss

pass 