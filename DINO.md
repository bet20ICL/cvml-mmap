*Self-distillation without labels*

**Context**

We want a model that takes an image, returns a high-dimensional vector / embedding.

This embedding should be a good summary of the image. 

This embedding should help other tasks.

One way of getting good embeddings: pre-training a model backbone on a different task.

For example, using the backbone of a model trained on [[ImageNet]] classification improved performance on other tasks.

Downsides of image classification approach:
- too simple - a single label doesn't capture that much details, so training on image classification doesn't give a super strong embedding
- requires labelled data, which is
	- limited
	- needs to be manually curated

Another idea is to use text captions to capture more details of the image: [[CLIP]]

(image, caption) pairs are more available, since they can be scraped from the internet

Question: **can we learn strong image representations in a self supervised way?**
- i.e. without text captions?

# DINOv1

Create different sized crops of the same image
- global views: larger crops
- local views: smaller crops

All views of the same image should have a similar embeddings. 

# DINOv2

# DINOv3

Headline fix: **Gram Anchoring**

Problem: patch level features degrading over training. Not useful for dense tasks (prediction needed per patch)

# Questions

How does this compare to [[JEPA]] approaches? Feels very similar.
# Topics

- [[Contrastive Learning]]
- [[Self Supervised Learning]]
