# ST-Link V2 Setup and Configuration

This guide covers everything you need to know about setting up and using the ST-Link V2 programmer/debugger with your PikaDevBoard.

## What is ST-Link V2?

The ST-Link V2 is ST Microelectronics' official programming and debugging interface for STM32 microcontrollers. It provides:

- **In-Circuit Programming** - Flash firmware to STM32
- **Real-Time Debugging** - Step through code, set breakpoints
- **Virtual COM Port** - UART communication through USB
- **SWD Interface** - Serial Wire Debug protocol
- **Target Power** - Can power target board (limited current)

## ST-Link V2 Variants

### Original ST-Link V2
- **White plastic case** with ST logo
- **Mini USB connector**
- **4-pin SWD connector** + jumper wires
- **Status LED indicators**

### ST-Link V2 Clone
- **Black or colored case**
- **Often mini USB or USB-A**
- **Same functionality** as original
- **Lower cost** alternative

### ST-Link V2 ISOL
- **Electrical isolation** between PC and target
- **Higher price** but better protection
- **Recommended for professional use**

## Hardware Connection

### Standard 4-Wire SWD Connection

```
ST-Link V2 Pin    Signal    PikaDevBoard Pin
     1            VCC       3.3V (Pin 1)
     2            SWDIO     SWDIO (Pin 2)
     3            GND       GND (Pin 3)  
     4            SWCLK     SWCLK (Pin 4)
```

### Connection Diagram
```
    ST-Link V2                PikaDevBoard
   ┌──────────┐              ┌─────────────┐
   │ 1  2     │              │ 1  2        │
   │ │  │     │──────────────│ │  │        │
   │ 3  4     │   4-wire     │ 3  4        │  
   └──────────┘   cable      └─────────────┘
        │                           │
        │                           │
     USB to PC               STM32 MCU
```

### Important Notes
- **Pin 1 orientation** - Look for triangle or dot marking
- **VCC connection** - Can power board or just reference voltage
- **Ground is critical** - Must have solid ground connection
- **Wire length** - Keep connections short (< 20cm) for reliability

## Driver Installation

### Windows Automatic Installation
1. **Connect ST-Link** to PC via USB
2. **Windows 10/11** should auto-install drivers
3. **Check Device Manager**:
   - Expand "Universal Serial Bus controllers"
   - Look for "STMicroelectronics STLink dongle"

### Manual Driver Installation (Windows)
If auto-installation fails:

1. **Download** STSW-LINK009 from ST website
2. **Extract** and run `stlink_winusb_install.bat` as administrator
3. **Reboot** computer
4. **Reconnect** ST-Link and verify in Device Manager

### macOS Installation
```bash
# Install using Homebrew
brew install stlink

# Or download from ST website
# No additional drivers usually needed
```

### Linux Installation
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install stlink-tools

# CentOS/RHEL  
sudo yum install stlink

# Add user to dialout group for permissions
sudo usermod -a -G dialout $USER
# Log out and back in for changes to take effect
```

## Verification and Testing

### Check Connection in STM32CubeIDE
1. **Open STM32CubeIDE**
2. **Window** → **Show View** → **Other**
3. **STM32** → **STM32 ST-LINK GDB Server**
4. **Should display**: Connected ST-Link with serial number

### Command Line Testing

#### Windows (CubeIDE Installation Directory)
```cmd
cd "C:\ST\STM32CubeIDE_[version]\STM32CubeIDE\plugins\com.st.stm32cube.ide.mcu.externaltools.stlink-gdb-server.win32_[version]\tools\bin"

st-info --probe
```

#### Linux/macOS
```bash
# Check if ST-Link is detected
st-info --probe

# Get detailed information
st-info --descr

# Expected output example:
# Found 1 stlink programmers
# serial:     53FF6B064966545035361587
# hla-serial: "\x53\xff\x6b\x06\x49\x66\x54\x50\x35\x36\x15\x87"
# flash:      262144 (pagesize: 2048)
# sram:       65536
# chipid:     0x0431
# descr:      F4xx (Dynamic Efficency)
```

### Test Programming
```bash
# Read device info
st-info --probe

# Read memory (should not fail if connected properly)
st-flash read readback.bin 0x8000000 0x1000

