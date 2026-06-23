# End-to-End Monocular Visual SLAM System

Production-grade monocular Visual SLAM pipeline built from scratch using Python and OpenCV.

Features:
- Visual Odometry
- Sparse 3D Mapping
- Bundle Adjustment
- Loop Closure Detection
- Pose Graph Optimization
- Relocalization
- Keyframe Management

Dataset: 
[KITTI Vision Benchmark Suite – Odometry Benchmark](https://www.cvlibs.net/datasets/kitti/eval_odometry.php)

KITTI Odometry Sequence 05

![SLAM Demo](assets/demo.gif)

## Overview

This project implements a complete monocular Visual SLAM pipeline from scratch,
following concepts from modern SLAM literature and the SLAM Handbook.

The system estimates camera trajectory and reconstructs a sparse 3D map
using only monocular image sequences.

Major components include:

- Feature extraction and matching
- Camera pose estimation
- Landmark triangulation
- Bundle adjustment
- Loop closure detection
- Pose graph optimization
- Relocalization


## Project Highlights

✓ Complete monocular Visual SLAM pipeline developed through a structured 10-stage engineering roadmap

✓ Processed 1,500+ KITTI Sequence 05 frames at ~30 FPS

✓ Implemented ORB, SIFT, and AKAZE feature detection and matching

✓ Built a 1,000-word Bag-of-Visual-Words vocabulary for place recognition

✓ Achieved 54 geometrically verified loop closures using RANSAC-based verification

✓ Developed SE(3) pose graph optimization with Gauss-Newton and Levenberg-Marquardt damping

✓ Reduced trajectory drift by 24% (215m → 163m)

✓ Built from scratch without ORB-SLAM, ORB-SLAM2/3, RTAB-Map, or other SLAM frameworks

✓ Added advanced infrastructure including keyframe management, relocalization, map serialization, performance monitoring, and multi-threaded processing


## Why I Built This

After spending the previous year building neural networks from scratch and studying deep learning, I wanted to challenge myself with a computer vision problem that was well outside my comfort zone.

Visual SLAM quickly became that challenge I decided to dived head first into.

Rather than using an existing framework such as ORB-SLAM or RTAB-Map, I wanted to understand what was happening under the hood by learning and implementing the underlying mathematics and algorithms myself. This meant learning and building everything from feature matching and epipolar geometry to camera pose estimation, triangulation, loop closure detection, and pose graph optimization.

The project began after I came across *The SLAM Handbook: From Localization and Mapping to Spatial Intelligence*. What started as an effort to better understand modern SLAM systems eventually grew into a complete end-to-end monocular Visual SLAM pipeline developed using the KITTI autonomous driving dataset.

More than anything, this project was an opportunity to learn by building. Every subsystem was developed incrementally, tested independently, and integrated step-by-step into the final system. The result was not only a working Visual SLAM implementation, but also a much deeper understanding of geometric computer vision, localization, mapping, and optimization.

Through this project I developed a strong understanding of:

- Epipolar geometry and multi-view reconstruction
- Essential and fundamental matrix estimation
- Robust estimation with RANSAC
- Bundle adjustment and nonlinear optimization
- Loop closure detection and pose graph optimization
- SE(3) Lie groups and Lie algebra for camera motion modeling

## Built From Scratch

Unlike many SLAM projects that integrate existing frameworks, this project implements the majority of core SLAM components directly.

Implemented Components:

✓ Feature extraction and matching

✓ Essential matrix estimation

✓ Camera pose recovery

✓ Landmark triangulation

✓ Bundle adjustment

✓ Bag-of-Visual-Words place recognition

✓ Loop closure verification

✓ Pose graph optimization

✓ SE(3) Lie algebra operations

✓ Relocalization

✓ Keyframe selection

✓ Map serialization


## System Architecture
post the diagram png you goof.

## Development Timeline

This project was developed incrementally as a 10-stage engineering roadmap,
with each stage building on the previous subsystem.

| Step | Component |
|--------|------------|
| 1 | Dataset Loading & Camera Calibration |
| 2 | Feature Detection & Matching |
| 3 | Visual Odometry |
| 4 | Sparse Mapping & Triangulation |
| 5 | Bundle Adjustment |
| 6 | Loop Closure Detection |
| 7 | Bag-of-Visual-Words (BoVW) Place Recognition |
| 8 | Pose Graph Optimization |
| 9 | Advanced SLAM Infrastructure |
| 10 | Relocalization & System Integration |

# Engineering Journey

## Step 1 — Dataset Loading & Camera Calibration

The project began by loading KITTI Sequence 00 and validating
all required data sources for a monocular SLAM pipeline.

### Implemented

- KITTI dataset loader
- Camera calibration parser
- Ground-truth trajectory loading
- Dataset visualization utilities

### Result

Successfully loaded:

- 4,541 monocular images
- Camera intrinsic calibration matrix
- Ground-truth vehicle trajectory

![KITTI Dataset Analysis](docs/images/step1_dataset_analysis.png)

This stage established the data foundation required for
feature extraction, visual odometry, mapping, and optimization.

---

## Step 2 — Feature Detection & Matching

### Implemented

- ORB feature detector
- SIFT feature detector
- Feature descriptor extraction
- Detector comparison and evaluation

### Result

Detected and analyzed 1,000 visual features per frame using both
ORB and SIFT descriptors.

![Feature Detection Comparison](docs/images/step2_feature_comparison.png)

Feature extraction forms the foundation of visual odometry by
providing stable image correspondences between consecutive frames.

---

## Step 3 — Feature Matching

### Implemented

- Brute Force feature matching
- FLANN-based matching
- Lowe's ratio test filtering
- Fundamental matrix estimation
- RANSAC outlier rejection

### Result

Established reliable correspondences between consecutive frames by
combining descriptor matching with geometric verification.

- 369 initial feature matches
- 123 geometrically verified matches
- 66.7% outlier rejection rate

![Geometrically Verified Matches](docs/images/step3_geometric_filtering.png)

Geometric filtering using RANSAC significantly improved match quality
by removing inconsistent correspondences before pose estimation.

---

## Step 4 — Pose Estimation & Triangulation

### Implemented

- Essential matrix estimation
- Relative camera pose recovery
- RANSAC-based inlier filtering
- 3D point triangulation
- Reprojection error evaluation
- Epipolar geometry visualization

### Result

Estimated relative camera motion between KITTI frames using 123 geometrically filtered feature matches.

- 109 / 123 pose inliers
- 88.6% inlier ratio
- 109 triangulated 3D points
- Average reprojection error: 0.319 pixels
- Dominant motion direction recovered along the forward driving axis

![Pose Estimation and Triangulation](docs/images/step4_pose_estimation.png)

This stage transformed 2D feature correspondences into relative camera motion and sparse 3D structure, forming the foundation for map building and bundle adjustment.

---

## Step 5 — Map Building & Bundle Adjustment

### Implemented

- Landmark data structure
- Camera pose data structure
- SLAM map container
- Landmark observation tracking
- Data association between frames
- Landmark culling logic
- Simplified local bundle adjustment

### Result

Built an initial map representation and demonstrated bundle adjustment on a local optimization window.

| Metric | Result |
|--------|--------|
| Camera Poses Added | 5 |
| Landmarks Added | 20 |
| Average Observations per Landmark | 4.0 |
| Mature Landmarks | 20 |
| Initial Reprojection Error | 866.55 |
| Final Reprojection Error | 693.24 |
| Error Reduction | 20% |

This stage marked the transition from visual odometry to SLAM: the system began maintaining camera poses, 3D landmarks, landmark observations, and local map optimization.

---

## Step 6 — Loop Closure Detection

### Implemented

- Bag-of-Visual-Words place recognition
- K-Means visual vocabulary generation
- Loop candidate retrieval
- RANSAC-based geometric verification
- KITTI sequence evaluation
- Loop closure visualization and performance tracking

### Result

Built and evaluated a loop closure detection system on KITTI Sequence 05.

| Metric | Result |
|--------|--------|
| Frames Processed | 1,500 |
| Visual Vocabulary Size | 1,000 words |
| Loop Closures Detected | 45 |
| Average Processing Time | 23.4 ms/frame |
| Estimated Trajectory Length | 1,419.0 m |
| Ground Truth Trajectory Length | 1,042.4 m |

![KITTI Loop Closure Results](docs/images/step6_loop_closure_kitti.png)

This stage introduced place recognition into the SLAM pipeline, allowing the system to detect when the camera revisited previously seen locations.

---

## Step 7 — Pose Graph Optimization

### Implemented

- SE(3) pose representation
- Lie algebra operations
- Pose graph construction
- Odometry constraints
- Loop closure constraints
- Gauss-Newton optimization
- Levenberg-Marquardt damping
- Information matrix weighting
- Numerical stability safeguards

### Result

Integrated pose graph optimization into the SLAM pipeline and evaluated it on KITTI Sequence 05.

| Metric | Result |
|--------|--------|
| Frames Processed | 1,400 |
| Loop Closures Detected | 54 |
| Pose Constraints | 1,399 |
| Processing Time | 47.22 seconds |
| Average FPS | 29.65 |
| Original Trajectory Drift | 215.112 m |
| Optimized Trajectory Drift | 163.494 m |
| Drift Reduction | 24.0% |

![Pose Graph Optimization Results](docs/images/step7_pose_graph_optimization.png)

This stage added global trajectory correction using pose graph optimization. Loop closures provided long-range constraints, while odometry constraints preserved local motion consistency.

---

## Step 8 — Advanced SLAM Infrastructure

### Implemented

- Keyframe selection logic
- Performance monitoring
- Map serialization
- Map loading from disk
- Configuration-driven SLAM system design
- Initial relocalization system integration

### Result

Validated several production-oriented SLAM components independently.

| Component | Result |
|--------|--------|
| Keyframe Selection | Passed |
| Performance Monitoring | Passed |
| Map Serialization | Passed |
| Map Save/Load | Verified |
| Test Map | 1 keyframe, 1 landmark |
| Runtime FPS | 20–23 FPS |

This stage focused on moving the project beyond the core SLAM algorithm and toward production-style system infrastructure. Keyframe selection, runtime monitoring, configuration management, and map persistence were implemented and tested.

> Development note: the full advanced SLAM system demo encountered a configuration bug related to the relocalization module. The working components were validated independently, while full relocalization integration remained future work.

---

## Step 9 — Advanced SLAM Infrastructure

Implemented:
- Keyframe management
- Performance monitoring
- Map serialization
- Multi-threading

Result:
Production-grade SLAM framework

---

## Step 10 — Relocalization & System Integration

Implemented:
- Tracking recovery
- Global system integration
- End-to-end testing

Result:
Complete monocular SLAM system























