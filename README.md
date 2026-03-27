# 🚨 ESP32 Security System — Home Assistant Distributed Alarm System (Sensor, Siren & Kill Switch)

<p align="center">
  <img src="esp32alarm.png" width="800" alt="PNWC Car Alarm Project Collage">
</p>

A professional-grade, distributed vehicle security system. This project bridges a battery-powered mobile sensor unit with a mains-powered house siren using Home Assistant and ESPHome.

---

## 🏗️ System Architecture

1.  **The Sensor (Car Unit):** A LILYGO T-Display S3 with **LD2410C Human Presence Radar** and an **SW-420 Vibration Sensor**. Optimized for deep sleep, waking instantly on physical disturbance.
2.  **The Brain (Home Assistant):** Manages global logic, time-of-day scheduling, and **Frigate NVR** camera integration.
3.  **The Alarm (House Unit):** A second T-Display S3 controlling a **12V DC Siren** via a 5V Relay. Features a hardwired "Wall Kill" button for emergency local control.

---

## 🛒 Parts List & Shopping Links

To build your own **ESP32 -> Home Assistant, Security System**, you will need the following components. These links represent the specific hardware used in the circuit designs.

### 🚗 Unit 1: Car Sensor (Presence & Vibration Sensor)
* **ESP32 Microcontroller:** [LILYGO T-Display S3](https://a.co/d/0jiQC1WG) (Dual-core ESP32-S3 with 1.9" LCD)
* **Presence Sensor:** [LD2410C mmWave Radar](https://a.co/d/06LWMXAf) (Human presence detection)
* **Vibration Sensor:** [SW-420 Motion Sensor](https://a.co/d/02E32HKW) (Shock/Vibration trigger)
* **LiPo Battery:** [3.7V 2000mAh LiPo Battery](https://a.co/d/02E32HKW) (JST 1.25mm connector)

### 🏠 Unit 2: House Siren (Siren Wall Powered/Relay Controlled)
* **ESP32 Microcontroller:** [ESP32-WROOM-32 DevKit](https://a.co/d/05z59KT6) (Standard 30-pin version)
* **5V Relay:** [5V One-Channel Relay Module](https://a.co/d/0aWROudj) (Opto-isolated High/Low trigger)
* **12V Siren:** [12V DC Wired Indoor/Outdoor Siren](https://a.co/d/06rncyCy) (High-decibel output)
* **Siren Power:** [12V 2A DC Power Supply Adapter](https://a.co/d/0imZnCsy) (Mains to 12V for the siren)
* * **Siren Kill Switch:** [Push Button Switch](https://a.co/d/031p9keo) (For the "Wall Kill" extension)

### 🛠️ Tooling Requirements
* **Soldering:** Basic iron and solder for connecting sensors to pins.
* **Wiring:** [22AWG](https://a.co/d/0aoN3zOD) wire or standard doorbell wire if/where needed.
* **Software:** [Home Assistant](https://www.home-assistant.io/) instance with the **ESPHome** add-on installed.
* **Zigbee Gateway:** [SONOFF Zigbee 3.0 USB Dongle](https://a.co/d/08299SBs) universal wireless Zigbee USB adapter with antenna for Home Assistant.
* **Zigbee, Thread or Matter Gateway:** [Home Assistant Connect ZB-2](https://a.co/d/0iVhmzxv) provides the best possible connection to Zigbee or Thread devices for Home Assistant.

---

## 📂 Repository Structure

* `car_security_tdisplay_s3.yaml`: Sensor unit config (Vibration + Radar + Battery Mgmt).
* `house_siren_tdisplay_s3.yaml`: Siren controller with status display and hardwired button logic.
* `ha_automations.yaml`: Global logic, notifications, and camera triggers.
* `zigbee_kill_button.yaml`: Optional automation for remote Zigbee-based silencing.

---

## 🛠️ 3D Enclosure & Mounting Designs

The system requires specific mounting to ensure sensor accuracy and weather resistance. 

<p align="center">
  <img src="alarm_3d_case_designs.svg" width="800" alt="3D Enclosure Blueprints">
</p>

### Enclosure Specifications

| Feature | Car Sensor Unit | House Siren Unit |
| :--- | :--- | :--- |
| **Material** | PETG (Heat tolerant for car interiors) | ASA (UV & Weather stable for outdoors) |
| **Infill** | 25% Gyroid | 40% Gyroid |
| **Mounting** | Under-seat rail / Hook tabs | M6 Concrete Anchors |
| **Hardware** | M3 Heat-set inserts x 4 | M3 Heat-set / M6 Bolts |

**Design Note:** The outdoor siren bracket includes a built-in **15° down-tilt** to maximize audio projection toward the driveway and prevent water ingress.

---

## 🤖 Advanced Features

### 📅 Smart Scheduling & Routines
* **Time-of-Day Arming:** Automatically arm via HA `schedule` helpers (e.g., 10 PM – 6 AM).
* **Manual Override:** A 'Disable' feature for maintenance that includes an **Auto-Recovery** check every hour to ensure the system is never accidentally left off overnight.

### 📡 Detection Logic
* **mmWave Radar:** Distinguishes between environmental vibration (wind/cats) and actual human presence inside or near the cabin.
* **Intelligent Power:** Car unit remains in deep sleep until the SW-420 triggers a hardware wake-up.

### 🔘 Triple-Layer Kill Switch
1. **Local:** Top/Bottom buttons on the House Unit display.
2. **Hardwired:** GPIO13 "Wall Kill" button (doorbell wire extension).
3. **Mobile:** Actionable notifications on the Home Assistant app.

---

## 🔌 Hardware & Wiring

### 🏠 House Siren Unit (Unit 2)
* **Relay IN:** GPIO12
* **Wall Kill Button:** GPIO13 ➔ GND (Internal Pullup)
* **Status Display:** Built-in ST7789 (`invert: true` enabled in YAML)

### 🚗 Car Sensor Unit (Unit 1)
* **SW-420 (Wake):** GPIO1
* **LD2410C TX/RX:** GPIO3 / GPIO2
* **Battery ADC:** GPIO4 (100k/100k Voltage Divider)

---

## ⚠️ Safety Note
The house unit switches **12V DC only**. Do not attempt to switch mains AC (120V/240V) directly with the relay modules provided in this project.
**
