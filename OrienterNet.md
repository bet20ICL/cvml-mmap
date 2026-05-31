# Intro

A model that takes two inputs:
- ground perspective image (query image to localise)
- an OSM maptile

Goal: find where the query image is on the OSM tile.

Task: [[Cross View GeoLocalisation]]

# Training

Ground images are in a different domain to OSM tiles (they look very different). 

Data needed:
- ground images with ground truth position labels (lat, lon, heading)
- **gravity direction**
- 

Therefore use contrastive learning and InfoNCE loss.
- [[Contrastive Learning]]
- [[InfoNCE Loss]]

### References
- https://github.com/facebookresearch/OrienterNet

### Contributors
#Meta #ETHZ #PSarlin