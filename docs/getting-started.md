# Getting Started with PikaDevBoard

This comprehensive guide will help you set up your PikaDevBoard development environment and create your first project.

## What You'll Need

### Hardware
- **PikaDevBoard** (assembled or kit)
- **STM32 Microcontroller** (compatible with your board version)
- **ST-Link V2** programmer/debugger
- **USB Cables** (for power and ST-Link connection)
- **Computer** (Windows, macOS, or Linux)

### Software
- **STM32CubeIDE** (free from ST Microelectronics)
- **ST-Link Drivers** (usually included with CubeIDE)
- **Terminal Program** (PuTTY, TeraTerm, or built-in terminal)

## Step 1: Hardware Setup

### 1.1 Board Assembly (if needed)
If you have a kit, follow the [Assembly Guide](../hardware/assembly/README.md) to build your board.

### 1.2 STM32 Installation
1. **Identify your STM32 part number** (e.g., STM32F401, STM32F103)
2. **Insert STM32 into socket** or **solder directly** (depending on board design)
3. **Verify orientation** - Pin 1 should align with board markings

### 1.3 Power Connection
1. **Connect USB cable** to board's USB port
2. **Verify power LED** illuminates (usually green)
3. **Check 3.3V** at test points with multimeter (optional)

### 1.4 Programming Connection
1. **Connect ST-Link V2** to programming header:
   ```
   ST-Link V2    →    PikaDevBoard
   Pin 1 (VCC)   →    Pin 1 (3.3V)
   Pin 2 (SWDIO) →    Pin 2 (SWDIO)  
   Pin 3 (GND)   →    Pin 3 (GND)
   Pin 4 (SWCLK) →    Pin 4 (SWCLK)
   ```
2. **Connect ST-Link** to computer via USB

## Step 2: Software Installation

