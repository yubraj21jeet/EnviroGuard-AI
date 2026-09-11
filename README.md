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
2. Detection vs Prediction

A key distinction in EnviroGuard AI is the difference between detecting current risk and estimating future risk.

Current Prototype
Water Level
     +
Rainfall
     +
Soil Moisture
     ↓
Risk Calculation
     ↓
Risk Score
     ↓
NORMAL / WARNING / CRITICAL

This provides immediate and deterministic risk assessment.

Future AI System
Real-Time Sensor Data
          +
Time-Series Trends
          +
Historical Data
          +
Weather Information
          +
Upstream / Environmental Data
          ↓
       ML MODEL
          ↓
   Flood-Risk Estimate
          +
     Risk Level
          +
    Early Warning
          +
     Explanation

The major improvement is that the system can consider how conditions are changing, not just their current values.

3. How the AI Estimates Flood Risk

The AI layer can analyse several categories of information.

3.1 Current Environmental Conditions

Example:

Water Level     : 68%
Rainfall        : 82%
Soil Moisture   : 76%

These values describe the current state of the monitored environment.

3.2 Rate of Change

A single sensor reading does not tell the complete story.

For example:

10:00 → 45%
10:10 → 51%
10:20 → 58%
10:30 → 66%

The water level is not simply high — it is rising rapidly.

The system can therefore calculate features such as:

Water-Level Rate of Change
Rainfall Rate of Change
Soil-Moisture Rate of Change
Rolling Average
Rolling Rainfall Accumulation
Water-Level Acceleration

These time-series features provide more information than a single measurement.

4. Sensor Correlation

The system can analyse relationships between environmental parameters.

For example:

Heavy Rainfall
      ↓
Soil Saturation Increases
      ↓
Water Level Begins Rising
      ↓
Water Level Rises Rapidly
      ↓
Flood Risk Escalates

Instead of treating every sensor independently, the AI/ML layer can learn patterns across multiple measurements.

This is one of the main ways predictive modelling can provide additional value over a simple fixed threshold.

5. Historical Pattern Analysis

Historical data can help identify conditions associated with previous flood-risk events.

For example:

High Rainfall
+
High Soil Saturation
+
Rapid Water-Level Increase
        ↓
Elevated Historical Risk Pattern

A trained ML model can learn relationships from properly labelled historical observations.

The model should be trained using representative real-world data rather than relying only on manually selected thresholds.

6. External Environmental Information

A full-scale EnviroGuard AI deployment could incorporate additional data sources such as:

Weather forecasts
Rainfall forecasts
Upstream water levels
River and reservoir measurements
Historical flood records
Geographic information
Elevation/topography
Land-use information
Camera observations

This would allow the system to understand the monitored environment using a broader context.

7. Role of n8n

n8n is the orchestration layer — not the flood-prediction model.

Its role is to connect the different components of the system.

             ESP32
               │
               ▼
          Sensor Data
               │
               ▼
              n8n
               │
       ┌───────┼────────┐
       ▼       ▼        ▼
   Validate   Store   Process
       │       Data      │
       └───────┼────────┘
               ▼
        Historical Data
               +
         Current Data
               │
               ▼
          ML / AI Layer
               │
               ▼
       Flood-Risk Estimate
               │
        ┌──────┴──────┐
        ▼             ▼
    Dashboard       Alerts
Component Responsibilities
Component	Responsibility
ESP32	Sensor acquisition and local safety logic
IoT Network	Data transmission
n8n	Workflow orchestration
Database	Historical/time-series storage
ML Model	Predictive risk estimation
AI/LLM	Explanation and contextual analysis
Dashboard	Monitoring and visualization
Alert System	User/operator notifications
8. AI Model Development Strategy

EnviroGuard AI should evolve gradually rather than immediately using a complex neural network.

Stage 1 — Prototype
Rule-Based
+
Weighted Risk Score

Example:

Water Level    → 50%
Rainfall       → 30%
Soil Moisture  → 20%

This provides a transparent baseline.

Stage 2 — Machine Learning

Potential models:

Logistic Regression
Random Forest
XGBoost
Stage 3 — Advanced Time-Series Models

After collecting enough high-quality data, models such as:

LSTM
GRU
Other sequence/time-series architectures

can be evaluated.

A complex model is not automatically better. Data quality, correct labelling, validation, and generalisation are more important than model complexity.

9. Example AI Output

Instead of simply displaying:

FLOOD = YES

EnviroGuard AI can provide an explainable environmental assessment:

╔══════════════════════════════════╗
║      ENVIRONMENTAL ANALYSIS      ║
╚══════════════════════════════════╝

Water Level       : 72%
Rainfall          : 84%
Soil Moisture     : 79%

Water Trend       : RAPIDLY RISING
Rain Trend        : INCREASING
Soil Condition    : HIGH SATURATION

Risk Score        : 86 / 100
Risk Level        : CRITICAL

