---
layout: post
title: Robotic Gripper
description:  Designed and manufactured a robotic gripper using Fusion 360, integrating a geared mechanical transmission and 3D-printed components. The system is actuated via an Arduino-controlled motor driver, allowing precise motion control for grasping and releasing objects. This project demonstrates the intersection of mechanical design, embedded electronics, and control programming in a compact robotic subsystem. The design was inspired by observing larger, high-precision, industrial-grade grippers at a robotics conference, motivating a cost-effective, scaled-down version.
skills: 
- Mechanical design (Fusion 360)
- 3D printing and prototyping
- Arduino programming
- Motor driver integration
- Motion control and actuation
- Geared transmission design
- Embedded system prototyping
main-image: /RoboticGripperMainImage.png
---

---
# ADDITIONAL DETAILS
### 
## Problem Statement
Manual or semi-automated grippers often suffer from limited precision and repeatability in object handling, particularly when dealing with delicate or irregularly shaped parts. Achieving reliable grasping with consistent force and accurate positioning requires a design that integrates **mechanical leverage, controlled actuation, and embedded sensing.** Without such a system, object handling tasks are prone to human error, inconsistent motion, and potential damage to parts, highlighting the need for a compact, reliable, and programmable robotic gripper.

## Bill of Materials (BOM)
The following table lists the components used in the prototype, including quantities, materials, estimated costs, and suppliers.

| ITEM | COMPONENT | QTY | MATERIAL | COST($) | SUPPLIER | NOTES |
|----------|----------|----------|----------|----------|----------|----------|
| 1 | ULN2003 5V Stepper Motor and ULN2003 Driver Board | 1 | PCB/Metal/Plastic | 17.00 | Arduino Kit | Common stepper driver + motor interface module |
| 2 | Arduino Uno | 1 | PCB/Plastic | 26.00 | Arduino Kit | Microcontroller for control |
| 3 | Resistor Set (includes 10k) | 1 | Various | 5.00 | Arduino Kit | Includes the two 10 kΩ resistors used |
| 4 | Push Button Set | 1 | Plastic/Metal | 5.00 | Arduino Kit | Includes both 4 pin buttons |
| 5 | 3-D Printed Grippers, Gears, Connector Rods, and Fasteners | 20 | PLA | 10.00 | In-House | FDM-printed |
| 6 | Wiring & Connectors | - | Copper/Plastic | 5.00 | Arduino Kit | Male to Female and Male to Male |

## Arduino Gripper Actuation Controller Code
```cpp
#include <AccelStepper.h>

// ULN2003 uses 4-wire sequence
#define IN1 8
#define IN2 9
#define IN3 10
#define IN4 11

AccelStepper stepper(AccelStepper::FULL4WIRE, IN1, IN3, IN2, IN4);

// Buttons (using internal pull-ups)
const int btnCW  = 2;
const int btnCCW = 3;

// State machine
enum MotorState {
  IDLE,
  ROTATE_CW,
  ROTATE_CCW
};

MotorState state = IDLE;

// Motion parameters
const float MAX_SPEED = 800.0;   // steps/sec
const float ACCEL     = 400.0;   // steps/sec^2

void setup() {
  pinMode(btnCW,  INPUT_PULLUP);
  pinMode(btnCCW, INPUT_PULLUP);

  stepper.setMaxSpeed(MAX_SPEED);
  stepper.setAcceleration(ACCEL);

  Serial.begin(9600);
}

void loop() {
  bool cwPressed  = digitalRead(btnCW)  == LOW;
  bool ccwPressed = digitalRead(btnCCW) == LOW;

  // State transitions
  if (cwPressed && !ccwPressed) {
    state = ROTATE_CW;
  }
  else if (ccwPressed && !cwPressed) {
    state = ROTATE_CCW;
  }
  else {
    state = IDLE;
  }

  // State actions
  switch (state) {
    case ROTATE_CW:
      stepper.setSpeed(MAX_SPEED);
      stepper.runSpeed();
      break;

    case ROTATE_CCW:
      stepper.setSpeed(-MAX_SPEED);
      stepper.runSpeed();
      break;

    case IDLE:
      stepper.stop();   // decelerates smoothly
      stepper.run();
      break;
  }
}
```