### 2.1 Download STM32CubeIDE
1. Go to [ST's official website](https://www.st.com/en/development-tools/stm32cubeide.html)
2. **Create account** or sign in (free registration required)
3. **Download** latest version for your operating system
4. **Run installer** with administrator privileges

### 2.2 Installation Process
#### Windows
1. **Run** `en.st-stm32cubeide_[version]_[build].exe`
2. **Accept** license agreement
3. **Choose installation directory** (default is fine)
4. **Select components**:
   - ☑️ STM32CubeIDE
   - ☑️ ST-Link drivers
   - ☑️ STM32CubeMX integration
5. **Complete installation** (may take 10-15 minutes)

#### macOS
1. **Mount** the `.dmg` file
2. **Drag** STM32CubeIDE to Applications folder
3. **Install ST-Link drivers** separately if needed
4. **Grant security permissions** in System Preferences

#### Linux (Ubuntu/Debian)
```bash
# Install dependencies
sudo apt update
sudo apt install default-jre libncurses5

# Download and install
wget [download_url]
sudo dpkg -i en.st-stm32cubeide_[version].deb

# Install ST-Link tools
sudo apt install stlink-tools
```

### 2.3 First Launch Setup
1. **Launch STM32CubeIDE**
2. **Choose workspace** location (e.g., `~/STM32Workspace`)
3. **Accept** terms and conditions
4. **Skip** update notifications for now
5. **Close** welcome screen

## Step 3: Verify ST-Link Connection

### 3.1 Check Device Manager (Windows)
1. **Open Device Manager**
2. **Look for** "STMicroelectronics STLink dongle" under "Universal Serial Bus controllers"
3. **If not found**: Install ST-Link drivers manually

### 3.2 Test Connection in CubeIDE
1. **Open STM32CubeIDE**
2. **Click** "Information Center" tab
3. **Click** "ST-LINK GDB server"
4. **Should show** connected ST-Link device
5. **Note the serial number** for reference

### 3.3 Command Line Test (Optional)
```bash
# Windows (in STM32CubeIDE installation folder)
st-info --probe

# Linux/macOS
st-info --probe

# Expected output:
# Found 1 stlink programmers
# serial: [serial_number]
# hla-serial: "[serial_number]"
```

## Step 4: Create Your First Project

### 4.1 Start New Project
1. **File** → **New** → **STM32 Project**
2. **Select Board Selector** tab (or MCU Selector for specific part)
3. **Enter your STM32 part number** (e.g., STM32F401RET6)
4. **Click Next**

### 4.2 Project Configuration
1. **Project Name**: `PikaBoard_FirstProject`
2. **Location**: Use default workspace
3. **Targeted Language**: C
4. **Targeted Binary Type**: Executable
5. **Targeted Project Type**: STM32Cube
6. **Click Next**

### 4.3 Firmware Package
1. **Select firmware version** (use latest stable)
2. **Check** "Download all firmware packages"
3. **Click Next**
4. **Click Finish**

### 4.4 CubeMX Configuration Opens
1. **Pin configuration window** opens automatically
2. **We'll configure this in the next step**

## Step 5: Basic Pin Configuration

### 5.1 Configure User LED
1. In **Pinout & Configuration** tab
2. **Click on pin PA5** (or your LED pin)
3. **Select** "GPIO_Output"
4. **User Label**: Enter "USER_LED"

### 5.2 Configure User Button (Optional)
1. **Click on pin PC13** (or your button pin)  
2. **Select** "GPIO_Input"
3. **User Label**: Enter "USER_BUTTON"
4. **In GPIO settings**: Set to "GPIO_PULLUP"

### 5.3 Configure UART (Optional)
1. **Expand** "Connectivity" in Categories
2. **Click** "USART2" (or available USART)
3. **Set Mode** to "Asynchronous"
4. **Pins auto-configure** (PA2=TX, PA3=RX typically)

### 5.4 Generate Code
1. **Click** "Generate Code" (gear icon)
2. **Keep default settings**
3. **Click** "Generate"
4. **CubeMX closes**, returns to CubeIDE

## Step 6: Write Your First Program

### 6.1 Locate main.c
1. **Expand project** in Project Explorer
2. **Navigate to**: `Core` → `Src` → `main.c`
3. **Double-click** to open

### 6.2 Add LED Blink Code
Find the `/* USER CODE BEGIN WHILE */` section and add:

```c
/* USER CODE BEGIN WHILE */
while (1)
{
  /* USER CODE END WHILE */

  /* USER CODE BEGIN 3 */
  
  // Turn LED on
  HAL_GPIO_WritePin(USER_LED_GPIO_Port, USER_LED_Pin, GPIO_PIN_SET);
  HAL_Delay(500);
  
  // Turn LED off
  HAL_GPIO_WritePin(USER_LED_GPIO_Port, USER_LED_Pin, GPIO_PIN_RESET);
  HAL_Delay(500);
  
}
/* USER CODE END 3 */
```

## Step 7: Build and Flash

### 7.1 Build Project
1. **Right-click** project name in Project Explorer
2. **Select** "Build Project"
3. **Wait for compilation** (should complete without errors)
4. **Check Console** for "Build Finished" message

### 7.2 Configure Debug Settings
1. **Right-click** project → **Debug As** → **Debug Configurations**
2. **Double-click** "STM32 C/C++ Application"
3. **Verify settings**:
   - Main tab: Project and C/C++ Application should auto-populate
   - Debugger tab: Debug probe = ST-LINK (OpenOCD)
4. **Click** "Debug"

### 7.3 First Flash
1. **Perspective switch dialog** appears → Click "Yes"
2. **Code uploads** to STM32
3. **Program starts** automatically
4. **LED should blink** every 500ms!

## Step 8: Verify Operation

### 8.1 LED Behavior
- **LED blinks** on/off every 500ms
- **Consistent timing**
- **No flickering or irregular behavior**

### 8.2 Debug Features
1. **Set breakpoint** by double-clicking line number
2. **Step through code** using debug toolbar
3. **Watch variables** in Variables view
4. **Resume execution** with F8

### 8.3 Serial Output (if UART configured)
1. **Open terminal program** (PuTTY, TeraTerm)
2. **Connect to** ST-Link virtual COM port
3. **Set baud rate** to 115200
4. **Should see** debug messages (if added to code)

## Troubleshooting

### Build Errors
**"HAL_GPIO_WritePin not defined"**
- Check that HAL library is included
- Verify GPIO pin configuration in CubeMX

**"USER_LED_Pin not defined"**  
- Re-run CubeMX code generation
- Check pin label spelling

### Programming Errors
**"No ST-Link detected"**
- Check USB connections
- Verify ST-Link drivers installed
- Try different USB port

**"Cannot connect to target"**
- Check 4-wire ST-Link connection
- Verify board power (3.3V)
- Try "Connect under reset" option
- Check for solder bridges on programming header

### Runtime Issues
**LED doesn't blink**
- Verify pin configuration matches hardware
- Check LED orientation (if external)
- Add breakpoints to verify code execution
- Measure GPIO pin voltage with multimeter

**Irregular blinking**
- Check for infinite loops in code
- Verify system clock configuration
- Look for interrupt conflicts

## Next Steps

### Explore Examples
1. **UART Communication** - Add serial debugging
2. **Button Input** - Respond to user button presses  
3. **ADC Reading** - Read analog sensors
4. **PWM Output** - Control servo motors or LED brightness

### Advanced Topics
1. **Interrupts** - Handle real-time events
2. **Timers** - Precise timing control
3. **DMA** - High-speed data transfers
4. **Low Power** - Battery-powered applications

### Join the Community
- **GitHub Issues** - Report bugs and ask questions
- **Discussions** - Share projects and get help
- **Contributing** - Add your own examples

## Quick Reference

### Common GPIO Functions
```c
// Set pin HIGH
HAL_GPIO_WritePin(GPIO_Port, GPIO_Pin, GPIO_PIN_SET);

// Set pin LOW  
HAL_GPIO_WritePin(GPIO_Port, GPIO_Pin, GPIO_PIN_RESET);

// Toggle pin
HAL_GPIO_TogglePin(GPIO_Port, GPIO_Pin);

// Read pin state
GPIO_PinState state = HAL_GPIO_ReadPin(GPIO_Port, GPIO_Pin);
```

### Debug Shortcuts
- **F5** - Step Into
- **F6** - Step Over  
- **F7** - Step Return
- **F8** - Resume
- **Ctrl+Shift+B** - Build Project

Congratulations! You now have a working PikaDevBoard development setup. The LED blinking confirms that your hardware, software, and programming chain are all working correctly.