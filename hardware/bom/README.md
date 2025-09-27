# Bill of Materials (BOM)

## PikaDevBoard Components

### Main Components

| Reference | Value | Package | Description | Quantity | Supplier | Part Number |
|-----------|-------|---------|-------------|----------|----------|-------------|
| STM32F103C8T6 | - | LQFP-48 | ARM Cortex-M3 Microcontroller | 1 | LCSC | C8734 |
| AMS1117 | LM1117MPX-25 | SOT-223 | 2.5V Linear Regulator | 1 | LCSC | C2902257 |
| BME680 | - | LGA-8 | Environmental Sensor | 1 | LCSC | C125972 |
| MPU6050 | - | QFN-24 | 6-Axis Motion Sensor | 1 | LCSC | C2912107 |
| SSD1306-OLED | 128x64 | Module | I2C OLED Display | 1 | - | - |
| 8MHZ | 8MHz | SMD Crystal | External Crystal Oscillator | 1 | LCSC | C9900003449 |

### Power Supply Components

| Reference | Value | Package | Description | Quantity | Notes |
|-----------|-------|---------|-------------|----------|-------|
| C24 | 22μF | Electrolytic | Power supply filtering | 1 | Input filter |
| C25 | 10μF | Electrolytic | Regulator output | 1 | Output stabilization |
| C1-C12 | 100nF | 1206/0402 | Decoupling capacitors | 11 | Power supply noise filtering |
| C7 | 220nF | 0402 | Additional filtering | 1 | EMI suppression |
| U7 | 1N5819 | SOD-123 | Schottky Diode | 1 | Reverse polarity protection |

### User Interface Components

| Reference | Value | Package | Description | Quantity | Notes |
|-----------|-------|---------|-------------|----------|-------|
| LED1-LED4 | Red | 1206 | Status LEDs | 4 | User indicators |
| KEY1-KEY4 | 6x6mm | Through-hole | Tactile switches | 4 | User input buttons |
| RESET | 6x6mm | Through-hole | Reset button | 1 | System reset |
| BUZZER | - | Through-hole | Piezo buzzer | 1 | Audio feedback |
| LDR | 5528 | Through-hole | Photoresistor | 1 | Light sensing |

### Resistors

| Reference | Value | Package | Description | Quantity | Application |
|-----------|-------|---------|-------------|----------|-------------|
| R2, R14-R16 | 220Ω | 1206 | LED current limiting | 4 | LED brightness control |
| R4, R6, R17-R18, R21 | 10kΩ | 1206 | Pull-up resistors | 5 | Button debouncing |
| R1 | 470Ω | 1206 | General purpose | 1 | Signal conditioning |
| R3 | 4.7kΩ | 1206 | I2C pull-up | 1 | I2C bus termination |
| R7 | 1MΩ | 0603 | High impedance | 1 | Sensor biasing |
| R9 | 100kΩ | 1206 | LDR voltage divider | 1 | Light sensor circuit |
| R19, R20 | 10Ω | 1206 | Current limiting | 2 | Protection resistors |

### Connectors and Headers

| Reference | Description | Package | Quantity | Notes |
|-----------|-------------|---------|----------|-------|
| USB | USB-B | SMD | 1 | Power and communication |
| BOOT | 2x3 Header | 2.54mm | 1 | Boot mode selection |
| FLASH | 1x4 Header | 2.54mm | 1 | Programming interface |
| GPIO1 | 1x8 Header | 2.54mm | 1 | General purpose I/O |
| GPIO2 | 1x6 Header | 2.54mm | 1 | Additional GPIO |
| USART1 | 1x4 Header | 2.54mm | 1 | Serial communication |
| I2C2 | 1x4 Header | 2.54mm | 1 | I2C bus connector |

### Optional Components

| Reference | Value | Package | Description | Quantity | Notes |
|-----------|-------|---------|-------------|----------|-------|
| X1 | 8MHz | HC-49 | Crystal oscillator | 1 | If not using internal osc |
| C5, C6 | 22pF | 0603 | Crystal load caps | 2 | For external crystal |

## Component Notes

### Power Supply Components
- **AMS1117-3.3**: Provides stable 3.3V for microcontroller
- **Input capacitors**: Filter power supply noise
- **Output capacitors**: Stabilize regulator output

### Programming Interface
- **ST-Link Header**: Standard 4-pin connector
- **Pin 1**: VCC (3.3V)
- **Pin 2**: SWDIO
- **Pin 3**: GND
- **Pin 4**: SWCLK

### User Interface
- **LED1**: Power indicator (Green)
- **LED2**: User LED (Red) - GPIO controlled
- **SW1**: Reset button - connected to MCU reset
- **SW2**: User button - GPIO input with pullup

## Sourcing Information

### Recommended Suppliers

1. **Digikey** - Wide selection, good for prototyping
2. **Mouser** - Professional components, good documentation
3. **LCSC** - Cost-effective for larger quantities
4. **Arrow** - Good for STM32 and ST components

### Cost Estimation (USD)

| Category | Components | Estimated Cost | Notes |
|----------|------------|----------------|-------|
| **Microcontroller** | STM32F103C8T6 | $1.13 | Main processor |
| **Sensors** | BME680, MPU6050, LDR | $9.50 | Environmental + motion sensing |
| **Display** | SSD1306 OLED | $3-5 | 128x64 I2C display |
| **Passive Components** | Resistors, capacitors | $2-3 | 30+ components |
| **Power Components** | Regulators, diodes | $0.50 | Power management |
| **Connectors** | Headers, USB | $1-2 | All interfaces |
| **Mechanical** | Buttons, buzzer | $1-2 | User interface |
| **PCB (5 pcs)** | 4-layer PCB | $15-30 | Professional fab house |
| **Assembly** | SMD + THT components | $10-20 | If professionally assembled |
| **Total per board** | **$15-25** | Depending on quantities and assembly |

## Assembly Notes

1. **Component Orientation**: Pay attention to LED and connector polarity
2. **Soldering Order**: Start with smallest components first
3. **Programming Header**: Ensure correct pin 1 alignment
4. **Testing**: Test power supply before installing MCU

## Alternative Components

### Cost Reduction Options
- Use through-hole components instead of SMD
- Single-sided assembly to reduce costs
- Larger component packages (0805 instead of 0603)

### Enhanced Options
- Add ESD protection diodes
- Include additional filtering capacitors
- Use gold-plated connectors for better reliability

## Files Included

- `bom.csv` - Machine-readable BOM file
- `bom.xlsx` - Excel format with supplier links
- `components.pdf` - Component placement reference