KEY FACTORS
• Heavy rainfall
• High soil saturation
• Rapid water-level increase

ASSESSMENT
Elevated flood-risk conditions are
developing in the monitored area.

OUTLOOK
Risk may increase if the current
environmental trend continues.
10. Risk Score vs Probability

The prototype should distinguish between a risk score and a statistically validated probability.

For example:

Risk Score: 86 / 100

is appropriate for a weighted prototype.

However:

Flood Probability: 86%

should only be reported after a properly trained, validated, and appropriately calibrated predictive model produces that probability.

Therefore, the preferred terminology during the prototype stage is:

Flood Risk Score

or

Estimated Flood Risk

11. Safety Architecture

The AI system should not depend entirely on an AI model or LLM for emergency decisions.

EnviroGuard AI follows a layered approach:

┌─────────────────────────────┐
│     Local Safety Logic      │
│           ESP32             │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│    Deterministic Risk       │
│          Engine             │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│       ML Risk Model         │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│      AI Explanation         │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│       Alert / Dashboard     │
└─────────────────────────────┘

This provides a defense-in-depth architecture.

If cloud AI or network connectivity becomes unavailable, local deterministic safety logic can still provide basic prototype-level warnings.

12. Complete System Architecture
                         ENVIRONMENT
                              │
        ┌─────────────────────┼─────────────────────┐
        ↓                     ↓                     ↓
   WATER LEVEL            RAINFALL             SOIL MOISTURE
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                              ↓
                       ESP32 SENSOR NODE
                              ↓
                       Wi-Fi / IoT Network
                              ↓
                             n8n
                              ↓
                     DATA PRE-PROCESSING
                              ↓
                ┌─────────────┴─────────────┐
                ↓                           ↓
         HISTORICAL DATA              CURRENT DATA
                │                           │
                └─────────────┬─────────────┘
                              ↓
                       AI / ML ANALYSIS
                              ↓
          ┌───────────────────┼───────────────────┐
          ↓                   ↓                   ↓
      CURRENT              TREND             HISTORICAL
     CONDITIONS           ANALYSIS             PATTERNS
          └───────────────────┼───────────────────┘
                              ↓
                       FLOOD-RISK MODEL
                              ↓
                     RISK SCORE / LEVEL
                              ↓
             ┌────────────────┼────────────────┐
             ↓                ↓                ↓
          NORMAL           WARNING          CRITICAL
             ↓                ↓                ↓
         Monitoring         Alert       Emergency Alert
                              │
                              ▼
                       DASHBOARD / USER
13. What Does AI Actually Add?

A judge may ask:

"You can already calculate risk using a formula. What is AI actually adding?"

Answer

"Our prototype uses a weighted risk score for basic real-time assessment. The AI/ML layer goes beyond fixed thresholds by analysing time-series sensor data, rate of change, relationships between rainfall, soil saturation and water level, and historical patterns. This allows the system to estimate evolving flood risk rather than only detecting the present condition. The deterministic rule-based layer provides immediate safety logic, while the ML layer provides predictive intelligence and the AI explanation layer helps communicate the factors behind the risk."

14. Key Technical Position

EnviroGuard AI is not claiming to magically predict floods.

The project's approach is:

Sense
  ↓
Validate
  ↓
Store
  ↓
Analyse
  ↓
Learn Patterns
  ↓
Estimate Risk
  ↓
Explain Risk
  ↓
Warn Users

The long-term objective is to transform raw environmental measurements into early, explainable, and actionable flood-risk information.

🚀 Future Development
 Deploy multiple sensor nodes
 Add MQTT/IoT communication
 Add time-series database
 Collect real historical data
 Engineer temporal features
 Benchmark Logistic Regression, Random Forest and XGBoost
 Evaluate time-series models when sufficient data is available
 Integrate weather information
 Integrate upstream river-level data
 Add camera-based verification
 Add sensor fault detection
 Add GPS-based sensor mapping
 Add SMS/mobile alerts
 Build live monitoring dashboard
 Add model validation and drift monitoring
 Implement redundant communication and power systems for field deployment
📌 Project Principle

"The goal is not to simply detect water. The goal is to understand how environmental conditions are changing and provide earlier, more explainable flood-risk information."

Recommended GitHub placement
EnviroGuard-AI/
│
├── README.md
│
├── docs/
│   ├── AI-ARCHITECTURE.md       ← THIS FILE
│   ├── judge-questions.md
│   ├── real-world-deployment.md
│   └── architecture.md
│
├── firmware/
├── ai/
├── n8n/
├── dashboard/
└── hardware/.

Then add this to your main README.md:

## 📚 Documentation

- [AI Architecture & Judge Preparation](docs/AI-ARCHITECTURE.md)
- [Judge Questions & Answers](docs/judge-questions.md)
- [Real-World Deployment](docs/real-world-deployment.md)
- [System Architecture](docs/architecture.md)
