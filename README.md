# Vision-Based Dynamic Objects Path Prediction for Safer Robot Navigation on Construction Sites

**Authors:**
* [cite_start]**Liqun Xu** - *Department of Civil and Environmental Engineering, UW-Madison* [cite: 2, 3]
* [cite_start]**Pakorn Boonpetch** - *Department of Materials Science and Engineering, UW-Madison* [cite: 5, 6]

---

## 📖 Abstract
[cite_start]This project presents an end-to-end pipeline for collecting field data and training a vision-based motion prediction system for construction sites[cite: 8, 9]. [cite_start]Using a Unitree B2 quadruped robot, we captured egocentric data to train a model capable of predicting the paths of workers and machinery[cite: 10]. [cite_start]The system utilizes a Robust Tracking Pipeline with YOLOv8 Nano and a 6-state Kalman Filter to ensure stable tracking and prediction in dynamic environments[cite: 12, 13].

## 🎯 Motivation
Construction sites are complex environments where safety is paramount. [cite_start]Unlike traditional static surveillance, autonomous agents require dynamic, egocentric perception to navigate safely[cite: 17, 18].
* [cite_start]**The Gap:** Standard datasets often lack the specific constraints and unpredictable nature of active construction zones[cite: 24].
* [cite_start]**The Goal:** To align with modern robotic perception benchmarks by collecting data from the robot's own moving perspective, enabling proactive accident prevention[cite: 19, 74].

## 🛠️ Approach & Hardware

### Data Collection Platform
[cite_start]To ensure authenticity and safety, we utilized a mobile platform capable of traversing uneven terrain[cite: 11, 21].

* [cite_start]**Robot:** Unitree B2 Commercial Quadruped Robot (selected for 4-5 hour battery life and mobility)[cite: 20, 21].
* [cite_start]**Sensors:** ZED 2i Stereo Camera (High-definition RGB video + Depth sensing)[cite: 22].
* [cite_start]**Location:** Kellner Family Athletic Center construction site, UW-Madison[cite: 17].

> [cite_start]**Collection Strategy:** The robot was manually navigated between randomly selected waypoints (e.g., material storage, active machinery routes) to capture diverse site conditions[cite: 23, 24].

![Unitree B2 Robot and ZED Camera Setup](PLACEHOLDER_PATH_TO_FIGURE_2)
[cite_start]*Figure: The Unitree B2 robot equipped with ZED 2i camera used for field data collection[cite: 20, 34].*

## 💻 Implementation

### 1. Robust Tracking Pipeline
[cite_start]We developed a custom pipeline inspired by "tracking-by-detection" paradigms to convert unstructured video into trajectory data[cite: 27].

1.  [cite_start]**Object Detection:** Replaced initial zero-shot approaches with **YOLOv8 Nano** for efficient detection[cite: 12].
2.  [cite_start]**3D Localization:** Utilized ZED 2i stereo depth to map 2D detections to 3D space[cite: 12].
3.  **Tracking & Filtering:**
    * [cite_start]Implemented a **6-state Kalman Filter**[cite: 13].
    * **Jitter Filter Heuristic:** Suppresses velocity for objects with a displacement under **0.5 meters**. [cite_start]This filters sensor noise, enabling stable 'Active Track' for moving targets while preventing false alarms in 'Ghost mode'[cite: 13, 14].

### 2. Trajectory Extraction Algorithm
[cite_start]The processing logic iterates through video frames to generate dataset entries[cite: 37]:

* **Input:** Raw Video Set $\mathcal{V}$
* **Process:**
    1.  [cite_start]**Detect:** Identify agents in frame $f_t$[cite: 39].
    2.  [cite_start]**Centroid Calculation:** Compute $(u, v)$ for bounding boxes[cite: 40].
    3.  [cite_start]**Sequence Generation:** Aggregate coordinates by unique ID to form trajectories[cite: 41].
* [cite_start]**Output:** Structured dataset $\mathcal{D}$ containing Observation ($X$) and Ground Truth ($Y$) components[cite: 46, 47].

![Logic Flow Diagram](PLACEHOLDER_PATH_TO_FIGURE_3)
[cite_start]*Figure: Logic flow for generating trajectory datasets from raw video[cite: 73].*

## 📊 Results

[cite_start]The primary outcome is a verified, high-fidelity dataset and a validated real-time prediction model[cite: 15].

### Qualitative Analysis (BEV Reconstruction)
[cite_start]We validated the model by projecting output trajectories onto a Bird's Eye View (BEV) map[cite: 81].

* [cite_start]**Path Planning:** The model successfully generates future path plans (colored lines) based on observed history[cite: 82].
* [cite_start]**Constraint Adherence:** Predicted paths respect site boundaries and avoid static obstacles, mirroring the natural flow of movement seen in training data[cite: 85].

![Qualitative Results BEV](PLACEHOLDER_PATH_TO_FIGURE_4)
[cite_start]*Figure: Bird's Eye View reconstruction showing predicted path lines for workers and machinery[cite: 101, 102].*

## 📝 Discussion & Challenges

While the pipeline is functional, we encountered several challenges common to wild construction environments:

* [cite_start]**Occlusion Handling:** In crowded zones, dynamic occlusion (e.g., passing machinery) caused fragmented trajectory inputs[cite: 89].
* [cite_start]**Environmental Variance:** Extreme lighting transitions (sunlight to building shadows) occasionally introduced noise into the detection module[cite: 90].
* [cite_start]**Complex Interactions:** The current baseline treats agents independently and does not yet account for social interactions (e.g., a worker pausing for a vehicle)[cite: 91].

## 🚀 Future Work

* [cite_start]**Social Pooling:** Integrate social pooling mechanisms to model agent-to-agent interactions explicitly[cite: 95].
* [cite_start]**Robustness:** Expand the dataset to include diverse weather conditions[cite: 95].

---
*Based on the Final Report "Vision-Based Dynamic Objects Path Prediction for Safer Robot Navigation on Construction Sites" (2025).*
