# Target_Follow_Jetbot

A deep learning-based object tracking and object avoidance system for the NVIDIA JetBot, using SSD MobileNet as the object detection backbone for target detection.

This work was published in the **IEEE International Conference on Consumer Electronics-Asia (ICCE-Asia), 2022**, with oral presentation.  

> M. C. Lee and M. Lee, "Deep Learning-based Target Following and Obstacle Avoidance Methods in Mobile Robots," *2022 IEEE International Conference on Consumer Electronics-Asia (ICCE-Asia)*, pp. 1–4, doi: [10.1109/ICCE-Asia57006.2022.9954653](https://doi.org/10.1109/ICCE-Asia57006.2022.9954653)


## System Overview

<img src="https://github.com/user-attachments/assets/3820039b-e88f-40c0-ac3a-940f596c9360" width="400"/>
Jetbot Platform


<img src="https://github.com/user-attachments/assets/1bd5f635-1796-4f7e-b98c-72c8f3d8ceb8" width="400">
Overall System Logic Flow

## System Workflow (3 core components)

### 1. Target Detection


<img src="https://github.com/user-attachments/assets/eb0b0ea3-bee0-4c1d-83c7-18d7c85389d1" width="500">
SSD Architecture

We utilize transfer learning with SSD MobileNet model pretrained on the COCO Dataset for real-time object detection.


<img src="https://github.com/user-attachments/assets/5fcf1875-946a-4059-96cd-2e4178309595" width="500">
Detection Output

Only the output of oject detection algorithm is used for target following and object avoiding

Usage
  - Class : Target vs Obstacle
  - Bounding Box : distance, location, occlusion
  - Confidence : oclusion


a. Detects the target in the current frame (green bounding box)

b. If multiple targets are found, selects the one closest to the center of the frame

c. In subsequent frames, selects the bounding box with the highest IoU with the previous target, to preserve on following the same target

d. If the target is lost (e.g., detection loading), retains the last known bouding box.

e. JetBot performs a shakes its head(No sign) when the target is not detected.


### 2. Target Following

a. Distane

Distance is estimated by the bounding box size.

Direction is derived from the bounding box position.

If the bounding box becomes smaller than a threshold, the robot moves forward.

If the target is not centered in the frame, the robot adjusts its heading to re-center it.

### 3. Obstacle Avoidance
Any object with a different label than the target is considered an obstacle.

Calculates IoU between target and obstacle to estimate overlap.

Compares the center position of each to determine if the obstacle is on the left or right.

The robot avoids the obstacle by moving in the opposite direction.

If the target is lost during avoidance, the robot performs a head tilt to recover the target.



| Test Case                    | Success Rate                                                        |
| ---------------------------- | ------------------------------------------------------------------- |
| **Side-to-side tracking**    | 100%                                                                |
| **Straight & side tracking** | 100% (up to 90cm) <br> Performance drops significantly beyond 100cm |
| **Obstacle avoidance**       | 50% (left) <br> 60% (right)                                         |
