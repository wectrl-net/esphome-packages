# AMDSG08 ESPHome Package

This package provides support for the AMDSG08 8-channel DS18B20 temperature sensor module with RS485/Modbus interface.

## Features

- Support for 8 DS18B20 temperature sensors
- Temperature readings in Celsius
- Configurable temperature offsets for each channel
- RS485 communication settings (address, baud rate, parity)
- Automatic temperature reporting interval configuration

## Installation

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

The package is designed to allow multiple AMDSG08 devices to be defined in a single ESPHome configuration file by utilizing different substitution variables. The `amdsg08_prefix` and `amdsg08_modbus_address` variables allow you to create distinct configurations for each device.

### Example: Configuring Multiple Devices

```yaml
# In your main ESPHome configuration
uart:
  id: modbus1
  tx_pin: GPIO1
  rx_pin: GPIO3
  baud_rate: 9600
  stop_bits: 1

# First AMDSG08 device
substitutions:
  amdsg08_prefix: "amdsg08_1"
  amdsg08_modbus_address: "0x01"
  amdsg08_update_interval: 30s
  amdsg08_sensor_1_name: "Device 1 Channel 1"
  amdsg08_sensor_2_name: "Device 1 Channel 2"
  # ... and so on for all 8 channels

packages:
  amdsg08_device1: !include amdsg08.yaml

# Second AMDSG08 device
substitutions:
  amdsg08_prefix: "amdsg08_2"
  amdsg08_modbus_address: "0x02"
  amdsg08_update_interval: 30s
  amdsg08_sensor_1_name: "Device 2 Channel 1"
  amdsg08_sensor_2_name: "Device 2 Channel 2"
  # ... and so on for all 8 channels

packages:
  amdsg08_device2: !include amdsg08.yaml
```

### Making the modbus_id Configurable

To make the `modbus_id` configurable as well, you can add another substitution variable to the package. Edit the `amdsg08.yaml` file to add:

```yaml
substitutions:
  # ... existing substitutions ...
  amdsg08_modbus_id: "modbus1"  # Add this line
  
modbus_controller:
  - id: ${amdsg08_prefix}_modbus_controller
    address: ${amdsg08_modbus_address}
    modbus_id: ${amdsg08_modbus_id}  # Use the variable here
    setup_priority: -10
    update_interval: ${amdsg08_update_interval}
```

Then in your configuration:

```yaml
# First AMDSG08 device
substitutions:
  amdsg08_prefix: "amdsg08_1"
  amdsg08_modbus_address: "0x01"
  amdsg08_modbus_id: "modbus1"  # Specify the modbus_id for each device
  # ... other substitutions

packages:
  amdsg08_device1: !include amdsg08.yaml

# Second AMDSG08 device
substitutions:
  amdsg08_prefix: "amdsg08_2"
  amdsg08_modbus_address: "0x02"
  amdsg08_modbus_id: "modbus1"  # Can use the same or different modbus_id
  # ... other substitutions

packages:
  amdsg08_device2: !include amdsg08.yaml
```

This approach allows you to:
- Define multiple AMDSG08 devices in a single configuration
- Use different prefixes and Modbus addresses for each device
- Optionally use different Modbus interfaces (modbus_id) if needed
- Keep unique naming for all sensors across devices

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