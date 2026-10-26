## Setup

For a given area, we have this data:
- NavVis scans
	- LiDAR pointcloud
	- posed camera images
		- LiDAR SLAM

- iPhone/iPad captures
	- posed camera images
		- ARKit
	- bunch of sensor data

- Hololens 2 captures
	- posed camera images
	- bunch of sensor data
## Goal

Co-register everything for benchmarking localisation for AR applications

**Motivation:**
Existing datasets are not good enough for xyz purposes...
## Approach

Assumptions:
- LiDAR SLAM is perfect, no drift
- ARKit is locally good, but has long term drift

Stages:
### NavVis
- Co-register LiDAR captures - get a **reference map**
	- a
	- b
	- Run meshing using pointcloud, create texture using images
	- Obtain a reference map
		- posed reference images
		- aligned mesh
#### PGO
Use Open3D pose graph formulation

### Device

- Register device data to the **reference map**
	- Initial Localisation
	- Rigid Alignment
	- PGO
	- Guided Localisation
	- PGO & BA

#### Initial Localisation
- retrieve the top k reference (NavVis) images for some number of device images 
	- [[Question]]: how do they subsample the device images?
- 2d-2d matching and P3P + LO-RANSAC to localise device image to reference images

#### Rigid Alignment
- some kind of voting scheme

#### PGO
- minimise errors of odometry factors + localisation factors
- get new estimates of poses

#### Guided Localisation
- run localisation on the device frames again
- this time, retrieve reference images by poses + visual overlap rather than by netvlad
	- avoid failures due to retrieving incorrect but visually similar reference images
- repeat PGO with new localisation priors

#### PGO & BA
- 

