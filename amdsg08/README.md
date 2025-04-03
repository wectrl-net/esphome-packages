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

To use multiple AMDSG08 devices on the same RS485 bus, you need to:

1. Create separate configuration files for each device with different prefixes and Modbus addresses
2. Include these configurations in your main ESPHome configuration

### Step 1: Create Device-Specific Configuration Files

Create a separate file for each device (e.g., `amdsg08_device1.yaml`, `amdsg08_device2.yaml`) with unique substitution values:

**amdsg08_device1.yaml:**
```yaml
substitutions:
  amdsg08_prefix: "amdsg08_1"
  amdsg08_modbus_address: "0x01"
  amdsg08_update_interval: 30s
  amdsg08_sensor_1_name: "Device 1 DS18B20 Channel 1"
  # ... other substitutions with unique names

# Include the base AMDSG08 package
<<: !include amdsg08.yaml
```

**amdsg08_device2.yaml:**
```yaml
substitutions:
  amdsg08_prefix: "amdsg08_2"
  amdsg08_modbus_address: "0x02"
  amdsg08_update_interval: 30s
  amdsg08_sensor_1_name: "Device 2 DS18B20 Channel 1"
  # ... other substitutions with unique names

# Include the base AMDSG08 package
<<: !include amdsg08.yaml
```

### Step 2: Include in Main Configuration

```yaml
# In your main ESPHome configuration
uart:
  id: modbus1
  tx_pin: GPIO1
  rx_pin: GPIO3
  baud_rate: 9600
  stop_bits: 1

packages:
  device1: !include amdsg08_device1.yaml
  device2: !include amdsg08_device2.yaml
```

This approach allows you to:
- Use different Modbus addresses for each device
- Give unique names to each set of sensors
- Customize update intervals per device if needed
- Keep all devices on the same RS485 bus

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