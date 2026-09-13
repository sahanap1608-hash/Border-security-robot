# System Configuration

## 1. Controller

The robot uses an **ESP32 DevKit** as the main control unit.

The ESP32 is responsible for:

* Receiving sensor data
* Controlling the motor driver
* Controlling the servo-mounted ultrasonic sensor
* Monitoring the metal detection sensor
* Activating the buzzer and warning LED
* Providing wireless monitoring through a local Wi-Fi interface

## 2. Motor Drive Configuration

The robot uses a **4WD chassis with four DC motors**.

The four motors are controlled through an **L298N motor driver**. The motors are arranged as two drive sides:

| Drive side | Motors                   |
| ---------- | ------------------------ |
| Left side  | Front-left + Rear-left   |
| Right side | Front-right + Rear-right |

The ESP32 sends control signals to the L298N.

### Movement Logic

| Left side | Right side | Robot movement |
| --------- | ---------- | -------------- |
| Forward   | Forward    | Forward        |
| Reverse   | Reverse    | Reverse        |
| Forward   | Reverse    | Turn right     |
| Reverse   | Forward    | Turn left      |
| Stop      | Stop       | Stationary     |

This arrangement provides differential steering.

## 3. Ultrasonic Scanning Configuration

An **HC-SR04 ultrasonic sensor** is mounted on an **SG90 servo motor**.

The servo rotates the sensor through an angular range so that the robot can measure obstacles in different directions.

### Connections

| Component    | Connection                         |
| ------------ | ---------------------------------- |
| SG90 VCC     | 5 V supply                         |
| SG90 GND     | Common GND                         |
| SG90 Signal  | ESP32 servo-control GPIO           |
| HC-SR04 VCC  | 5 V supply                         |
| HC-SR04 GND  | Common GND                         |
| HC-SR04 TRIG | ESP32 GPIO                         |
| HC-SR04 ECHO | ESP32 GPIO through voltage divider |

The HC-SR04 provides distance measurements while the servo changes the sensing direction.

The resulting measurements can be used to create a **radar-like obstacle display**.

> Note: The HC-SR04 is an ultrasonic sensor, not an RF radar sensor. The radar-like function is a visualization of ultrasonic distance measurements.

## 4. Metal Detection Configuration

A **metal detector sensor module** is positioned toward the ground at the front of the robot.

The sensor provides a detection signal to the ESP32.

| Metal detector | ESP32               |
| -------------- | ------------------- |
| VCC            | 5 V supply          |
| GND            | Common GND          |
| OUT            | ESP32 digital input |

When a metallic object is detected, the ESP32 processes the sensor signal and activates the warning system.

## 5. Warning System

The robot uses two local indicators:

* Buzzer
* Red LED

When a hazard condition is detected, the ESP32 activates the warning indicators.

| Device  | Control      |
| ------- | ------------ |
| Buzzer  | ESP32 output |
| Red LED | ESP32 output |

The warning system provides an immediate indication to the operator without requiring continuous observation of the monitoring dashboard.

## 6. Power Configuration

The battery is the primary power source for the robot.

The **L298N** receives the required motor supply from the battery.

An **LM2596 buck converter** is used to reduce the battery voltage to a regulated **5 V supply** for the low-voltage electronics.

### Power Flow

```text
Battery
   |
   +------------------> L298N Motor Supply
   |
   +------------------> LM2596 Buck Converter
                              |
                              +--> 5 V
                                   |
                                   +--> ESP32
                                   +--> SG90 Servo
                                   +--> HC-SR04
                                   +--> Metal Detector
```

All interconnected electronic modules must share a **common ground**.

## 7. HC-SR04 Echo Protection

The HC-SR04 Echo output can be higher than the ESP32 GPIO input voltage level.

A resistor voltage divider using **1 kΩ and 2 kΩ resistors** is used between the HC-SR04 Echo output and the ESP32 input.

```text
HC-SR04 ECHO
     |
    1 kΩ
     |
     +---------> ESP32 ECHO GPIO
     |
    2 kΩ
     |
    GND
```

This reduces the voltage reaching the ESP32 input.

## 8. ESP32 Interface Configuration

The functional interfaces are arranged as follows:

| Function                    | Interface               |
| --------------------------- | ----------------------- |
| Left motor forward/reverse  | L298N motor inputs      |
| Right motor forward/reverse | L298N motor inputs      |
| Motor speed control         | L298N enable/PWM inputs |
| Servo control               | SG90 signal             |
| Ultrasonic transmission     | HC-SR04 TRIG            |
| Ultrasonic measurement      | HC-SR04 ECHO            |
| Metal detection             | Sensor OUT              |
| Hazard warning              | Buzzer                  |
| Visual warning              | Red LED                 |
| Wireless monitoring         | ESP32 Wi-Fi             |

The final ESP32 GPIO numbers should be added after the physical wiring is verified.

## 9. Overall Hardware Interface

```text
                    ESP32 DevKit
                         |
        +----------------+----------------+
        |                |                |
     L298N           HC-SR04          Metal Sensor
        |                |                |
    4 DC Motors       SG90 Servo       Detection
                         |
                    Distance Scan
        |
        +-------------------+
        |
   Buzzer + Red LED

Battery
   |
   +----> L298N
   |
   +----> LM2596 ----> 5 V Electronics
```

## 10. Technical Design Summary

The ESP32 acts as the central controller. Motor movement is handled through the L298N driver, while the servo and HC-SR04 provide directional obstacle sensing. The metal detector provides an additional ground-level detection capability. When a potential hazard is detected, the ESP32 activates the local warning system and can update the wireless monitoring interface.

The final GPIO assignment and physical wiring diagram will be documented after verification of the assembled hardware.
