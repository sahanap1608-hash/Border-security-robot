# Working Principle

The robot operates by combining mobility, obstacle sensing, metal detection, warning indication, and wireless monitoring under the control of the ESP32.

## 1. System Initialization

When the robot is powered ON, the ESP32 initializes the motor driver, ultrasonic sensor, servo motor, metal detector, buzzer, LED, and Wi-Fi monitoring system.

## 2. Robot Movement

The ESP32 sends control signals to the L298N motor driver. The driver controls the four DC motors according to the required movement command.

The robot can move forward, reverse, turn left, turn right, or remain stationary using differential steering.

## 3. Obstacle Scanning

The SG90 servo rotates the HC-SR04 ultrasonic sensor through different angles. At each position, the sensor measures the distance to nearby objects.

The ESP32 processes these measurements to determine the approximate location of obstacles around the robot.

## 4. Metal Detection

The ground-facing metal detector continuously monitors the area in front of the robot. When a metallic object is detected, the sensor sends a signal to the ESP32.

The ESP32 identifies the detection condition and activates the warning system.

## 5. Hazard Alert

When metal detection or another defined hazard condition occurs, the ESP32 activates the buzzer and red LED. This provides an immediate local warning to the operator.

## 6. Wireless Monitoring

The ESP32 can create a local Wi-Fi connection and host a monitoring interface. Sensor information and robot status can be displayed on a phone or laptop connected to the system.

## 7. Complete Operating Flow

```text
Power ON
   |
Initialize ESP32 and peripherals
   |
Read sensor inputs
   |
+---------------------------+
| Ultrasonic distance scan  |
| Metal detection           |
+---------------------------+
   |
Process sensor information
   |
Hazard detected?
   |             |
  Yes            No
   |             |
Activate       Continue
buzzer/LED     monitoring
   |
Update monitoring interface
   |
Continue operation
```

## 8. Overall Principle

The project demonstrates the integration of embedded control, robotic mobility, sensor-based detection, warning systems, and wireless monitoring in a single mobile robotic platform. The system is intended as an academic prototype for studying robotic assistance in border and restricted-area security applications.
