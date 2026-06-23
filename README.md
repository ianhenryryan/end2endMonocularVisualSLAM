# End-to-End Monocular Visual SLAM System

Production-grade monocular Visual SLAM pipeline built from scratch using Python and OpenCV.

Features:
- Visual Odometry
- Sparse 3D Mapping
- Loop Closure Detection
- Pose Graph Optimization
- Relocalization
- Keyframe Management

Dataset: KITTI Odometry Sequence 05



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
