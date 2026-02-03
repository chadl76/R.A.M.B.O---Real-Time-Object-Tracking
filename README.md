# R.A.M.B.O: Robotic Arm for Monitoring and Ballistic Operations

**Status:** Completed | **Tech Stack:** Raspberry Pi, Python, OpenCV, LiDAR

![Hero GIF](./assets/Rambo-gif.gif)
*(A real-time demonstration of the autonomous tracking and firing system)*

## 📋 Abstract
This project implements an autonomous defense system capable of detecting, tracking, and neutralizing targets in real-time. Integrating a Raspberry Pi with a 2-DOF servo turret and Computer Vision, the system maps 2D camera coordinates to physical 3D space. A TF-Luna LiDAR sensor provides continuous Time-of-Flight (ToF) ranging, acting as a safety interlock that only triggers the ballistic mechanism (DC Motor via MOSFET) when the target breaches a 20cm proximity threshold.

[📄 **Read the full IEEE Technical Report**](./docs/EE452 IEEE Report Chad Lucas.pdf)

![Title](./assests/title.png)
## ⚙️ Key Features
*   **Computer Vision:** Real-time color masking and centroid tracking using OpenCV.
*   **Sensor Fusion:** Combines visual data (X/Y) with LiDAR depth data (Z) for 3D threat assessment.
*   **Hardware Interface:** Custom Logic Level Shifting (TXS0108E) to bridge 3.3V Pi logic with 5V servos.
*   **Actuation:** High-current DC motor control using an IRLZR44N N-Channel MOSFET.

## 🛠️ Hardware Architecture
The system uses a mixed-voltage architecture handled by logic level shifters.

*   **Controller:** Raspberry Pi 4 (GPIO & USB Interface)
*   **Vision:** Raspberry Pi AI Camera Module
*   **Servos:** 2x LX-16A Serial Bus Servos (UART Protocol)
*   **Sensor:** TF-Luna LiDAR (I2C Protocol)

### Circuit Schematic
![Schematic](./assets/schematic_image_name.png)
*LTSPICE schematic showing the power distribution and MOSFET switching logic.*

## 💻 Software Logic
The system runs a closed-loop control cycle:
1.  **Acquire:** Camera frame is processed for red color mask.
2.  **Calculate:** Centroid (X,Y) is mapped to servo angle (0-1000).
3.  **Track:** PID loop adjusts Pan/Tilt servos via UART.
4.  **Measure:** LiDAR checks distance via I2C.
5.  **Engage:** If distance < 20cm, GPIO High -> MOSFET Gate -> Fire.


## 📊 Performance
The system achieves real-time tracking at 30FPS with a reaction time suitable for low-speed ballistic interception.

![Tracking Demo](./assets/tracking_image.png)
