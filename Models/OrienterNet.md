---
tags: [model, localisation, bev, contrastive]
---

# OrienterNet

**OrienterNet: Visual Localization in 2D Public Maps with Neural Matching**
Sarlin et al., Meta AI / ETH Zurich, CVPR 2023.

## Problem

Given a single ground-level image, estimate the 6-DoF pose (or at minimum the 2D position + orientation) of the camera within a large-scale public map (OpenStreetMap).

This is **cross-view localisation**: matching a perspective street-level image against a top-down semantic map — two very different visual domains.

## Architecture

```
Ground image
    │
    ▼
Image Encoder (CNN/ViT)
    │ perspective features
    ▼
BEV Projection Head
    │ flattened BEV feature map  (H × W × C)
    ▼
    ┌──────────────────────────────┐
    │  Cosine similarity matching  │
    └──────────────────────────────┘
    ▲
    │ map tile features (H × W × C)
Map Encoder
    │
OpenStreetMap raster tile
```

1. **Image encoder** extracts features from the input image
2. **BEV projection** transforms perspective features into a top-down feature map — without explicit depth; the projection is learned end-to-end
3. **Map encoder** encodes an OSM raster tile (roads, buildings, land use rendered as a semantic image) into the same feature space
4. **Matching** computes cosine similarity between the BEV feature map and all possible positions + orientations in the map tile → produces a probability distribution over pose

## Training: Contrastive Objective

The model is trained with a contrastive loss:
- **Positive**: the (image BEV features, map tile at the true pose) pair
- **Negatives**: all other (position, orientation) combinations in the batch

This forces the BEV feature map to be semantically consistent with the top-down map representation, bridging the **ground ↔ aerial domain gap** without any depth supervision.

The key insight: you don't need to reconstruct the scene geometry — you just need the BEV representation to **match** the map in feature space.

See: [[Contrastive Learning]]

## Why This Works Without Depth

Classical BEV methods (LSS, BEVFormer) explicitly predict depth to lift features into 3D. OrienterNet skips this: the contrastive training signal is strong enough to implicitly teach the BEV projection what structure corresponds to what map feature.

See: [[Concepts/BEV Localisation]]

## Limitations

- Map staleness: OSM may not reflect recent construction
- Textureless environments (tunnels, highways) are harder
- Large-scale retrieval still needed to shortlist candidate areas before fine localisation

## Related

- [[Visual Localisation]] — the broader task
- [[Contrastive Learning]] — core training paradigm
- [[Concepts/BEV Localisation]] — the representation
