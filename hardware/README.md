# PikaDevBoard Hardware

This directory contains all hardware design files for the PikaDevBoard.

## Directory Structure

- **`schematics/`** - Schematic files (KiCad, Eagle, etc.)
- **`pcb/`** - PCB layout files and Gerber files
- **`bom/`** - Bill of Materials and component specifications
- **`assembly/`** - Assembly instructions and guides

## Hardware Specifications

### Main Features
- STM32 microcontroller socket/headers
- ST-Link V2 programming interface
- Power management (USB + external)
- GPIO breakout with clear labeling
- User LEDs and buttons
- Communication interfaces (UART, SPI, I2C)

### Power Supply
- **Input**: 5V via USB or external jack
- **Output**: 3.3V regulated for microcontroller
- **Current**: Up to 500mA available

### Programming Interface
- **Connector**: Standard 4-pin ST-Link header
- **Signals**: SWDIO, SWCLK, GND, VCC
- **Compatible**: ST-Link V2, ST-Link V3

## Design Files

### Schematic Files
Place your schematic files (.sch, .kicad_sch, etc.) in the `schematics/` directory.

### PCB Layout
Place your PCB files (.pcb, .kicad_pcb, etc.) and Gerber files in the `pcb/` directory.

### Manufacturing
- **PCB Thickness**: 1.6mm standard
- **Copper**: 1oz (35μm)
- **Solder Mask**: Green (or your preference)
- **Silkscreen**: White text

## Component Sourcing

See the `bom/` directory for:
- Complete bill of materials
- Preferred suppliers
- Alternative component options
- Pricing information

## Assembly

See the `assembly/` directory for:
- Step-by-step assembly guide
- Component placement diagrams
- Soldering instructions
- Testing procedures

## Modifications

This is an open-source design! Feel free to:
- Modify the design for your needs
- Add additional features
- Create variants for different applications
- Share your improvements with the community

## License

Hardware designs are licensed under CERN Open Hardware License v2.0.

## Support

For hardware-related questions:
- Check the assembly guide first
- Review the schematic for pin connections
- Post issues on GitHub for community support