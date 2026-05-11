---
tags: [task, localisation, slam, place-recognition]
---

# Visual Localisation

## Problem

Estimate the 6-DoF pose (position + orientation) of a camera from one or more images, with respect to a known map or reference database.

## Subtasks

### Place Recognition (Global Retrieval)
Coarse localisation: retrieve the closest database image or map tile.
- NetVLAD, DINOv2 + cosine retrieval, CosPlace, EigenPlaces
- Returns a shortlist of candidates; metric accuracy is ~metres

### Metric Localisation (Local Refinement)
Fine-grained pose within a candidate region.
- Feature matching: SuperPoint + SuperGlue / LightGlue against a 3D point cloud
- Direct methods: minimise photometric or feature-space error

### Cross-View Localisation
Match images from one viewpoint (e.g. ground) to a map from another (e.g. aerial/satellite).
- Challenging domain gap — different perspective, texture, and illumination
- BEV-based approaches directly address this: [[OrienterNet]]

## BEV-Based Approaches

Converting ground images to [[Concepts/BEV Localisation|BEV]] allows direct comparison with top-down maps:
- Avoids 3D reconstruction — no need for a dense SfM map
- Maps (OSM, satellite imagery) are freely available globally
- See [[OrienterNet]] for a concrete system

## Descriptor-Based Approaches

- **NetVLAD**: aggregates local CNN features into a compact global descriptor; trained with weak GPS supervision using a contrastive triplet loss
- **DINOv2**: strong zero-shot visual descriptors from self-supervised ViT pretraining; works well for place recognition out of the box
- **CosPlace / EigenPlaces**: aggregate local features with geographical awareness

## Relationship to SLAM

Visual localisation answers "where am I in a known map?" SLAM answers "where am I and what does the map look like?" They compose:
1. SLAM builds a local map and tracks pose within it
2. Global localisation (VL) re-localises the SLAM map into a world-frame reference

## Open Problems

- Appearance change (day/night, seasons, weather)
- Large-scale efficiency (searching millions of map tiles)
- Long-term map maintenance

## Related

- [[OrienterNet]] — BEV-based cross-view localisation
- [[Concepts/BEV Localisation]]