# If successful, ST-Link and target are properly connected
```

## STM32CubeIDE Configuration

### Debug Configuration Setup
1. **Right-click project** → **Debug As** → **Debug Configurations**
2. **Double-click** "STM32 C/C++ Application"
3. **Configure Debugger tab**:

#### Main Settings
- **Debug probe**: ST-LINK (OpenOCD)
- **Interface**: SWD
- **Frequency**: 4000 kHz (start with lower if issues)
- **Access Port**: 0
- **Reset Mode**: Software system reset

#### Advanced Settings
- **External loader**: Not used (for external flash)
- **Shared**: Unchecked
- **Enable live expressions**: Checked (for real-time debugging)

### Connection Options

#### Connect Under Reset
- **When to use**: If normal connection fails
- **How to enable**: Check "Connect under reset" in debug config
- **What it does**: Holds reset during connection attempt

#### Hot Plug Detection
- **Enable**: "Hot plug" checkbox
- **Allows**: Connecting while target is running
- **Useful**: For debugging running applications

### Speed Settings
Start with conservative settings and increase as needed:

| Frequency | Use Case | Notes |
|-----------|----------|-------|
| 480 kHz | Initial testing | Very reliable |
| 1800 kHz | Normal development | Good balance |
| 4000 kHz | Fast programming | May need adjustment |
| 8000 kHz | Maximum speed | Check signal quality |

## Common Issues and Solutions

### Connection Problems

#### "No ST-Link Detected"
**Symptoms**: CubeIDE can't find programmer
**Solutions**:
1. Check USB cable (try different cable)
2. Verify drivers installed correctly
3. Try different USB port
4. Restart CubeIDE
5. Power cycle ST-Link

#### "Cannot Connect to Target"  
**Symptoms**: ST-Link detected but can't reach STM32
**Solutions**:
1. Verify 4-wire SWD connection
2. Check target power (3.3V present)
3. Try "Connect under reset" option
4. Lower SWD frequency (480 kHz)
5. Check for solder bridges on programming pins

#### "Communication Failure"
**Symptoms**: Intermittent connection issues
**Solutions**:
1. Shorten connection wires
2. Add ground plane or twisted pairs
3. Lower SWD frequency
4. Check power supply stability
5. Remove other devices from SWD bus

### Programming Problems

#### "Flash Write Error"
**Symptoms**: Code won't upload to STM32
**Solutions**:
1. Verify correct STM32 part number in project
2. Check flash memory size limits  
3. Erase chip manually: `st-flash erase`
4. Try mass erase in debug configuration
5. Check write protection settings

#### "Verification Failed"
**Symptoms**: Programming succeeds but verification fails
**Solutions**:
1. Check power supply stability
2. Lower programming speed
3. Use external power instead of ST-Link power
4. Check for electromagnetic interference

### Debug Issues

#### "Cannot Set Breakpoint"
**Symptoms**: Breakpoints don't work or are ignored
**Solutions**:
1. Ensure code optimization is disabled (Debug build)
2. Check that code is in Flash, not RAM
3. Verify debug symbols are generated
4. Try software breakpoints if hardware ones fail

#### "Variables Not Visible"
**Symptoms**: Can't see variable values in debugger
**Solutions**:
1. Compile with debug symbols (-g flag)
2. Disable optimization (-O0)
3. Ensure variables are in scope
4. Use volatile keyword for hardware registers

## Advanced Configuration

### Virtual COM Port Setup
Some ST-Link V2 variants support virtual COM port:

1. **Install VCP drivers** (separate download from ST)
2. **Configure in device** (if supported)
3. **Use for UART** communication without separate USB-serial adapter

### Multiple ST-Link Devices
To use multiple ST-Links simultaneously:

1. **Note serial numbers**: `st-info --probe`
2. **Configure in debug settings**:
   - Add serial number to connection string
   - Use different GDB server ports
3. **Example**: Different projects can target different boards

### Custom SWD Pin Mapping
If your board uses non-standard SWD pins:

1. **Check STM32 datasheet** for alternate SWD pins
2. **Configure in CubeMX**:
   - Enable SYS → Debug Serial Wire
   - Select appropriate pins
3. **Update hardware** connections accordingly

## Performance Tips

### Faster Programming
1. **Use external power** instead of ST-Link power
2. **Increase SWD frequency** (test stability)
3. **Disable verification** (for trusted environments)
4. **Use mass erase** before programming large files

### Reliable Debugging
1. **Use adequate power supply** (stable 3.3V)
2. **Keep wires short** and use proper grounding
3. **Start with low speeds** and increase gradually
4. **Use hardware breakpoints** when possible

### Multi-Target Development
1. **Label ST-Links** with board assignments
2. **Use consistent naming** in project configurations
3. **Create scripts** for batch operations
4. **Document serial numbers** for team sharing

## Maintenance and Care

### Hardware Care
- **Handle gently** - PCB can crack if dropped
- **Protect connectors** - Use proper mating cycles
- **Store safely** - Avoid static electricity damage
- **Check cables** regularly for wear

### Software Updates
- **STM32CubeIDE** updates include ST-Link firmware updates
- **Manual updates** available from ST website
- **Check compatibility** before updating firmware

## Alternative Programming Methods

### Other ST-Link Variants
- **ST-Link V3** - Newer, faster, more features
- **Nucleo embedded ST-Link** - Use Nucleo board as programmer
- **ST-Link/V2-ISOL** - Isolated version for safety

### Third-Party Options
- **J-Link** - Segger's professional debugger
- **Black Magic Probe** - Open-source SWD debugger
- **DFU Bootloader** - Built-in USB bootloader (some STM32)

## Troubleshooting Checklist

When ST-Link isn't working, check:

- [ ] **USB cable** connected and functional
- [ ] **Drivers** installed correctly
- [ ] **4-wire SWD** connection secure
- [ ] **Target power** present (3.3V)
- [ ] **Ground connection** solid
- [ ] **Pin 1 orientation** correct
- [ ] **STM32CubeIDE** can see ST-Link
- [ ] **Project configuration** matches hardware
- [ ] **SWD frequency** not too high
- [ ] **No solder bridges** on programming pins

Following this guide should get your ST-Link V2 working reliably with your PikaDevBoard for all programming and debugging needs.