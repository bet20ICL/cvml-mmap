---
tags: [model, vision-language, encoder, contrastive]
---

# SigLIP

**Sigmoid Loss for Language-Image Pre-Training**
Zhai et al., Google, 2023. ([arXiv:2303.15343](https://arxiv.org/abs/2303.15343))

## What It Is

SigLIP is a vision-language model trained similarly to CLIP but replacing the softmax-based InfoNCE loss with a **sigmoid binary cross-entropy loss** applied independently to each image-text pair.

## Key Difference from CLIP

| | CLIP | SigLIP |
|---|---|---|
| Loss | InfoNCE (softmax over batch) | Sigmoid BCE per pair |
| Normalisation | Global: requires knowing all negatives | Local: each pair is independent |
| Batch dependency | High — performance degrades at small batches | Low — scales smoothly |
| False negatives | Problematic at large batches | Less sensitive |

The CLIP loss computes softmax over an entire batch row — so every image must "compete" against all texts. SigLIP instead asks: for this (image, text) pair, is it a match? This binary framing sidesteps the global normalisation and makes distributed training across many devices easier.

## Architecture

- ViT-based image encoder (various sizes: So400m is common)
- Text encoder (transformer)
- Trained on large-scale noisy image-text data (LAION, internal Google data)

## Why VLAs Use SigLIP

SigLIP encoders produce rich, spatially-aware visual representations that transfer well to downstream tasks. Most open VLAs (OpenVLA, pi0, Octo variants) use a **frozen** SigLIP ViT as the visual backbone — the LLM backbone then operates on the resulting patch tokens.

Freezing the encoder:
- Preserves the rich generalisation from large-scale pretraining
- Reduces the number of trainable parameters
- Allows the policy to be trained on smaller robot datasets without catastrophic forgetting

## Related

- [[Contrastive Learning]] — the training paradigm SigLIP improves upon
- [[VLA - Vision Language Action Models]] — primary downstream use in this vault
