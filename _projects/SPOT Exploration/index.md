---
layout: post
title: SPOT Semantic Exploration and Dense Mapping
description:  End-to-end autonomy on Boston Dynamics Spot (panoramic LiDAR–camera mapping, semantic-aware planning, and safe-aggressive exploration validated in simulation and on hardware)
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
  

main-image: /title.png
---

## Overview
Autonomy stack for **semantic exploration and dense object mapping** with Boston Dynamics **Spot**. The system separates **mapping** and **planning** concerns, then couples them through a clean interface: mapping delivers reliable background and object-level models; planning guarantees coverage and required multi-view object observations with short, consistent paths. :contentReference[oaicite:0]{index=0}

## Role & Scope
- Led perception–planning integration, simulation benchmarking, and on-robot validation.  
- Delivered production-ready ROS/ROS2 components, launch/config, logging/metrics, and operator-facing safety behaviors.

---

## Hardware & Sensors
- **Platform:** Boston Dynamics Spot with ROG NUC (RTX 4070).  
- **Sensors:** Ouster **OS1-128** (360° × 45°, 10 Hz) + **SPOT CAM** panoramic array (5 × IMX290; ~360° × 165°). :contentReference[oaicite:1]{index=1}

---

## Mapping Module (Perception)
**Goal:** produce stable background maps and dense object reconstructions while tolerating viewpoint changes and occasional detector noise.

**Key elements**
- **Panoramic LiDAR–camera fusion**: time sync, rigid-body extrinsics, image–point projection, keyframe selection.  
- **Instance association & robustness:** DBSCAN-based filtering, IoU/IoMin checks, Bernoulli validity for keyframes, and **model merge / re-centering** to correct association drift.  
- **Outputs:** occupancy (0.1 m), coarse-to-fine object voxel models, and **dense object maps** extracted from SLAM point clouds. :contentReference[oaicite:2]{index=2}

**Result snapshot**  
- Lobby: **97.28%** object completeness (1.75 cm avg error); background **99.33%** (2.31 cm).  
- Construction mock-up: **87.19%** object completeness (3.11 cm); background **99.19%** (2.27 cm). :contentReference[oaicite:3]{index=3}

---

## Planning Module (Autonomy)
**Problem framing:** cover unknown voxels and satisfy **multi-view** observations for each target by visiting the best-scoring semantic viewpoints in each accessible observation “sector,” while keeping travel short and behavior predictable. :contentReference[oaicite:4]{index=4}

**Local viewpoint generation**
- **Priority-driven Decoupled Local Sampler (PDLS):** manages **semantic** and **geometric** viewpoints separately; selects minimal sets that achieve both semantic observation and geometric coverage; accounts for submodularity and overlap. :contentReference[oaicite:5]{index=5}

**Global routing**
- Receding-horizon **ATSP** over active regions; exact cost from current pose, ED approximation among remote viewpoints. Shortest segment executed; stack replans as regions complete. :contentReference[oaicite:6]{index=6}

**Safety & execution**
- **Safe-Aggressive Exploration State Machine (SAESM):**  
  - *Exploring:* **Aggressive Graph Construction** (PRM nodes in free & unknown space; dynamic pruning in LPH).  
  - *Executing:* backward safety checks around the planned corridor.  
  - *Recovering:* controlled backtrack to last verified corridor, then resume exploration. :contentReference[oaicite:7]{index=7}

---

## Benchmarks & Validation
**Simulation (Isaac Sim):** Office, Warehouse, Factory.  
Our planner achieved **faster exploration** and **shorter paths** than HIRE, Semantic Eight, and TARE while satisfying all semantic-viewpoint requirements; trajectories show fewer revisits and steadier coverage. :contentReference[oaicite:8]{index=8}

**Hardware (Spot):**  
- Construction mock-up (12 m × 8 m): five targets; exploration **54 s** at ~0.7 m/s; dense object maps with low error.  
- Lobby (12 m × 23 m): eleven chairs; exploration **132 s** at ~0.9 m/s; complete multi-view observation coverage. :contentReference[oaicite:9]{index=9}

---

## System Architecture (at a glance)
1. **Perception:** panoramic fusion → object association → voxel/object models → TSDF/ESDF services for planners.  
2. **Planning:** PDLS for local viewpoints → receding-horizon **ATSP** → SAESM for safe, efficient execution. :contentReference[oaicite:10]{index=10}

---

## Media
{% include image-gallery.html images="/assets/img/semantic.jpg, /assets/img/fusion.jpg" height="380" %}

{% include youtube-video.html id="XXXXXXXXXXX" autoplay="false" %}

---

## Tech Stack
- **ROS/ROS2**, TF/TF2, RViz, MoveIt (support)  
- **NVIDIA Isaac Sim**; Open3D, PCL, OpenCV  
- **Python / C++**; unit tests, launch/config, logging & metrics

---

## Notes
- This page follows your Jekyll post template structure (front matter, skills list, media includes). :contentReference[oaicite:11]{index=11}

## References
- Zhan, Zhou, **Zhao**, *et al.* “Semantic Exploration and Dense Mapping of Complex Environments using Ground Robot with Panoramic LiDAR–Camera Fusion,” **IEEE RAL**, accepted Aug 2025. :contentReference[oaicite:12]{index=12}
