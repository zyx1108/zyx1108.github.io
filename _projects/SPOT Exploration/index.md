---
layout: post
title: Super Heavy Booster Catch (Demo Only)
description:  End-to-end autonomy on Boston Dynamics Spot: panoramic LiDAR–camera mapping, semantic-aware planning, and safe-aggressive exploration validated in simulation and on hardware.
skills: 
  - ROS / ROS2
  - NVIDIA Isaac Sim / Omniverse
  - LiDAR–camera fusion (calib, sync)
  - TSDF / ESDF mapping
  - ATSP / TSP routing
  - PRM / A* (hierarchical)
  - State machines (safety / recovery)
  - Open3D / PCL / OpenCV
  - Python / C++

main-image: /title.jpg
---

## Overview
Autonomy stack for **semantic exploration and dense object mapping** with Boston Dynamics **Spot**. The system separates **mapping** and **planning** concerns, then couples them through a clean interface: mapping delivers reliable background and object-level models; planning guarantees coverage and required multi-view object observations with short, consistent paths. :contentReference[oaicite:0]{index=0}


