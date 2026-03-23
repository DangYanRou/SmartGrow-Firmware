# 🌱 Smart Grow: IoT Edge Firmware

> **Project Context:** This repository is a showcase/fork of the firmware developed for the "Smart Grow" project, a simulated enterprise-level IoT embedded system built by a 60+ student cohort. 

## 👨‍💻 My Role: Group 6 (Integration & Edge Control)
As part of the Group 6 Integration Team, we acted as the central technical nexus for the firmware development lifecycle. Our core responsibilities included:
* **Codebase Unification:** Reviewed, refined, and integrated isolated C++ modules from 5 separate hardware/software departments into a single, cohesive, non-blocking operational loop.
* **Edge Control Logic:** Engineered the embedded decision-making logic, ensuring the ESP32 could independently evaluate sensor data against dynamic thresholds and trigger localized actuators without relying solely on cloud commands.
* **Physical Architecture:** Directed the physical installation, wiring, and closed-loop circuit construction for the ESP32, sensors, and high-voltage actuators.

---

## 🏗️ Departmental Collaboration Structure
This firmware is the integrated culmination of multiple specialized teams working in tandem via Agile Scrum:
* **Group 2:** Soil Sensing (Moisture ADC calibration)
* **Group 3:** Atmospheric Sensing (DHT11, MQ2, LDR data processing)
* **Group 4:** Watering Systems (Pump actuation)
* **Group 5:** Climate Systems (Fans and Grow Lights)
* **Group 7:** MQTT Communications
* **Group 10:** Backend Infrastructure (REST API & Database - *maintained in a separate repository*)
* **Group 6 (Us):** Firmware Integration, Code Quality, Edge Logic, and Circuit Assembly.

---

## ⚙️ System Architecture & Modularity
To handle the complexity of multiple teams contributing simultaneously, we enforced a strict Object-Oriented structure using separated `.h` and `.cpp` headers.

### 1. Multi-Plant Sensor Module (`SensorModule.cpp`)
* **Scalability:** Dynamically designed to monitor up to 4 individual plants concurrently within a single zone (`MAX_PLANTS = 4`).
* **Telemetry:** Processes environmental data including Temperature/Humidity (DHT11), Air Quality (MQ2), and Light levels (LDR).
* **Failsafes:** Converts raw ADC soil readings into percentages and verifies that no plant exceeds maximum moisture thresholds before triggering water pumps.

### 2. Actuator & Edge Control (`ActuatorModule.cpp` & `main.ino`)
* Manages the physical execution of environmental controls via designated GPIO pins: Water Pump, Dual Cooling Fans, and LED Grow Lights.
* **Edge Processing:** The `evaluateSensorsAndTrigger()` routine actively monitors light, air quality, and temperature arrays, autonomously activating fans or lights if conditions breach API-defined thresholds.

### 3. Dual-Protocol Connectivity (`RESTClient.cpp` & `MqttModule.cpp`)
* **REST API:** Utilizes `HTTPClient` and `ArduinoJson` to fetch dynamic environmental thresholds and POST heavy telemetry logs to the backend Render server.
* **MQTT:** Maintains a persistent connection to Adafruit IO for lightweight, real-time manual overrides and immediate actuator feedback broadcasting.

---

## 🚀 Getting Started
**Dependencies:**
* `WiFi.h` & `HTTPClient.h` (ESP32 Core)
* `Adafruit_MQTT_Client.h`
* `ArduinoJson.h`
* `DHT.h`

**Setup:**
1. Clone the repository.
2. Ensure you have a `secrets.h` file in your root directory containing your WiFi and API credentials.
3. Flash to an ESP32 using the Arduino IDE or PlatformIO.