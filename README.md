# ESPHome Packages

A collection of ESPHome packages for various devices and sensors by WECtrl.

## Available Packages

### AMDSG08
An 8-channel DS18B20 temperature sensor module with RS485/Modbus interface. [Documentation](amdsg08/README.md)

## Installation

There are several ways to use these packages in your ESPHome configuration:

### Method 1: External Components (Recommended)

Add the following to your ESPHome device configuration:

```yaml
external_components:
  - source: github://wectrl-net/esphome-packages
    components: [amdsg08]
```

Then use the component in your configuration:

```yaml
# Required: UART bus with modbus_id: modbus1
uart:
  id: modbus1
  tx_pin: GPIO1
  rx_pin: GPIO3
  baud_rate: 9600
  stop_bits: 1

# Include the package
amdsg08:
  # Optional: Custom settings (defaults shown below)
  amdsg08_prefix: "amdsg08"
  amdsg08_modbus_address: "0x01"
  amdsg08_update_interval: 30s
```

### Method 2: Manual Installation

1. Clone this repository:
   ```bash
   git clone git@github.com:wectrl-net/esphome-packages.git
   ```

2. Copy the required package directory to your ESPHome configuration directory.

3. Include the package in your device configuration:
   ```yaml
   packages:
     amdsg08: !include amdsg08/amdsg08.yaml
   ```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.