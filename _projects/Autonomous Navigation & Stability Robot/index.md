---
layout: post
title: Autonomous Navigation & Stability Robot
description: Designed and built a sensor-driven robotic vehicle capable of autonomous maze navigation and balance beam traversal during a time-constrained mechatronics competition. Integrated ultrasonic sensing with a custom servo scanning mechanism for obstacle detection and implemented gyroscope-based stabilization using a GY-521 (MPU6050). This project highlights rapid prototyping, hardware debugging, and real-world system integration under tight constraints.
skills: 
- Arduino programming
- Sensor integration (Ultrasonic, IMU)
- Embedded systems
- Hardware prototyping and wiring
- Motor driver integration (L298N)
- Debugging electromechanical systems
- Rapid prototyping under constraints
- Control systems (threshold-based logic)
main-image: /AutonomousRobotMainImage.png
---

---

# ADDITIONAL DETAILS
<div style="height: 2rem;"></div>

## 🚀 System Overview
Developed a mobile robotic platform capable of:
- Autonomous maze navigation using ultrasonic sensing  
- Directional environment scanning via a servo-mounted sensor  
- Balance beam stabilization using gyroscopic feedback  

The system was built during a competitive event with strict time constraints, requiring rapid iteration and real-time debugging of both hardware and software.

<div style="height: 1.25rem;"></div>

## 🧠 My Contributions
- Designed and implemented **full hardware layout and wiring**
- Mounted and integrated all sensors and electronic components  
- Developed **servo-based ultrasonic scanning mechanism**  
- Programmed **gyroscope (GY-521 / MPU6050)** for balance control  
- Assisted in debugging motors, drivers, and power system issues  

<div style="height: 1.25rem;"></div>

## ⚙️ Navigation Strategy
The robot used a **dynamic scanning approach** for decision-making:

- Ultrasonic sensor mounted on a **servo motor** to scan:
  - Forward  
  - Right  
  - Left  

### Decision Logic:
- If obstacle detected ahead → scan right  
- If right path clear → pivot turn right  
- Else → scan left  
- If left clear → pivot turn left  
- If both blocked → stop (end-of-maze condition)  

### Turning Method:
- Activated **3 motors while keeping 1 stationary**  
- Created a pivot-based turning mechanism for tighter navigation  

<div style="height: 1.25rem;"></div>

## ⚖️ Balance System
- Used **GY-521 (MPU6050)** gyroscope for tilt detection  
- Implemented threshold-based corrections  
- Adjusted motor outputs to maintain stability on beam  

<div style="height: 1.25rem;"></div>

## 🛠️ Bill of Materials (BOM)

| ITEM | COMPONENT | QTY | NOTES |
|----------|----------|----------|----------|
| 1 | Arduino Uno (Elegoo) | 1 | Main microcontroller |
| 2 | HC-SR04 Ultrasonic Sensor | 1 | Distance sensing |
| 3 | GY-521 (MPU6050) | 1 | Gyroscope + accelerometer |
| 4 | L298N Motor Driver | 1–2 | Motor control |
| 5 | DC Gear Motors (Yellow TT) | 4 | Drive system |
| 6 | Servo Motor | 1 | Sensor scanning mechanism |
| 7 | Battery Pack | 1 | Power supply |
| 8 | Chassis + mounts | - | Pre-fabricated + custom mounted |

<div style="height: 1.25rem;"></div>

## ⚠️ Challenges & Debugging
This project involved significant real-world hardware challenges:

- **Power system issue**
  - Batteries were incorrectly configured  
  - Resulted in insufficient current to motors  
  - Identified late in development  

- **Component failures**
  - 3 faulty DC motors  
  - 3 faulty L298N motor drivers  
  - Required systematic debugging and replacement  

- **Time constraints**
  - Debugging hardware significantly reduced time for control tuning  

These challenges emphasized the importance of **hardware validation and power system design** in robotics.

<div style="height: 1.25rem;"></div>

## 🧪 Results
- Navigation logic fully implemented and partially validated  
- Balance control system developed but not fully tuned  
- Robot performed reliably under **manual control**  
- Autonomous tasks were not completed due to late-stage hardware limitations  

<div style="height: 1.25rem;"></div>

## 💻 Example Control Logic


    moveForward();
}
