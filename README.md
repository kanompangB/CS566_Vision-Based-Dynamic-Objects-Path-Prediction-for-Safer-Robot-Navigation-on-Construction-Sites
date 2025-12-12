# Vision-Based Dynamic Objects Path Prediction for Safer Robot Navigation on Construction Sites

**Team Members:**
* **Liqun Xu** - *Department of Civil and Environmental Engineering, UW-Madison*
* **Pakorn Boonpetch** - *Department of Materials Science and Engineering, UW-Madison*

---

## 📖 Abstract
This project outlines the procedures for collecting and preparing field data to train a vision-based motion prediction system. Real-world data were gathered using a Unitree B2 quadruped robot deployed on an active construction site at the University of Wisconsin-Madison. The system utilizes a Robust Tracking Pipeline starting with object detection via YOLOv8 Nano and 3D localization using the ZED 2i stereo depth sensor. This approach successfully filters sensor noise, enabling stable 'Active Track' prediction for moving workers and machinery while preventing false alarms.

## 🎯 Motivation
Construction sites are complex environments that present significant safety challenges for autonomous agents.
* **The Problem:** Traditional static surveillance does not capture the dynamic, egocentric perspective inherent to autonomous agents in complex environments.
* **The Goal:** To collect data from the robot's own moving perspective, aligning with modern benchmarks for robotic perception and enabling proactive accident prevention.

## 🛠️ Hardware & Data Collection

### Platform
To ensure authenticity and safety, we utilized a mobile platform capable of navigating uneven terrain.

* **Robot:** **Unitree B2 Quadruped Robot**
    * Selected for high mobility and 4-5 hour battery life.
* **Sensors:** **ZED 2i Stereo Camera**
    * Provides high-definition RGB video and depth sensing essential for spatial mapping.
* **Location:** Kellner Family Athletic Center construction site, UW-Madison.

### Collection Strategy
The robot was manually navigated between randomly selected waypoints (e.g., material storage areas, active machinery routes). This randomized strategy exposed the system to diverse zones, ensuring the dataset covers the unpredictable nature of real-world construction sites.

![Robot Setup](PLACEHOLDER_PATH_TO_ROBOT_IMAGE.png)
*Figure: The Unitree B2 robot equipped with ZED 2i camera used for field data collection.*

## 💻 Implementation: The Tracking Pipeline

We developed a custom "tracking-by-detection" pipeline to convert unstructured video data into a format suitable for trajectory prediction.

### 1. Object Detection & Localization
* **Detection:** Replaced initial zero-shot approaches with **YOLOv8 Nano**.
* **Localization:** Used ZED 2i stereo depth to calculate 3D position.

### 2. Tracking Logic
* **Filter:** A **6-state Kalman Filter** is used to track movement.
* **Noise Suppression:** Integrated a **Jitter Filter heuristic** that suppresses velocity for objects with a displacement under **0.5 meters**. This filters sensor noise and prevents false alarms (Ghost mode) while maintaining tracking on moving agents.

### 3. Trajectory Extraction
The pipeline iterates through frames to:
1.  Detect dynamic agents (workers/vehicles).
2.  Calculate bounding box centroids $(u, v)$.
3.  Aggregate coordinates by unique ID to form continuous trajectory sequences.

![Pipeline Logic](PLACEHOLDER_PATH_TO_PIPELINE_DIAGRAM.png)
*Figure: Logic flow for generating trajectory datasets.*

## 📊 Results

The project successfully implemented an end-to-end pipeline from robotic data collection to model inference.

### Qualitative Analysis
We evaluated the model by projecting output trajectories onto a Bird's Eye View (BEV) map.
* **Path Planning:** The system successfully generates future path plans (visualized as colored lines) based on observed history.
* **Constraint Adherence:** Predicted paths generally adhere to navigable areas, avoiding static obstacles and following the natural flow of movement.

![BEV Results](PLACEHOLDER_PATH_TO_BEV_IMAGE.png)
*Figure: Bird's Eye View reconstruction showing predicted path plans.*

## 📝 Challenges & Limitations

* **Occlusion Handling:** In crowded zones, dynamic occlusion from machinery occasionally interrupted tracking continuity.
* **Environmental Variance:** Extreme lighting variations (e.g., direct sunlight vs. shadows) sometimes introduced noise into the detection module.
* **Complex Interactions:** The current baseline treats agents independently and does not fully account for social interactions (e.g., a worker pausing for a vehicle).

## 🚀 Future Work
* **Social Pooling:** Integrate social pooling mechanisms to model agent-to-agent interactions.
* **Dataset Expansion:** Include diverse weather conditions to improve model robustness.

---
*University of Wisconsin - Madison*
