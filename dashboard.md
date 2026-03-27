### 📊 Home Assistant Dashboard (Lovelace)
This dashboard configuration uses **Mushroom Cards** (available via HACS) to provide a clean, "Security Console" interface for your system. It includes conditional logic to show a massive "Kill Siren" button only when the alarm is actively screaming.

#### System Control Card (YAML)
```yaml
type: vertical-stack
cards:
  - type: custom:mushroom-title-card
    title: 🚨 PNWC Vehicle Security
    subtitle: System Status & Controls

  # --- Status Row (Chips) ---
  - type: custom:mushroom-chips-card
    chips:
      - type: entity
        entity: binary_sensor.car_security_sensor_car_power_detected
        content_info: name
        name: Engine/Charging
      - type: entity
        entity: sensor.car_security_sensor_car_battery_voltage
        icon: mdi:battery-check
      - type: entity
        entity: binary_sensor.car_security_sensor_car_presence
        content_info: state
        name: Presence

  # --- Main Control Row ---
  - type: horizontal-stack
    cards:
      - type: custom:mushroom-entity-card
        entity: input_boolean.car_alarm_armed
        name: Alarm System
        icon: mdi:shield-lock
        icon_color: red
        layout: vertical
        tap_action:
          action: toggle
        hold_action:
          action: more-info

      - type: custom:mushroom-entity-card
        entity: input_boolean.car_alarm_maintenance
        name: Maintenance
        icon: mdi:wrench
        icon_color: orange
        layout: vertical
        tap_action:
          action: toggle

  # --- Siren Override (Only shows if Siren is ON) ---
  - type: conditional
    conditions:
      - entity: switch.house_siren_siren_relay
        state: 'on'
    card:
      type: custom:mushroom-template-card
      primary: !! SIREN IS ACTIVE !!
      secondary: Tap to Kill Siren Immediately
      icon: mdi:alarm-light
      icon_color: red
      layout: horizontal
      fill_container: true
      tap_action:
        action: call-service
        service: switch.turn_off
        target:
          entity_id: switch.house_siren_siren_relay

  # --- Diagnostic Info ---
  - type: entities
    entities:
      - entity: sensor.car_security_sensor_car_detection_distance
        name: Radar Range
      - entity: text_sensor.house_siren_last_trigger_source
        name: Last Alarm Trigger
      - entity: sensor.car_security_sensor_car_sensor_wifi_rssi
        name: Car Signal Strength
    show_header_toggle: false
    state_color: true
```

---

### 🎥 Frigate Live View Integration
Add this card below your controls to get a real-time feed of the vehicle.

```yaml
type: custom:frigate-card
camera_entity: camera.driveway
view:
  default: live
menu:
  buttons:
    frigate:
      enabled: true
    timeline:
      enabled: true
live:
  preload: true
```

---

### 🛠️ Setup Instructions
1. **Requirements:** Ensure **Mushroom Cards** and **Frigate Card** are installed via HACS (Home Assistant Community Store).
2. **Manual Card:** Create a new card on your dashboard, select **Manual**, and paste the System Control Card YAML first.
3. **Entity Verification:** If your entities appear as `Entity not found`, go to **Settings > Devices & Services > ESPHome** and ensure the names match (e.g., check if your sensor is `car_security_car_battery_voltage` instead of the one listed).
