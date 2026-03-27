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
