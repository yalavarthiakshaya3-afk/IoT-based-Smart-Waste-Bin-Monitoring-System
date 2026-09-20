# IoT Based Smart Waste Bin Monitoring System

# Overview

The IoT-Based Smart Waste Bin Monitoring System is designed to monitor the waste level inside a bin. An ultrasonic sensor is used to measure the distance between the top of the bin and the waste. An ESP32 microcontroller processes the sensor data and estimates the waste level.

The system displays the waste level on an OLED display and uses Wi-Fi connectivity to send the information to an IoT platform for remote monitoring.

# Objectives

* To monitor the waste level inside a bin.
* To display the current waste status.
* To reduce the need for frequent manual inspection.
* To use ESP32 and IoT technology for remote monitoring.
* To support efficient waste collection.

# Components Used

* ESP32 Development Board
* HC-SR04 Ultrasonic Sensor
* OLED Display
* Breadboard
* Jumper Wires
* USB Power Supply
* Waste Bin

# Technologies Used

* ESP32
* Arduino IDE
* Ultrasonic Sensor
* Wi-Fi
* IoT

# GitHub Setup

1. Download or clone this repository.
2. Open the project code in Arduino IDE.
3. Install the required ESP32 board package and libraries.
4. Connect the ESP32 to the computer using a USB cable.
5. Select the correct ESP32 board and COM port.
6. Upload the program to the ESP32.
7. Connect the sensor and OLED display according to the circuit diagram.

# Working

The ultrasonic sensor is placed at the top of the waste bin. It sends ultrasonic waves towards the waste and receives the reflected waves.

The ESP32 calculates the distance from the sensor to the waste and uses this value to estimate the waste level. The result is displayed on the OLED display.

The ESP32 can also connect to a Wi-Fi network and send the waste-level information to an IoT platform. This allows the bin status to be monitored remotely.

The system can identify the bin condition as:

* Empty
* Partially Filled
* Nearly Full
* Full

# Block Diagram

```text
              ┌─────────────────────┐
              │  Ultrasonic Sensor  │
              │       HC-SR04       │
              └──────────┬──────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │        ESP32        │
              │   Microcontroller   │
              └──────┬────────┬─────┘
                     │        │
                     ▼        ▼
             ┌──────────┐  ┌──────────┐
             │   OLED   │  │  Wi-Fi   │
             │ Display  │  │Connection│
             └──────────┘  └─────┬────┘
                                 │
                                 ▼
                         ┌───────────────┐
                         │ IoT Platform  │
                         └───────┬───────┘
                                 │
                                 ▼
                         Remote Monitoring
```

# Applications

* Smart waste collection systems
* Colleges and educational institutions
* Offices and commercial buildings
* Public places
* Residential communities
* Municipal waste management

# Advantages

* Reduces manual checking of waste bins.
* Provides information about the waste level.
* Supports remote monitoring through IoT.
* Uses simple and easily available components.
* Can be expanded with additional sensors and features.

# Future Scope

The system can be improved by adding a load cell to measure the weight of waste. GPS can be added to track the location of multiple bins. A web dashboard or mobile application can also be developed for monitoring several bins from a single location. Alerts can be added to notify the concerned person when the bin reaches its maximum capacity.

# Author
Akshaya
B.Tech – Electrical and Electronics Engineering

