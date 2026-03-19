---
layout: post
title: Autonomous Navigation & Stability Robot
description: Designed and built a sensor-driven robotic vehicle capable of autonomous maze navigation and balance beam traversal as a team during a time-constrained mechatronics competition. Integrated ultrasonic sensing with a servo scanning mechanism for obstacle detection and implemented gyroscope-based stabilization using a GY-521 (MPU6050). This project highlights rapid prototyping, hardware debugging, and real-world system integration under tight constraints.
skills: 
- Arduino programming
- Sensor integration
- Embedded systems
- Hardware prototyping and wiring
- Motor driver integration
- Debugging electromechanical systems
- Rapid prototyping
- Control systems
main-image: /AutonomousRobotMainImage.png
---

---

# ADDITIONAL DETAILS
<div style="height: 2rem;"></div>

## Problem Statement

Autonomous navigation is a fundamental challenge in robotics, requiring a system to perceive its environment, make real-time decisions, and execute precise control without human intervention. This becomes increasingly complex in constrained environments such as mazes or narrow pathways, where limited sensing, tight clearances, and dynamic conditions demand robust and efficient solutions.

In addition to navigation, maintaining stability on uneven or narrow surfaces introduces further complexity. Accurate orientation sensing and responsive control are required to prevent tipping or loss of balance, especially when operating with limited hardware and simple control strategies.

<div style="height: 1.25rem;"></div>

## Objectives

The objective of this project, within the Mechatronics Showdown competition, was to design and develop a mobile robotic system capable of **Autonomous maze navigation** using onboard sensors and real-time decision-making, **Directional environment sensing** to detect and respond to obstacles effectively, **Balance beam traversal** using gyroscopic feedback for stability control, and **Reliable system integration** under strict time constraints and limited resources. The system needed to be robust enough to operate in a competitive setting while being rapidly prototyped and iterated within a short development window of two days.

<div style="height: 1.25rem;"></div>

## My Contributions
- Designed and implemented **full hardware layout and wiring**
- Mounted and integrated all sensors and electronic components  
- Developed **servo-based ultrasonic scanning mechanism**  
- Programmed **gyroscope (GY-521 / MPU6050)** for balance control  
- Assisted in debugging motors, drivers, and power system issues  

<div style="height: 1.25rem;"></div>

## Navigation Strategy
### Scanning Method:
- Ultrasonic sensor mounted on a servo motor to scan forward, right, and left

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

## Balance System
- Used **GY-521 (MPU6050)** gyroscope for tilt detection  
- Calculated beam angle with the IMU feedback
- Adjusted motor outputs to maintain a central position on the beam 

<div style="height: 1.25rem;"></div>

## Bill of Materials (BOM)

| ITEM | COMPONENT | QTY | NOTES |
|----------|----------|----------|----------|
| 1 | Arduino Mega 2560 R3 (Elegoo) | 1 | Main microcontroller |
| 2 | HC-SR04 Ultrasonic Sensor | 1 | Distance sensing |
| 3 | GY-521 (MPU6050) | 1 | Gyroscope + accelerometer for beam angle |
| 4 | L298N Motor Driver | 2 | Motor control |
| 5 | DC Gear Motors (Yellow TT) | 4 | Drive system |
| 6 | Plastic Wheels (TT Motor Wheels) | 4 | Mounted to DC motors |
| 7 | Servo Motor | 1 | Sensor scanning mechanism |
| 8 | Battery Pack (6V) | 2 | Power supply for motor drivers |
| 9 | 9V Battery + DC Barrel Jack | 1 | Power supply for Arduino Mega |
| 10 | Mini Breadboards | 2 | Prototyping and circuit distribution |
| 11 | Jumper Wires (M-M, M-F) | - | Electrical connections |
| 12 | Screws, Nuts, Fasteners | - | Mechanical assembly |
| 13 | Remote Controller + Cable | 1 | Manual control mode |
| 14 | Controller Cable Mount | 1 | Mounted interface for controller |
| 15 | Chassis + Mounts | - | Pre-fabricated + custom mounted |

<div style="height: 1.25rem;"></div>

<!--

update this in the future to include the schematic image
## Wiring Schematic

{% include image-gallery.html images="/AutonomousRobotMainImage.png" height="400" %}

*Note: This schematic was provided as part of the competition materials (Author: Jaspreet Chhabra). It illustrates the intended wiring layout for the robot's motors, sensors, and power system.*
-->


## Challenges & Debugging
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

These challenges emphasized the importance of **hardware validation, power system design, and effective debugging**. Understanding how to systematically identify and resolve hardware issues is critical; had we mastered this earlier, development would have proceeded more smoothly and efficiently.

<div style="height: 1.25rem;"></div>

## Results
- Navigation logic fully implemented and partially validated  
- Balance control system developed but not fully tuned  
- Robot performed reliably under **manual control**  
- Autonomous tasks were not completed due to late-stage hardware limitations

Although the autonomous tasks were not fully completed, I am grateful for the opportunity to participate in this competition. It provided a fast-paced, hands-on environment where I was able to learn and improve critical skills, including:

- Rapid hardware integration and wiring  
- Debugging motors, drivers, and power systems under time pressure  
- Implementing real-time sensor-based control (ultrasonic + gyroscope)  
- Designing and testing mechanical sensor mounts  
- Making system-level decisions in a multidisciplinary team setting

