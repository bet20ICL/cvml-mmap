---
tags: [concept, training, representation-learning]
---

# Contrastive Learning

A self-supervised (or weakly supervised) learning paradigm that trains encoders by pulling representations of semantically similar pairs together and pushing dissimilar pairs apart in embedding space.

## Core Intuition

Given a query embedding $q$ and a set of keys $\{k^+, k^-_1, k^-_2, \ldots\}$, learn an embedding space where:
- $\text{sim}(q, k^+)$ is maximised (positive pair — same semantic content)
- $\text{sim}(q, k^-)$ is minimised (negative pair — different content)

Similarity is typically cosine similarity or dot product.

## Key Loss Functions

### InfoNCE (Noise Contrastive Estimation)

$$\mathcal{L} = -\log \frac{\exp(\text{sim}(q, k^+)/\tau)}{\sum_{i=0}^{N} \exp(\text{sim}(q, k_i)/\tau)}$$

- $\tau$ is a temperature hyperparameter controlling the sharpness of the distribution
- Training pushes the positive pair to dominate the softmax over all negatives in the batch
- Used in MoCo, SimCLR, CLIP

### NT-Xent (Normalised Temperature-scaled Cross Entropy)

The symmetric version of InfoNCE — loss is computed from both directions (query→key and key→query). Used in SimCLR.

### Sigmoid Loss (SigLIP variant)

$$\mathcal{L} = -\frac{1}{N}\sum_{i,j} y_{ij} \log \sigma(z_{ij}) + (1-y_{ij})\log(1-\sigma(z_{ij}))$$

where $z_{ij} = \beta \cdot \text{sim}(q_i, k_j) + b$ and $y_{ij} = 1$ iff $i = j$.

- Treats each pair **independently** — no global softmax normalisation over the batch
- Avoids the "false negative" problem of large-batch InfoNCE (where a true positive might appear as a negative)
- Scales better to very large batch sizes
- See [[SigLIP]]

## CLIP Framework

CLIP (Radford et al., 2021) trains a paired image encoder and text encoder using InfoNCE on 400M internet image-text pairs.

- Batch matrix of similarity scores: $N \times N$
- Diagonal is positives; off-diagonal is negatives
- Loss is symmetric: images-to-texts + texts-to-images

Enables zero-shot classification and rich visual representations reusable downstream.
## Related Concepts

- Metric learning (triplet loss is an older precursor)
- Self-supervised learning (contrastive learning is one branch)
- Representation learning
