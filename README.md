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
