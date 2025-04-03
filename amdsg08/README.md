# AMDSG08 ESPHome Package

This package provides support for the AMDSG08 8-channel DS18B20 temperature sensor module with RS485/Modbus interface.

## Features

- Support for 8 DS18B20 temperature sensors
- Temperature readings in Celsius
- Configurable temperature offsets for each channel
- RS485 communication settings (address, baud rate, parity)
- Automatic temperature reporting interval configuration

## Installation

### Local Installation
1. Copy the `amdsg08.yaml` file to your ESPHome configuration directory
2. Include it in your device configuration:

```yaml
packages:
  amdsg08: !include amdsg08.yaml
```

### External Package Installation (Recommended)
Use ESPHome's external packages feature to include this package directly from GitHub:

```yaml
packages:
  amdsg08:
    url: https://github.com/wectrl-net/esphome-packages
    file: amdsg08/amdsg08.yaml
    refresh: 1d
```

## Configuration Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `amdsg08_prefix` | Prefix for all entities | `amdsg08` |
| `amdsg08_modbus_address` | Modbus address of the device | `0x01` |
| `amdsg08_update_interval` | Update interval for sensor readings | `30s` |
| `amdsg08_modbus_id` | ID of the modbus component to use | `modbus1` |
| `amdsg08_sensor_1_name` through `amdsg08_sensor_8_name` | Names for each temperature sensor | `DS18B20 Channel 1-8` |

## Requirements

- ESPHome 2023.12.0 or newer
- ESP32 or ESP8266 device
- Configured Modbus UART component

## Example Configuration

```yaml
# In your device's yaml configuration
uart:
  id: modbus1
  tx_pin: GPIO1
  rx_pin: GPIO3
  baud_rate: 9600
  stop_bits: 1

packages:
  amdsg08: !include amdsg08.yaml
```

## Using Multiple AMDSG08 Devices

The package is designed to allow multiple AMDSG08 devices to be easily configured by using ESPHome's external packages feature with custom substitutions for each device.

### Recommended Approach: Using External Packages

This is the cleanest and most maintainable approach for defining multiple devices:

```yaml
# In your main ESPHome configuration
uart:
  id: modbus1
  tx_pin: GPIO1
  rx_pin: GPIO3
  baud_rate: 9600
  stop_bits: 1

# First AMDSG08 device
packages:
  temp_rack1:
    url: https://github.com/wectrl-net/esphome-packages
    file: amdsg08/amdsg08.yaml
    refresh: 1d
    substitutions:
      amdsg08_prefix: "rack1"
      amdsg08_modbus_address: "0x01"
      amdsg08_modbus_id: "modbus1"
      amdsg08_sensor_1_name: "Rack 1 Temp Sensor 1"
      amdsg08_sensor_2_name: "Rack 1 Temp Sensor 2"
      # ... other sensor names as needed

  # Second AMDSG08 device
  temp_rack2:
    url: https://github.com/wectrl-net/esphome-packages
    file: amdsg08/amdsg08.yaml
    refresh: 1d
    substitutions:
      amdsg08_prefix: "rack2"
      amdsg08_modbus_address: "0x02"
      amdsg08_modbus_id: "modbus1" 
      amdsg08_sensor_1_name: "Rack 2 Temp Sensor 1"
      amdsg08_sensor_2_name: "Rack 2 Temp Sensor 2"
      # ... other sensor names as needed
```

Benefits of this approach:
- Clean configuration with less repetition
- Automatic updates when the package is updated (based on refresh interval)
- No need to manually copy files
- Easy to add and remove devices

### Alternative Approach: Local Packages

You can also use local package includes with different substitutions for each device:

```yaml
# First AMDSG08 device
substitutions:
  amdsg08_prefix: "rack1"
  amdsg08_modbus_address: "0x01"
  amdsg08_update_interval: 30s
  amdsg08_sensor_1_name: "Rack 1 Temp Sensor 1"
  # ... and so on for all 8 channels

packages:
  amdsg08_device1: !include amdsg08.yaml

# Second AMDSG08 device
substitutions:
  amdsg08_prefix: "rack2"
  amdsg08_modbus_address: "0x02"
  amdsg08_update_interval: 30s
  amdsg08_sensor_1_name: "Rack 2 Temp Sensor 1"
  # ... and so on for all 8 channels

packages:
  amdsg08_device2: !include amdsg08.yaml
```

**Note:** Each AMDSG08 device must have a unique Modbus address configured using its hardware settings.

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