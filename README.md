# ESPHome Packages

A collection of ESPHome packages for various devices and sensors.

## Available Packages

### AMDSG08
An 8-channel DS18B20 temperature sensor module with RS485/Modbus interface. [Documentation](amdsg08/README.md)

## Installation

To use these packages in your ESPHome configuration:

1. Clone this repository or download the package you need
2. Copy the package files to your ESPHome configuration directory
3. Include the package in your device configuration:

```yaml
packages:
  device_name: !include device_name/device_name.yaml
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.