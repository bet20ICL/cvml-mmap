
**MegaDepth** 
- CVPR2018, Noah Snavely (Cornell)
- ~200 scenes, each a real world landmark
- ~1k images per scene
	- intrinsics
	- dense depth map
- NOT metric scale
- high level view:
	- Run Colmap sfm + MVS
- used as a two-view matching benchmark
- [[Question]]: do we get GT 2d-2d correspondences between images?
	- Is it by backprojecting depth into the scene to get scene coordinates? Two pixels are correspondences if scene coordinates are close enough? 

**Aerial MegaDepth**
- CVPR2025
- Goal: get accurate co-registered ground + aerial images
- 


