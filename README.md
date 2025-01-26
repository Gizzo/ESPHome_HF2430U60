# ESPHome Configuration for SRNE HF2430U60

This repository contains the ESPHome configuration for integrating the SRNE HF2430U60 solar controller with Home Assistant. It would likely work with the similar variants like the HF2430U60, HF2320U60, etc... The setup uses Modbus communication to retrieve detailed data and control various aspects of the device.

## Features
- **Modbus Communication**: Collect real-time data using the UART interface.
- **Comprehensive Sensors**: Includes power, current, voltage, temperature, and historical energy statistics.
- **Home Assistant Integration**: Provides API and web server for seamless integration.
- **Custom Controls**: Features switches, numbers, and selectors for advanced configuration.

## Setup Instructions

### Hardware Requirements
1. **ESP32 Development Board**: Configured for Modbus communication.
2. **SRNE HF2430U60 Solar Controller**: Ensure Modbus is enabled.
3. **Wiring**: Connect the ESP32 UART pins to the solar controller:
   - `TX` → Solar Controller `RX`
   - `RX` → Solar Controller `TX`
   - `GND` → Solar Controller `GND`

### Software Requirements
1. [ESPHome](https://esphome.io/) installed.
2. A configured Home Assistant instance.

### Configuration Steps
1. Clone this repository or download the `SRNE_HF2430U60.yaml` file.
2. Adjust the following in the YAML file:
   - `wifi.ssid` and `wifi.password` (use `secrets.yaml` for secure storage).
   - `api.encryption.key` and `ota.password`.
3. Compile and upload the configuration to your ESP32 device.

### Upload Instructions
1. Use ESPHome Dashboard or CLI:
   ```bash
   esphome run SRNE_HF2430U60.yaml
   ```
2. Follow the prompts to upload the firmware.

## Key Configuration Highlights

### Modbus Setup
- **UART**:
  ```yaml
  uart:
    id: mod_bus
    tx_pin: GPIO1
    rx_pin: GPIO3
    baud_rate: 9600
  ```
- **Modbus Controller**:
  ```yaml
  modbus_controller:
    - id: srne1
      address: 0x01
      modbus_id: tdt1_modbus
  ```

### Sensors
Example sensor configuration for grid voltage:
```yaml
sensor:
  - platform: modbus_controller
    modbus_controller_id: srne1
    name: "Grid Voltage"
    register_type: holding
    address: 0x0213
    unit_of_measurement: "V"
    accuracy_decimals: 1
    device_class: voltage
    state_class: measurement
```

### Web Server
Provides local access:
```yaml
web_server:
  port: 80
  auth:
    username: "JohnDoe"
    password: "abc123456"
```

## Known Issues
- Some Modbus registers may return unexpected values.
- Ensure wiring is secure to avoid communication errors.

## Future Enhancements
- Add support for additional Modbus registers.
- Improve error handling for invalid data.

## License
This project is licensed under the MIT License. See the `LICENSE` file for details.

---

For detailed documentation, visit the [ESPHome Website](https://esphome.io/).
