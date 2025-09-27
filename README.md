# PikaDevBoard 🔧⚡

## 📋 Specifications

| Feature | Details |
|---------|----------|
| **Microcontroller** | STM32F103C8T6 (ARM Cortex-M3, 72MHz) |
| **Memory** | 64KB Flash, 20KB SRAM |
| **Display** | SSD1306 OLED 128×64 pixels (I2C) |
| **Sensors** | BME680 (temp/humidity/pressure/gas), MPU6050 (accel/gyro), LDR |
| **User Interface** | 4 LEDs, 4 buttons + reset, buzzer |
| **Connectivity** | USB-B, 4×GPIO headers, I2C/UART breakouts |
| **Programming** | ST-Link V2 via SWD interface |
| **Power** | USB (5V) or external, onboard 3.3V/2.5V regulators |
| **Crystal** | 8MHz external crystal oscillator |
| **Dimensions** | Compact PCB with mounting holes |IT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![STM32](https://img.shields.io/badge/STM32-Compatible-blue.svg)](https://www.st.com/en/microcontrollers-microprocessors/stm32-32-bit-arm-cortex-mcus.html)
[![Open Source](https://img.shields.io/badge/Open%20Source-Hardware-green.svg)](https://www.oshwa.org/)

> **A versatile, open-source development board designed for STM32 microcontroller projects**

PikaDevBoard is a comprehensive development platform that provides everything you need to start working with STM32 microcontrollers. Designed with both beginners and experienced developers in mind, it offers a clean, well-documented hardware platform with extensive software support.

## 🌟 Features

- **STM32F103C8T6**: 32-bit ARM Cortex-M3 microcontroller (72MHz, 64KB Flash, 20KB RAM)
- **Rich Sensors**: BME680 environmental sensor, MPU6050 motion sensor, photoresistor
- **Visual Feedback**: 4 user LEDs, SSD1306 OLED display (128x64 I2C)
- **User Interface**: 4 programmable buttons + reset button, buzzer for audio feedback
- **Connectivity**: USB interface, multiple GPIO headers, I2C/UART/SPI interfaces
- **Power Management**: Onboard voltage regulator, USB or external power
- **Programming**: ST-Link V2 compatible, dedicated programming headers
- **Expandable**: Multiple GPIO breakout headers for sensors and actuators
- **Open Source**: Complete schematics, PCB files, BOM, and software examples

## 📋 Specifications

| Feature | Details |
|---------|---------|
| **Microcontroller** | STM32 series compatible |
| **Programming Interface** | ST-Link V2 |
| **Power Supply** | USB or external (5V/3.3V) |
| **Digital I/O** | Multiple GPIO pins |
| **Analog Inputs** | ADC channels |
| **Communication** | UART, SPI, I2C |
| **PWM Outputs** | Multiple channels |
| **LEDs** | Status and user LEDs |
| **Buttons** | Reset and user buttons |

## 🚀 Getting Started

### Prerequisites

Before you start, make sure you have:

- PikaDevBoard hardware
- ST-Link V2 programmer/debugger
- STM32CubeIDE (latest version)
- USB cable for power and programming

### Quick Start Guide

1. **Hardware Setup**
   ```
   1. Connect your STM32 microcontroller to the PikaDevBoard
   2. Connect ST-Link V2 to the programming header
   3. Connect USB cable for power
   4. Verify all connections are secure
   ```

2. **Software Setup**
   ```
   1. Install STM32CubeIDE from ST's website
   2. Clone this repository
   3. Open the example projects in STM32CubeIDE
   4. Build and flash your first program
   ```

3. **First Program**
   ```c
   // Simple LED blink example
   #include "main.h"
   
   int main(void) {
       HAL_Init();
       SystemClock_Config();
       MX_GPIO_Init();
       
       while (1) {
           HAL_GPIO_TogglePin(LED_GPIO_Port, LED_Pin);
           HAL_Delay(500);
       }
   }
   ```

## 📁 Repository Structure

```
PikaDevBoard/
├── hardware/                  # Hardware design files
│   ├── schematics/           # Schematic files
│   ├── pcb/                  # PCB layout files
│   ├── bom/                  # Bill of Materials
│   └── assembly/             # Assembly instructions
├── software/                 # Software examples and libraries
│   ├── examples/             # Example projects
│   │   ├── blink/           # Basic LED blink
│   │   ├── uart/            # Serial communication
│   │   ├── adc/             # Analog input reading
│   │   └── pwm/             # PWM output control
│   ├── libraries/            # Reusable code libraries
│   └── templates/            # Project templates
├── docs/                     # Documentation
│   ├── getting-started.md    # Detailed setup guide
│   ├── pinout.md            # Pin configuration guide
│   ├── stlink-setup.md      # ST-Link configuration
│   └── troubleshooting.md   # Common issues and solutions
├── tools/                    # Development tools and utilities
└── images/                   # Photos and diagrams
```

## 🔧 Hardware Information

### Core Components

- **STM32F103C8T6**: Main microcontroller (LQFP-48 package)
- **BME680**: Environmental sensor (temperature, humidity, pressure, VOC)
- **MPU6050**: 6-axis motion tracking (accelerometer + gyroscope)
- **SSD1306**: OLED display controller (128×64 pixels)
- **LM1117**: Voltage regulators for stable power supply

### User Interface

- **4 LEDs**: Individually controllable status indicators
- **5 Buttons**: 4 user programmable + 1 reset button
- **OLED Display**: 128×64 pixel monochrome display
- **Buzzer**: Audio feedback for user interactions
- **LDR**: Light-dependent resistor for ambient light sensing

### Connectivity

- **USB Interface**: Power and communication
- **GPIO Headers**: Multiple expansion connectors
- **I2C Header**: Dedicated sensor bus connector
- **UART Header**: Serial communication breakout
- **Programming Headers**: BOOT and FLASH configuration

### Power Requirements

- **Input**: 5V via USB-B connector
- **Regulation**: Onboard LM1117 regulators (3.3V/2.5V)
- **Consumption**: ~50-200mA depending on active peripherals

### Programming Interface

Standard ST-Link V2 SWD connection:
- **SWDIO**: Serial Wire Debug I/O
- **SWCLK**: Serial Wire Clock  
- **GND**: Ground reference
- **VCC**: 3.3V power

## 💻 Software Examples

### Available Examples

1. **Basic Examples**
   - **Multi-LED Control**: Control 4 individual LEDs with different patterns
   - **Button Matrix**: Handle 4 user buttons with debouncing
   - **OLED Display**: Text and graphics on SSD1306 display
   - **Buzzer Control**: Generate tones and melodies

2. **Sensor Examples**
   - **Environmental Monitoring**: BME680 temperature, humidity, pressure, gas
   - **Motion Detection**: MPU6050 accelerometer and gyroscope readings
   - **Light Sensing**: LDR photoresistor for ambient light measurement
   - **Data Logging**: Combine all sensors with timestamp logging

3. **Communication Examples**
   - **USB Serial**: Debug output and command interface
   - **I2C Scanner**: Detect and communicate with I2C devices
   - **Sensor Dashboard**: Real-time sensor data display on OLED
   - **Remote Control**: Button-controlled device interactions

4. **Advanced Applications**
   - **Weather Station**: Complete environmental monitoring system
   - **Motion Tracker**: Gesture recognition and motion analysis
   - **Smart Alarm**: Light-sensitive alarm with snooze functionality
   - **IoT Data Logger**: Sensor data collection and transmission

### Building Examples

```bash
# Navigate to example directory
cd software/examples/blink

# Open in STM32CubeIDE
# File -> Import -> Existing Projects into Workspace
# Select the example folder and import
```

## 📚 Documentation

- **[Getting Started Guide](docs/getting-started.md)** - Complete setup instructions
- **[Pinout Reference](docs/pinout.md)** - Detailed pin descriptions
- **[ST-Link Setup](docs/stlink-setup.md)** - Programming interface configuration
- **[Troubleshooting](docs/troubleshooting.md)** - Common issues and solutions
- **[Hardware Design](hardware/README.md)** - Design files and specifications

## 🛠️ Development Tools

### Required Software
- **STM32CubeIDE** - Primary development environment
- **STM32CubeMX** - Code generation and configuration
- **ST-Link Utility** - Programming and debugging

### Optional Tools
- **Serial Terminal** - For UART communication
- **Logic Analyzer** - For signal analysis
- **Oscilloscope** - For analog signal debugging

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

### How to Contribute

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Areas for Contribution

- Additional example projects
- Hardware design improvements
- Documentation enhancements
- Bug fixes and optimizations
- Test coverage improvements

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- ST Microelectronics for the STM32 ecosystem
- The open-source hardware community
- All contributors and users of PikaDevBoard

## 📞 Support

- **Documentation**: Check the `docs/` directory
- **Issues**: Use GitHub Issues for bug reports
- **Discussions**: Use GitHub Discussions for questions
- **Website**: Visit [pen.engineering/pikadevboard](https://pen.engineering/pikadevboard)

## 🔄 Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history and updates.

---

**Happy coding with PikaDevBoard! 🚀**