<div style="height: 1.25rem;"></div>


## Intertial Measurement Unit Code
This Arduino code uses the GY-521 (MPU6050) gyroscope and accelerometer to measure the robot’s tilt and adjust its four DC motors for balance. The system calculates the pitch angle and drives the motors forward or backward if the tilt exceeds a threshold, or stops them when the robot is level. This provides a simple feedback-based stabilization mechanism to keep the robot upright on narrow or uneven surfaces. For best results, the gyroscope should be mounted near the center of the vehicle, and for future improvement, a PID controller could be implemented to achieve smoother and more precise balance control.

```cpp
#include <Wire.h>
#include <MPU6050.h>

MPU6050 mpu; // creates an object called mpu which represents the physical MPU6050 sensor

// Motor pins
#define back_left_enable 7
#define back_left_1 6
#define back_left_2 5
#define back_right_enable 2
#define back_right_1 4
#define back_right_2 3
#define front_right_enable 8
#define front_right_1 9
#define front_right_2 10
#define front_left_enable 13
#define front_left_1 11
#define front_left_2 12

// Adjustable stop angle (deadzone)
float cutAngle = 10;     // ±cutAngle around deadzoneCenter stops motors
float deadzoneCenter = -3; // measured pitch when level

void setup() {
  Serial.begin(115200);   // lets the Arduino send text to the Serial Monitor for debugging
  Wire.begin();   // Starts the I2C bus used by the MPU6050

  mpu.initialize();   //tells the MPU6050 to start operating
  //Checks if sensor is connected
  if (mpu.testConnection()) { 
    Serial.println("MPU6050 connected!");
  } else {
    Serial.println("MPU6050 connection failed!");
  }

  // Set motor pins as outputs
  pinMode(back_left_enable, OUTPUT);
  pinMode(back_left_1, OUTPUT);
  pinMode(back_left_2, OUTPUT);
  pinMode(back_right_enable, OUTPUT);
  pinMode(back_right_1, OUTPUT);
  pinMode(back_right_2, OUTPUT);
  pinMode(front_left_enable, OUTPUT);
  pinMode(front_left_1, OUTPUT);
  pinMode(front_left_2, OUTPUT);
  pinMode(front_right_enable, OUTPUT);
  pinMode(front_right_1, OUTPUT);
  pinMode(front_right_2, OUTPUT);
}

void loop() {
  int16_t ax, ay, az, gx, gy, gz;   // ax = acceleration X, ay = acceleration Y, az = acceleration Z, gx = gyro X, gy = gyro Y, gz = gyro Z
  mpu.getMotion6(&ax, &ay, &az, &gx, &gy, &gz);   // This function reads all six measurements from the MPU6050 where the & symbol means store the value in that variable's memory location

  // Calculate pitch
  float pitch = atan2(ax, az) * 180 / PI;   // This converts acceleration data into tilt angle
  Serial.print("Pitch: ");
  Serial.println(pitch);

  // Use adjustable cut angle for stopping motors
  if (pitch < deadzoneCenter - cutAngle) {          // means the platform is tilted forward strongly
    driveForward();
  } 
  else if (pitch > deadzoneCenter + cutAngle) {     // Means platform tilted backwards strongly 
    driveBackward();
  } 
  else {                                            // Means the platform is flat
    stopMotors();
  }

  // Optional: adjust cutAngle via Serial (if you want real-time tuning)
  if (Serial.available() > 0) {
    float val = Serial.parseFloat();
    if (val >= 0) {
      cutAngle = val;
      Serial.print("New cut angle set: ");
      Serial.println(cutAngle);
    }
  }

  delay(50); // small delay to stabilize readings
}

// Motor control functions at full speed
void driveForward() {   // This sets motor directions so the car moves forward
  digitalWrite(back_left_1, LOW);
  digitalWrite(back_left_2, HIGH);
  digitalWrite(back_right_1, LOW);
  digitalWrite(back_right_2, HIGH);
  digitalWrite(front_left_1, HIGH);
  digitalWrite(front_left_2, LOW);
  digitalWrite(front_right_1, HIGH);
  digitalWrite(front_right_2, LOW);

  analogWrite(back_left_enable, 255);
  analogWrite(back_right_enable, 255);
  analogWrite(front_left_enable, 255);
  analogWrite(front_right_enable, 255);
}

void driveBackward() {   // This sets motor directions so the car moves backward
  digitalWrite(back_left_1, HIGH);
  digitalWrite(back_left_2, LOW);
  digitalWrite(back_right_1, HIGH);
  digitalWrite(back_right_2, LOW);
  digitalWrite(front_left_1, LOW);
  digitalWrite(front_left_2, HIGH);
  digitalWrite(front_right_1, LOW);
  digitalWrite(front_right_2, HIGH);

  analogWrite(back_left_enable, 255);
  analogWrite(back_right_enable, 255);
  analogWrite(front_left_enable, 255);
  analogWrite(front_right_enable, 255);
}

void stopMotors() {
  // Disable all motors completely
  digitalWrite(back_left_1, LOW);
  digitalWrite(back_left_2, LOW);
  digitalWrite(back_right_1, LOW);
  digitalWrite(back_right_2, LOW);
  digitalWrite(front_left_1, LOW);
  digitalWrite(front_left_2, LOW);
  digitalWrite(front_right_1, LOW);
  digitalWrite(front_right_2, LOW);
}
```

