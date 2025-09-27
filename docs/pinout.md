# PikaDevBoard Pinout Reference

This document provides detailed pinout information for the PikaDevBoard based on the STM32F103C8T6 microcontroller.

## STM32F103C8T6 Overview

- **Package**: LQFP-48
- **Core**: ARM Cortex-M3, 72MHz
- **Flash**: 64KB
- **SRAM**: 20KB
- **GPIO**: 37 I/O pins
- **Timers**: 3x 16-bit, 1x advanced control
- **Communication**: 2×UART, 2×SPI, 2×I2C, USB, CAN

## Power Pins

| Pin | Name | Function | Connection |
|-----|------|----------|------------|
| 1 | VBAT | Battery backup | Connected to VDD |
| 9 | VDD_1 | Digital power | 3.3V rail |
| 24 | VDD_2 | Digital power | 3.3V rail |
| 36 | VDD_3 | Digital power | 3.3V rail |
| 48 | VDD_4 | Digital power | 3.3V rail |
| 8 | VSS_1 | Digital ground | Ground plane |
| 23 | VSS_2 | Digital ground | Ground plane |
| 35 | VSS_3 | Digital ground | Ground plane |
| 47 | VSS_4 | Digital ground | Ground plane |
| 6 | VDDA | Analog power | 3.3V filtered |
| 7 | VSSA | Analog ground | Ground plane |

## Programming and Debug

| Pin | Name | Function | Header |
|-----|------|----------|---------|
| 46 | SWDIO | Serial Wire Debug I/O | FLASH Header Pin 2 |
| 49 | SWCLK | Serial Wire Clock | FLASH Header Pin 4 |
| 44 | BOOT0 | Boot mode selection | BOOT Header |
| 7 | NRST | Reset (active low) | RESET button |

## GPIO and Peripheral Assignments

### Port A (GPIOA)

| Pin | GPIO | Function | PikaBoard Connection | Notes |
|-----|------|----------|---------------------|-------|
| 10 | PA0 | ADC_IN0 | Available on GPIO header | ADC capable |
| 11 | PA1 | ADC_IN1 | Available on GPIO header | ADC capable |
| 12 | PA2 | USART2_TX | USART1 Header Pin 1 | Serial transmit |
| 13 | PA3 | USART2_RX | USART1 Header Pin 2 | Serial receive |
| 14 | PA4 | ADC_IN4 | Available on GPIO header | ADC capable |
| 15 | PA5 | SPI1_SCK | Available on GPIO header | SPI clock |
| 16 | PA6 | SPI1_MISO | Available on GPIO header | SPI master in |
| 17 | PA7 | SPI1_MOSI | Available on GPIO header | SPI master out |
| 29 | PA8 | MCO | Available on GPIO header | Clock output |
| 30 | PA9 | USART1_TX | Available on GPIO header | Alternate UART |
| 31 | PA10 | USART1_RX | Available on GPIO header | Alternate UART |
| 32 | PA11 | USB_DM | USB Connector | USB data minus |
| 33 | PA12 | USB_DP | USB Connector | USB data plus |
| 34 | PA13 | SWDIO | Programming header | Debug interface |
| 37 | PA14 | SWCLK | Programming header | Debug clock |
| 38 | PA15 | Available | GPIO header | General purpose |

### Port B (GPIOB)

| Pin | GPIO | Function | PikaBoard Connection | Notes |
|-----|------|----------|---------------------|-------|
| 18 | PB0 | ADC_IN8 | Available on GPIO header | ADC capable |
| 19 | PB1 | ADC_IN9 | Available on GPIO header | ADC capable |
| 20 | PB2 | BOOT1 | Available on GPIO header | Boot mode |
| 39 | PB3 | Available | GPIO header | General purpose |
| 40 | PB4 | Available | GPIO header | General purpose |
| 41 | PB5 | Available | GPIO header | General purpose |
| 42 | PB6 | I2C1_SCL | I2C2 Header Pin 3 | I2C clock |
| 43 | PB7 | I2C1_SDA | I2C2 Header Pin 4 | I2C data |
| 45 | PB8 | I2C1_SCL | Available | Alternate I2C |
| 46 | PB9 | I2C1_SDA | Available | Alternate I2C |
| 21 | PB10 | I2C2_SCL | Available | I2C clock |
| 22 | PB11 | I2C2_SDA | Available | I2C data |
| 25 | PB12 | SPI2_NSS | Available | SPI slave select |
| 26 | PB13 | SPI2_SCK | Available | SPI clock |
| 27 | PB14 | SPI2_MISO | Available | SPI master in |
| 28 | PB15 | SPI2_MOSI | Available | SPI master out |

