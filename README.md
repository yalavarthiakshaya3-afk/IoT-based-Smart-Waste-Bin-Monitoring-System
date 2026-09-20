# IoT-Based Smart Waste Bin Monitoring System

# Overview

The IoT-Based Smart Waste Bin Monitoring System is designed to monitor the waste level inside a bin without requiring frequent manual checking. The system uses an HC-SR04 ultrasonic sensor to measure the distance between the top of the bin and the waste.

An ESP32 microcontroller processes the sensor reading and estimates the waste level. The measured level can be displayed on an OLED display. Since the ESP32 provides Wi-Fi connectivity, the system can also be extended for remote monitoring through an IoT platform.

# Objectives

* To monitor the waste level inside a bin.
* To identify the current condition of the bin.
* To reduce unnecessary manual inspection.
* To display the measured waste level.
* To use ESP32 and IoT technology for remote monitoring.

## Components Used

| Component                 | Purpose                                            |
| ------------------------- | -------------------------------------------------- |
| ESP32                     | Main controller for processing sensor data         |
| HC-SR04 Ultrasonic Sensor | Measures the distance between the sensor and waste |
| OLED Display              | Displays waste level and bin status                |
| Breadboard                | Used for circuit connections                       |
| Jumper Wires              | Connect the electronic components                  |
| USB Cable                 | Powers and programs the ESP32                      |
| Waste Bin                 | Container used for waste collection                |

## Technologies Used

* ESP32
* Arduino IDE
* Embedded C/C++
* Ultrasonic Sensing
* Wi-Fi
* IoT

## Block Diagram

```text id="zzxbk1"
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
              ┌──────────┐  ┌───────────┐
              │   OLED   │  │   Wi-Fi   │
              │ Display  │  │Connection │
              └──────────┘  └─────┬─────┘
                                  │
                                  ▼
                           ┌──────────────┐
                           │ IoT Platform │
                           └──────┬───────┘
                                  │
                                  ▼
                          Remote Monitoring
```

# Working Principle

The ultrasonic sensor is positioned at the top of the waste bin and directed towards the inside of the bin. It transmits ultrasonic waves and measures the time required for the reflected waves to return.

The ESP32 uses this measurement to calculate the distance between the sensor and the waste. As the amount of waste increases, this distance decreases.

The approximate waste percentage is calculated using:

```text id="dhnkrv"
Waste Level (%) =
(Bin Height - Measured Distance) / Bin Height × 100
```

Based on the calculated percentage, the system identifies the bin condition as:

| Waste Level | Status           |
| ----------- | ---------------- |
| 0–24%       | Empty            |
| 25–59%      | Partially Filled |
| 60–84%      | Nearly Full      |
| 85–100%     | Full             |

The calculated waste level and status are displayed on the OLED screen.

# Circuit Connections

# HC-SR04 to ESP32

| HC-SR04 | ESP32    |
| ------- | -------- |
| VCC     | 5V       |
| GND     | GND      |
| TRIG    | GPIO 5   |
| ECHO    | GPIO 18* |

# OLED to ESP32

| OLED | ESP32   |
| ---- | ------- |
| VCC  | 3.3V    |
| GND  | GND     |
| SDA  | GPIO 21 |
| SCL  | GPIO 22 |

The HC-SR04 ECHO output can be approximately 5V. A voltage divider or suitable level shifting should be used before connecting it to a 3.3V ESP32 GPIO.

# Source Code

```cpp id="81i1bo"
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define TRIG_PIN 5
#define ECHO_PIN 18

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

const float BIN_HEIGHT = 30.0;

float measureDistance() {

  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);

  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  long duration = pulseIn(ECHO_PIN, HIGH, 30000);

  if (duration == 0) {
    return -1;
  }

  return duration * 0.0343 / 2.0;
}

void setup() {

  Serial.begin(115200);

  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);

  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println("OLED initialization failed");

    while (true) {
      delay(1000);
    }
  }

  display.clearDisplay();
  display.setTextColor(SSD1306_WHITE);

  display.setTextSize(1);
  display.setCursor(15, 20);
  display.println("SMART WASTE BIN");

  display.setCursor(25, 35);
  display.println("MONITORING");

  display.display();

  delay(2000);
}

void loop() {

  float distance = measureDistance();

  if (distance < 0) {

    Serial.println("Sensor reading failed");

    display.clearDisplay();
    display.setCursor(20, 25);
    display.println("Sensor Error");
    display.display();

    delay(2000);
    return;
  }

  if (distance > BIN_HEIGHT) {
    distance = BIN_HEIGHT;
  }

  float wasteLevel =
      ((BIN_HEIGHT - distance) / BIN_HEIGHT) * 100.0;

  if (wasteLevel < 0) {
    wasteLevel = 0;
  }

  if (wasteLevel > 100) {
    wasteLevel = 100;
  }

  String status;

  if (wasteLevel < 25) {

    status = "EMPTY";

  } else if (wasteLevel < 60) {

    status = "PARTIALLY FILLED";

  } else if (wasteLevel < 85) {

    status = "NEARLY FULL";

  } else {

    status = "FULL";
  }

  // Display values on Serial Monitor

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  Serial.print("Waste Level: ");
  Serial.print(wasteLevel);
  Serial.println(" %");

  Serial.print("Status: ");
  Serial.println(status);

  Serial.println("-------------------------");

  // Display values on OLED

  display.clearDisplay();

  display.setTextSize(1);

  display.setCursor(0, 0);
  display.println("SMART WASTE BIN");

  display.setCursor(0, 18);
  display.print("Level: ");
  display.print(wasteLevel, 0);
  display.println("%");

  display.setCursor(0, 32);
  display.print("Distance: ");
  display.print(distance, 1);
  display.println(" cm");

  display.setCursor(0, 48);
  display.print("Status: ");
  display.println(status);

  display.display();

  delay(2000);
}
```

# Required Libraries

The following libraries are required in Arduino IDE:

* Adafruit GFX Library
* Adafruit SSD1306
* Wire Library

# Setup

1. Install Arduino IDE.
2. Add ESP32 board support to Arduino IDE.
3. Install the required OLED libraries.
4. Connect the HC-SR04 and OLED display to the ESP32.
5. Open the source code in Arduino IDE.
6. Select the appropriate ESP32 board and COM port.
7. Upload the program.
8. Open the Serial Monitor at `115200` baud to view the readings.

# Expected Output

An example output is:
text id="ixg7qa"
Distance: 8.4 cm
Waste Level: 72 %
Status: NEARLY FULL
The OLED display shows the waste percentage, measured distance, and current bin status.

# Applications
* Smart waste management
* Colleges and universities
* Offices
* Shopping centres
* Residential communities
* Public places
* Municipal waste collection

# Advantages

* Reduces frequent manual checking of bins.
* Provides waste-level information.
* Uses low-cost and commonly available components.
* Can support remote monitoring.
* Can be expanded for multiple waste bins.

# Future Scope

The system can be extended by connecting the ESP32 to an IoT dashboard for real-time remote monitoring. A load cell can be added to measure the weight of the waste along with its level.

Additional features such as full-bin notifications, GPS location tracking, data logging, and monitoring of multiple bins can also be incorporated.

# Author

Akshaya
B.Tech – Electrical and Electronics Engineering


