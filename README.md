# EnviroGuard-AI
ESP32-based dynamic flood monitoring system utilizing HC-SR04, soil/rain sensors, and risk-adaptive IoT analytics.
# EnviroGuard AI 🌊

An IoT-based Smart Flood Monitoring System using risk-adaptive sensing. Instead of operating at a fixed frequency, EnviroGuard AI dynamically increases sensing frequency as environmental risk increases, improving responsiveness during hazardous conditions.

## ⚙️ Hardware Components
* ESP32 DevKit V1
* HC-SR04 Ultrasonic Sensor (Water Level)
* Rain Sensor Module
* Capacitive Soil Moisture Sensor
* 0.96" I2C OLED (SSD1306)
* 3x LEDs (Green, Yellow, Red) & Active Buzzer
* Resistors: 1 kΩ & 2 kΩ (Voltage Divider), 3x 220 Ω (LEDs)

## 🔌 ESP32 Pin Connections

| Component | Pin | ESP32 Pin | Note |
| :--- | :--- | :--- | :--- |
| **HC-SR04** | TRIG | GPIO 5 | |
| | ECHO | GPIO 18 | *Via 1kΩ/2kΩ voltage divider* |
| **Rain Sensor** | AO | GPIO 34 | |
| **Soil Sensor** | AO | GPIO 35 | |
| **OLED** | SDA | GPIO 21 | |
| | SCL | GPIO 22 | |
| **Alerts** | Green LED | GPIO 25 | *Normal* |
| | Yellow LED | GPIO 26 | *Warning* |
| | Red LED | GPIO 27 | *Danger* |
| | Buzzer | GPIO 23 | |

## 🧠 Risk Logic Algorithm
The system calculates real-time flood risk based on multiple environmental factors:
* **LOW (Green):** Normal conditions.
* **MEDIUM (Yellow):** Warning state. Triggered if `Water Level > MEDIUM_THRESHOLD` OR (`Heavy Rain` + `High Soil Moisture`).
* **HIGH (Red):** Critical danger. Triggered if `Water Level > HIGH_THRESHOLD`. Activates Buzzer and Red LED.

## 🚀 Setup Instructions
1. Wire components according to the pinout table above. 
2. Ensure the HC-SR04 ECHO pin uses a voltage divider (ESP32 logic is 3.3V, sensor output is 5V).
3. Open `EnviroGuard.ino` in Arduino IDE.
4. Install `Adafruit SSD1306` and `Adafruit GFX` libraries.
5. Upload to ESP32 and monitor via Serial/OLED.
2. Detection vs Prediction# EnviroGuard AI — AI Architecture

## Overview

EnviroGuard AI is an IoT-based flood-risk monitoring and early-warning system.

The current prototype monitors three core environmental parameters:

- Water Level
- Rainfall
- Soil Moisture

The prototype demonstrates real-time environmental monitoring and deterministic risk assessment. The planned full-scale system extends this architecture with time-series analysis, machine learning, historical data, and additional environmental information.

> **Note:** EnviroGuard AI estimates evolving flood risk. It does not claim to predict the exact time or certainty of a flood.

---

## 1. Prototype vs Real-World Deployment

The prototype uses three sensors to demonstrate the core sensing and risk-assessment pipeline.

A real-world deployment would use multiple distributed sensor nodes and additional environmental parameters.

| Parameter | Possible Sensor | Purpose |
|---|---|---|
| Water Level | Radar / Industrial Ultrasonic Sensor | Monitor river, drain, or reservoir levels |
| Rainfall | Tipping-Bucket Rain Gauge | Measure rainfall amount and rate |
| Flow Velocity | Doppler / Ultrasonic Flow Sensor | Detect rapidly moving water |
| Soil Moisture | Industrial Capacitive Sensor | Measure ground saturation |
| Temperature | Industrial Temperature Sensor | Environmental context |
| Humidity | Industrial Humidity Sensor | Monitor atmospheric conditions |
| Atmospheric Pressure | Barometric Sensor | Detect weather trends |
| Camera | IP / Edge Camera | Visual confirmation |
| Location | GPS / GNSS | Identify sensor-node location |

The prototype uses an **ESP32** as the controller. A production deployment could use industrial IoT or edge controllers depending on site conditions, power availability, communication requirements, and reliability needs.

---

## 2. Detection vs Prediction

One of the main goals of EnviroGuard AI is to move from simple **current-risk detection** toward **future-risk estimation**.

### Current Prototype

```text
Water Level
    +
Rainfall
    +
Soil Moisture
    |
    v
Risk Calculation
    |
    v
Risk Score
    |
    v
NORMAL / WARNING / CRITICAL
Real-Time Sensor Data
        +
Time-Series Trends
        +
Historical Data
        +
Weather Information
        +
Upstream Environmental Data
        |
        v
    ML MODEL
        |
        v
Flood-Risk Estimate
        +
Risk Level
        +
Early Warning
        +
Explanation
Water Level    : 68%
Rainfall       : 82%
Soil Moisture  : 76%
10:00 -> 45%
10:10 -> 51%
10:20 -> 58%
10:30 -> 66%
Heavy Rainfall
      |
      v
Soil Saturation Increases
      |
      v
Water Level Begins Rising
      |
      v
Water Level Rises Rapidly
      |
      v
Flood Risk Escalates
High Rainfall
      +
High Soil Saturation
      +
Rapid Water-Level Increase
      |
      v
Elevated Historical Risk Pattern
ESP32
  |
  v
Sensor Data
  |
  v
n8n
  |
  +--> Validate Data
  |
  +--> Store Data
  |
  +--> Process Data
  |
  v
Historical + Current Data
  |
  v
AI / ML Layer
  |
  v
Flood-Risk Estimate
  |
  +--> Dashboard
  |
  +--> Alerts

| Parameter     | Weight |
| ------------- | -----: |
| Water Level   |    50% |
| Rainfall      |    30% |
| Soil Moisture |    20% |



ENVIRONMENTAL ANALYSIS

Water Level      : 72%
Rainfall         : 84%
Soil Moisture    : 79%

Water Trend      : RAPIDLY RISING
Rain Trend       : INCREASING
Soil Condition   : HIGH SATURATION

Risk Score       : 86 / 100
Risk Level       : CRITICAL

KEY FACTORS
- Heavy rainfall
- High soil saturation
- Rapid water-level increase

ASSESSMENT
Elevated flood-risk conditions are developing
in the monitored area.

OUTLOOK
Risk may increase if the current environmental
trend continues.
Sense
  |
  v
Validate
  |
  v
Store
  |
  v
Analyse
  |
  v
Learn Patterns
  |
  v
Estimate Risk
  |
  v
Explain Risk
  |
  v
Warn Users
EnviroGuard-AI/
|
+-- README.md
|
+-- docs/
|   +-- AI-ARCHITECTURE.md
|   +-- judge-questions.md
|   +-- real-world-deployment.md
|   +-- architecture.md
|
+-- firmware/
|
+-- ai/
|
+-- n8n/
|
+-- dashboard/
|
+-- hardware/