### Port C (GPIOC)

| Pin | GPIO | Function | PikaBoard Connection | Notes |
|-----|------|----------|---------------------|-------|
| 2 | PC13 | Available | Available on GPIO header | General purpose |
| 3 | PC14 | OSC32_IN | Available | RTC oscillator |
| 4 | PC15 | OSC32_OUT | Available | RTC oscillator |

## Onboard Peripheral Connections

### LEDs
- **LED1**: Connected to GPIO pin (via 220Ω resistor)
- **LED2**: Connected to GPIO pin (via 220Ω resistor)  
- **LED3**: Connected to GPIO pin (via 220Ω resistor)
- **LED4**: Connected to GPIO pin (via 220Ω resistor)

### Buttons
- **KEY1**: Connected to GPIO pin with 10kΩ pull-up
- **KEY2**: Connected to GPIO pin with 10kΩ pull-up
- **KEY3**: Connected to GPIO pin with 10kΩ pull-up
- **KEY4**: Connected to GPIO pin with 10kΩ pull-up
- **RESET**: Connected to NRST pin

### Sensors (I2C Bus)
- **BME680**: Environmental sensor (I2C address: 0x77)
- **MPU6050**: Motion sensor (I2C address: 0x68)
- **SSD1306**: OLED display (I2C address: 0x3C)

### Analog Inputs
- **LDR**: Light-dependent resistor on ADC channel
- **Multiple ADC**: Available on GPIO headers

### Communication Interfaces
- **USB**: Full-speed USB 2.0 interface
- **UART**: Available on dedicated header
- **I2C**: Dedicated connector for external sensors
- **SPI**: Available on GPIO headers

## Header Pinouts

### USART1 Header (4-pin)
1. **TX** - USART2_TX (PA2)
2. **RX** - USART2_RX (PA3)  
3. **VCC** - 3.3V power
4. **GND** - Ground

### I2C2 Header (4-pin)
1. **VCC** - 3.3V power
2. **GND** - Ground
3. **SCL** - I2C clock (PB6)
4. **SDA** - I2C data (PB7)

### FLASH Header (4-pin)
1. **VCC** - 3.3V power
2. **SWDIO** - Debug I/O (PA13)
3. **GND** - Ground  
4. **SWCLK** - Debug clock (PA14)

### BOOT Header (2x3)
Boot mode selection jumpers for programming

### GPIO Headers
Multiple headers providing access to:
- Digital I/O pins
- ADC channels
- PWM outputs  
- SPI/I2C interfaces
- Power (3.3V/GND)

## Configuration Notes

### Clock Configuration
- **External Crystal**: 8MHz
- **System Clock**: 72MHz (via PLL)
- **APB1**: 36MHz max
- **APB2**: 72MHz max

### Power Consumption
- **Active mode**: ~25mA @ 72MHz
- **Sleep mode**: ~2mA
- **Standby mode**: ~2μA

### Programming Configuration
- Use **ST-Link V2** programmer
- Connect to **FLASH header**
- Set **BOOT0 = 0** for normal operation
- Set **BOOT0 = 1** for bootloader mode

## Pin Assignment Best Practices

1. **Reserve pins** for onboard peripherals (sensors, display, LEDs)
2. **Use ADC-capable pins** for analog sensors
3. **Group I2C devices** on same bus when possible  
4. **Leave SPI pins** available for expansion
5. **Use timer pins** for PWM applications
6. **Consider interrupt capability** for button inputs

This pinout reference helps you understand how to connect external components and configure the STM32F103C8T6 for your specific applications on the PikaDevBoard.