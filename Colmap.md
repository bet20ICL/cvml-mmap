
## Schema

Rigs
- rig ID


Cameras
- camera ID
- camera model
- camera params
	- width, height, focal length, principal point, etc.

Images
- image ID
- name
- camera ID

Keypoints
- image ID
- rows
	- number of keypoints in image
- cols
	- length of keypoint metadata
	- 2: only x,y
	- 4: x, y, scale, orientation
	- 6:
- data blob
	- R x C binary

Descriptors
- image ID
- type
- rows
	- number of keypoints in image
- cols
	- length of keypoint descriptor
- data blob
	- R x C binary


> [!NOTE] Pair ID
> Pair Id is a **single** id that uniquely identifies a tuple of image IDs
> 
> We don't store (img_id_a, and img_id_b), instead 
> 
> pair_id = N x img_id_a + img_id_b
> 
> Where N is a large number approx. 2 billion
> 
> Intuition: a grid of NxN where each cell is a unique pair

Matches
- pair_id
- rows
	- number of putative matches for the image pair
- cols
	- always 2
- data blob
	- R x 2 array
	- Each row is a putative match
	- left column is 0-indexed keypoint id in img A, right col is keypoint id in img B

Two View Geometries
- pair_id
- rows
	- number of **inlier** matches, strictly less than corresponding row in Matches
- cols
	- always 2
- data blob
	- R x 2 array
	- Inlier matches 
- F (blob)
	- fundamental matrix
- E (blob)
	- essential matrix
- H (blob)
	- homography matrix
- qvec
- tvec

## Feature Detection and Matching

Colmap typically supports different sparse methods:
[[Sparse Feature Detection & Matching]]

To use dense features, need hloc
[[Dense Feature Matching]]

- 
## Geometric Verification

- estimate fundamental (F), essential (E) or homography matrix (H)
-

## Point Triangulation

### Solver

This is the core mechanism.

Input:
- two cameras (camera model/params)
- camera poses
- two observations (two u,v pairs which are putative matches)

Estimate the 3D location of the point seen by both cameras. Should be the intersection of the two camera rays from the observations. 

Method: DLT + non-linear refinement

Output:
- 3d points

This can be extended to N cameras and N observations.

## Pipeline

Pseudo-code


> [!NOTE] Track
> A 3d point and a set of 2d keypoints it was triangulated from. The 2d keypoints will have been matched together by some feature matching method.


```python

def get_track_from_matches(
	keypoint,
	matches,
	tri_max_transitivity=N,
	tri_max_error_px=M,
):
	# Find the best track within error and max number of hops
	return track

def solve(cameras, ):
	pass


def reproj_error(track, uv):
	pass

def triangulate_image(image):
	for keypoint in image.keypoints:
		matches = graph.find_matches(keypoints)
		# Ideally all matches for the keypoint have the same track, 
		# but there may be multiple (i.e. match A, B, C belong to 3D point X but
		# D, E belong to 3D point Y). In such cases find the 3D point giving the
		# smallest reprojection error and add it to that track.
		track = get_track_from_matches(keypoint, matches)

		if track is not None and reproj_error(trakc, keypoint) < threshold:
			track.add(keypoint)
			continue

		cameras = cameras_from_matches(matches)	
		point3d = solve(cameras, matches)

		track = Track(point3d, matches)
		tracks.add(track)

def point_triangulator():
	# initial sweep
	for image in images:
		triangulate_image(image)

	# global refinement and BA
	
	
	
```

