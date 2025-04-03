# AMDSG08 ESPHome Package

This package provides support for the AMDSG08 8-channel DS18B20 temperature sensor module with RS485/Modbus interface.

## Features

- Support for 8 DS18B20 temperature sensors
- Temperature readings in Celsius
- Configurable temperature offsets for each channel
- RS485 communication settings (address, baud rate, parity)
- Automatic temperature reporting interval configuration

## Installation

There are two ways to use this package:

### Method 1: External Components (Recommended)

Add the following to your ESPHome device configuration:

```yaml
external_components:
  - source: github://wectrl-net/esphome-packages
    components: [amdsg08]

# Configure UART for Modbus
uart:
  id: modbus1
  tx_pin: GPIO1  # Adjust to your hardware
  rx_pin: GPIO3  # Adjust to your hardware
  baud_rate: 9600
  stop_bits: 1

# Include the package
amdsg08:
  # Optional: Custom settings (all settings shown below are defaults)
  amdsg08_prefix: "amdsg08"
  amdsg08_modbus_address: "0x01"
  amdsg08_update_interval: 30s
  amdsg08_sensor_1_name: "DS18B20 Channel 1"
  # ... other sensor names can be customized as needed
```

### Method 2: Manual Installation

1. Copy the `amdsg08.yaml` file to your ESPHome configuration directory
2. Include it in your device configuration:

```yaml
packages:
  amdsg08: !include amdsg08.yaml
```

## Configuration Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `amdsg08_prefix` | Prefix for all entities | `amdsg08` |
| `amdsg08_modbus_address` | Modbus address of the device | `0x01` |
| `amdsg08_update_interval` | Update interval for sensor readings | `30s` |
| `amdsg08_sensor_1_name` through `amdsg08_sensor_8_name` | Names for each temperature sensor | `DS18B20 Channel 1-8` |

## Requirements

- ESPHome 2023.12.0 or newer
- ESP32 or ESP8266 device
- Configured Modbus UART component (must have ID: `modbus1`)

## Example Configuration

```yaml
# Complete example configuration
esphome:
  name: temperature_hub
  platform: ESP32
  board: esp32dev

# Configure WiFi
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

# Enable API and OTA
api:
ota:

# Configure UART for Modbus
uart:
  id: modbus1
  tx_pin: GPIO17
  rx_pin: GPIO16
  baud_rate: 9600
  stop_bits: 1

# Method 1: Using external components
external_components:
  - source: github://wectrl-net/esphome-packages
    components: [amdsg08]

# Include the AMDSG08 component
amdsg08:
  amdsg08_prefix: "temperature"
  amdsg08_sensor_1_name: "Tank Temperature"
  amdsg08_sensor_2_name: "Outdoor Temperature"
```

## Entities Created

### Sensors
- 8 temperature sensors (one per channel)
  - Unit: °C
  - Accuracy: 0.1°C
  - Update interval: configurable

### Number Entities
- RS485 Address (1-247)
- Baud Rate (0-7)
- Parity (0-2)
- Auto Temperature Report Interval (0-255 seconds)
- Temperature Offsets for each channel (-100°C to +100°C) 