# Border-security-robot
Formation and control of a security robot for border surveillance, metal detection, obstacle detection and threat monitoring.
# Formation and Control of Robots for Border Security

## Project Overview

This project presents the design and development of a 4WD robotic rover for border security applications.

The robot combines mobile navigation, ultrasonic scanning, metal detection, wireless communication, and real-time hazard monitoring. An ESP32 DevKit acts as the central controller, processing sensor information, controlling the robot, and providing a local Wi-Fi dashboard that can be accessed using a phone or laptop.

The system is intended as an academic prototype to demonstrate how robotics and embedded systems can assist security personnel in monitoring potentially hazardous or restricted areas.

## Objectives

* Design and develop a mobile robotic system for border security.
* Enable remote control and navigation of the rover.
* Detect obstacles using an ultrasonic sensor.
* Perform a wide-angle ultrasonic scan using a servo mechanism.
* Detect metallic objects near the ground.
* Provide immediate visual and audio alerts when a metallic target is detected.
* Display real-time sensor and hazard information through a local Wi-Fi dashboard.
* Demonstrate the integration of robotics, embedded systems, sensing, and IoT technologies.

## Applications

The proposed system can be used for:

* Border surveillance
* Restricted-area monitoring
* Metallic-object detection
* Obstacle detection and avoidance
* Security inspection
* Hazardous-area exploration
* Remote surveillance assistance
* Military and security research applications

> Note: This is an academic prototype and is not intended to replace certified security, military, or mine-detection equipment.

## System Configuration

### Main Components

| Component                 | Function                                                   |
| ------------------------- | ---------------------------------------------------------- |
| ESP32 DevKit              | Main controller, sensor processing and Wi-Fi communication |
| 4WD Robot Chassis         | Mechanical platform                                        |
| DC Geared Motors          | Robot movement                                             |
| L298N Motor Driver        | Motor control interface                                    |
| HC-SR04 Ultrasonic Sensor | Obstacle and distance detection                            |
| SG90 Micro Servo          | Sweeps the ultrasonic sensor                               |
| Metal Detector Module     | Detects nearby metallic objects                            |
| LM2596 Buck Converter     | Regulates battery voltage                                  |
| Buzzer                    | Audible hazard alert                                       |
| Red LED                   | Visual hazard alert                                        |
| 1 kΩ and 2 kΩ Resistors   | Voltage divider for HC-SR04 Echo                           |
| Battery Pack              | Main power source                                          |

## System Architecture

```text
                    +---------------------+
                    |     ESP32 DevKit    |
                    |   Main Controller   |
                    +----------+----------+
                               |
             +-----------------+-----------------+
             |                 |                 |
             v                 v                 v
       Motor Control      Sensor System      Wi-Fi Server
             |                 |                 |
             v          +------+-------+          v
          L298N         |              |     Phone/Laptop
             |          v              v
             v      HC-SR04      Metal Detector
        4 DC Motors      |              |
                         v              v
                       Servo       Buzzer + LED
```

## Working Principle

The ESP32 acts as the central control unit of the rover.

1. The robot receives movement commands.
2. The ESP32 controls the four DC motors through the L298N motor driver.
3. The SG90 servo sweeps the HC-SR04 ultrasonic sensor through a defined angular range.
4. The HC-SR04 measures the distance to surrounding obstacles.
5. The metal detector continuously monitors the area near the ground.
6. When a metallic object is detected, the ESP32 activates the buzzer and red LED.
7. Sensor and hazard information is displayed on a local Wi-Fi web dashboard.
8. The process continues continuously while the rover is operating.

## Radar-Like Scanning

The HC-SR04 is mounted on an SG90 servo to provide a sweeping motion.

```text
             90°
              |
              |
        45°   |   135°
          \   |   /
           \  |  /
            \ | /
             \|/
             [O]
          Ultrasonic
            Sensor
```

The servo changes the sensor's angle while the ESP32 records distance measurements.

This creates a radar-like 2D scanning visualization.

> The HC-SR04 uses ultrasonic waves and is therefore not an actual RF radar system. The term "radar-like" refers to the scanning and visualization concept.

## Hazard Detection

The metal detector is positioned toward the ground at the front of the rover.

When a metallic object is detected:

```text
Metal Detected
      |
      v
    ESP32
      |
  +---+----+
  |        |
  v        v
Buzzer   Red LED
  ON       ON
      |
      v
Dashboard Alert
```

A buried metallic object can be used during demonstration to simulate a potential hazardous metallic target.

## Wi-Fi Monitoring Dashboard

The ESP32 hosts a local web server that allows a phone or laptop to monitor the rover.

The dashboard can provide:

* Robot status
* Distance measurements
* Ultrasonic scan information
* Metal detection status
* Hazard alerts
* Movement status
* Sensor information

### Communication

```text
Sensors
   |
   v
 ESP32
   |
   v
Wi-Fi Web Server
   |
   v
Phone / Laptop
```

The basic system can operate through a local network without requiring an external cloud server.

## Power Architecture

The battery acts as the main power source.

```text
             Battery Pack
             7.4 V / 12 V
                   |
          +--------+--------+
          |                 |
          v                 v
      L298N Motor       LM2596 Buck
        Supply            Converter
                            |
                            v
                           5 V
                            |
             +--------------+--------------+
             |              |              |
             v              v              v
           ESP32           SG90          Sensors
```

The LM2596 buck converter provides regulated voltage for the low-voltage electronic components.

The converter output should be adjusted and verified using a multimeter before connecting the electronics.

## Safety Considerations

* The ESP32 uses 3.3 V logic.
* 5 V signals should not be connected directly to ESP32 GPIO inputs.
* The HC-SR04 Echo signal requires appropriate voltage-level reduction.
* The LM2596 output should be checked before connecting sensitive electronics.
* All interconnected modules should have an appropriate common ground.
* Motor polarity should be checked before final assembly.
* The metal detector is intended for prototype demonstration only.

## Repository Structure

```text
border-security-robot/
|
+-- README.md
|
+-- docs/
|   +-- system-configuration.md
|   +-- working-principle.md
|   +-- applications.md
|
+-- hardware/
|   +-- circuit-diagram/
|
+-- software/
|   +-- source-code/
|
+-- images/
|
+-- presentation/
```

## Project Status

Status: Under Development

### Completed

* Project concept
* System architecture
* Component selection
* Functional design

### In Progress

* Hardware assembly
* Motor control
* Sensor integration
* Ultrasonic scanning
* Metal detection
* Wi-Fi dashboard
* Software development

### Planned

* Complete prototype integration
* Testing and calibration
* Performance evaluation
* Final demonstration
* Project documentation

## Project Information

Project Title: Formation and Control of Robots for Border Security

Domain: Robotics, Embedded Systems and IoT

Department: Electrical and Electronics Engineering

Project Type: Academic Major Project

## Disclaimer

This project is developed as an academic prototype to demonstrate the integration of robotics, embedded control, sensing, and wireless monitoring technologies for security-related applications.

It is intended for educational and research purposes only and should not be considered a certified security or mine-detection system.
