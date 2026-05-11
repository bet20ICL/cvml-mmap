---
tags: [concept, representation, perception]
---

# Bird's Eye View (BEV)

A 2D spatial representation of a scene projected onto the ground plane, as if viewed from directly above. Coordinates are typically metric (metres from the ego vehicle or a reference point).

## Why BEV is Useful

- **Geometry-friendly**: objects have consistent scale regardless of distance (unlike perspective images)
- **Sensor fusion**: LiDAR, radar, and camera outputs can all be expressed in the same BEV grid
- **Downstream tasks**: path planning and localisation naturally operate in 2D metric space
- **Map alignment**: OpenStreetMap and HD maps are themselves 2D — BEV features can be directly compared to map features

## View Transformation (Perspective → BEV)

Converting monocular or multi-camera perspective images to BEV is a core challenge:

### Lift-Splat-Shoot (LSS)
- Predicts a depth distribution per pixel, "lifts" features to 3D frustum, then "splats" down onto BEV grid
- Fully differentiable; learned end-to-end
- Used in BEVFusion, BEVDepth, and many others

### BEVFormer (transformer-based)
- Deformable attention queries BEV grid positions back into image features
- Handles multi-camera rigs naturally
- Widely used in autonomous driving (nuScenes SOTA)

### Inverse Perspective Mapping (IPM)
- Classical approach assuming a flat ground plane — projects image to BEV via homography
- Fast but brittle: fails on uneven terrain and tall objects

### OrienterNet approach
- Does not use explicit depth — instead learns to produce BEV feature maps that are semantically aligned with aerial/map representations via contrastive training
- See [[OrienterNet]]

## Relationship to Localisation

BEV representations make localisation tractable: if you can produce a BEV feature map from an image and have a database of map tiles in the same representation, localisation reduces to **2D matching**.

See: [[Visual Localisation]], [[OrienterNet]]
