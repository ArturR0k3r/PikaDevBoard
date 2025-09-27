# PikaDevBoard Software Examples

This directory contains example projects and code for the PikaDevBoard, designed to work with STM32CubeIDE and ST-Link V2.

## Getting Started

### Prerequisites
- STM32CubeIDE (latest version)
- ST-Link V2 programmer/debugger
- PikaDevBoard with STM32 microcontroller
- Basic knowledge of C programming

### Setup Instructions
1. Install STM32CubeIDE from [ST's website](https://www.st.com/en/development-tools/stm32cubeide.html)
2. Connect your PikaDevBoard to ST-Link V2
3. Power the board via USB or external supply
4. Clone or download this repository

## Available Examples

### Basic Examples
- **`blink/`** - Simple LED blinking (GPIO basics)
- **`uart/`** - Serial communication example
- **`adc/`** - Analog input reading
- **`pwm/`** - PWM output generation

### Intermediate Examples
- **`i2c/`** - I2C communication with sensors
- **`spi/`** - SPI peripheral communication
- **`timer/`** - Advanced timer usage
- **`interrupt/`** - External interrupt handling

### Advanced Examples
- **`dma/`** - Direct Memory Access usage
- **`rtos/`** - Real-time operating system
- **`lowpower/`** - Power management techniques
- **`bootloader/`** - Custom bootloader implementation

## How to Use Examples

### Method 1: Import Existing Project
1. Open STM32CubeIDE
2. File → Import → General → Existing Projects into Workspace
3. Browse to the example directory (e.g., `examples/blink/`)
4. Select the project and click "Finish"
5. Build and flash to your board

### Method 2: Create New Project
1. Use the template files as reference
2. Create new STM32 project in CubeIDE
3. Configure your specific STM32 part number
4. Copy the relevant code sections
5. Modify pin assignments as needed

## Project Structure

Each example follows this structure:
```
example_name/
├── Core/
│   ├── Inc/           # Header files
│   └── Src/           # Source files
├── Drivers/           # HAL and CMSIS drivers
├── .cproject          # Eclipse project file
├── .project           # Eclipse project file
├── example_name.ioc   # CubeMX configuration
└── README.md          # Example-specific documentation
```

## Pin Configuration

### Default Pin Assignments (modify as needed)
- **LED1**: PA5 (User LED)
- **LED2**: PA6 (Status LED)
- **BUTTON1**: PC13 (User button)
- **UART**: PA2 (TX), PA3 (RX)
- **SPI**: PA7 (MOSI), PA6 (MISO), PA5 (SCK)
- **I2C**: PB8 (SCL), PB9 (SDA)

**Note**: Pin assignments may vary based on your specific STM32 part and board layout.

## Programming and Debugging

### Using ST-Link V2
1. Connect ST-Link to programming header:
   - Pin 1: VCC (3.3V)
   - Pin 2: SWDIO
   - Pin 3: GND
   - Pin 4: SWCLK

2. In STM32CubeIDE:
   - Right-click project → Run As → STM32 C/C++ Application
   - Or use Debug perspective for step-through debugging

### Serial Output
Many examples include UART output for debugging:
- **Baud Rate**: 115200
- **Data**: 8 bits, No parity, 1 stop bit
- Use any serial terminal (PuTTY, TeraTerm, Arduino Serial Monitor)

## Troubleshooting

### Common Issues

**Cannot detect STM32**
- Check ST-Link connections
- Verify board power (3.3V present)
- Try connecting under reset
- Check for proper ground connection

**Build Errors**
- Ensure correct STM32 part number selected
- Update HAL libraries if needed
- Check include paths and dependencies

**Code doesn't run**
- Verify system clock configuration
- Check for infinite loops in initialization
- Ensure proper GPIO configuration

**UART not working**
- Verify TX/RX pin configuration
- Check baud rate settings
- Ensure GPIO alternate function is set correctly

### Debug Tips
1. Use LED indicators for basic status
2. Add HAL_Delay() for timing issues
3. Use debugger breakpoints to trace execution
4. Check return values from HAL functions

## Contributing Examples

We welcome contributions! To add an example:

1. Create new directory under appropriate category
2. Include complete STM32CubeIDE project
3. Add comprehensive README with:
   - Purpose and functionality
   - Hardware connections required
   - Expected behavior
   - Troubleshooting notes
4. Test on actual hardware
5. Submit pull request

### Example Template Structure
```c
/* main.c template */
#include "main.h"

// Function prototypes
void SystemClock_Config(void);
void MX_GPIO_Init(void);
// Add other peripheral inits as needed

int main(void) {
    // Initialize HAL
    HAL_Init();
    
    // Configure system clock
    SystemClock_Config();
    
    // Initialize peripherals
    MX_GPIO_Init();
    // Add other peripheral inits
    
    // Main application loop
    while (1) {
        // Your code here
    }
}
```

## Libraries and Utilities

The `libraries/` directory contains:
- **PikaBoard_HAL** - Hardware abstraction layer for common functions
- **Sensors** - Drivers for common sensors and modules
- **Communication** - Protocol implementations (Modbus, CAN, etc.)
- **Utilities** - Helper functions and macros

## Templates

The `templates/` directory contains:
- **Basic_Template** - Minimal project setup
- **RTOS_Template** - FreeRTOS-enabled project
- **Bootloader_Template** - Application with bootloader support
- **Low_Power_Template** - Optimized for battery operation

## Support and Resources

- **STM32 Documentation**: [ST's Developer Zone](https://www.st.com/content/st_com/en/support/learning/stm32-education.html)
- **HAL Documentation**: Included with STM32CubeIDE
- **Community Forums**: [ST Community](https://community.st.com/)
- **GitHub Issues**: Report bugs and ask questions

## License

Software examples are licensed under MIT License. See individual files for specific license information.