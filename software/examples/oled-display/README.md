# OLED Display Example

This example demonstrates how to use the SSD1306 OLED display on the PikaDevBoard to show text, graphics, and sensor data.

## Purpose

This example shows how to:
- Initialize the SSD1306 OLED display via I2C
- Display text in different sizes and positions
- Show basic graphics (lines, rectangles, circles)
- Create a real-time sensor dashboard
- Handle display refresh and power management

## Hardware Required

- PikaDevBoard with STM32F103C8T6
- Onboard SSD1306 OLED display (128×64 pixels)
- ST-Link V2 programmer
- USB power connection

## Connections

The OLED display is already connected onboard via I2C:
- **SDA**: Connected to PB7 (I2C1_SDA)
- **SCL**: Connected to PB6 (I2C1_SCL)
- **VCC**: 3.3V power rail
- **GND**: Ground plane
- **I2C Address**: 0x3C (standard for SSD1306)

## Expected Behavior

1. Display initializes with startup message
2. Shows "PikaDevBoard" title screen
3. Cycles through different display modes:
   - Text display demo
   - Graphics demo (lines, shapes)
   - Real-time sensor dashboard
   - Button-controlled menu system

## Code Implementation

### Key Libraries Required

```c
// SSD1306 OLED driver library
#include "ssd1306.h"
#include "fonts.h"

// STM32 HAL libraries
#include "stm32f1xx_hal.h"
#include "i2c.h"
```

### Main Application Code

```c
#include "main.h"
#include "ssd1306.h"

// Function prototypes
void SystemClock_Config(void);
void MX_GPIO_Init(void);
void MX_I2C1_Init(void);
void OLED_Demo_Sequence(void);
void Display_Startup_Screen(void);
void Display_Text_Demo(void);
void Display_Graphics_Demo(void);
void Display_Sensor_Dashboard(void);

int main(void) {
    // Initialize system
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();
    MX_I2C1_Init();
    
    // Initialize OLED display
    ssd1306_Init();
    
    // Show startup screen
    Display_Startup_Screen();
    HAL_Delay(2000);
    
    // Main application loop
    while (1) {
        OLED_Demo_Sequence();
    }
}

void Display_Startup_Screen(void) {
    ssd1306_Fill(Black);
    
    // Title
    ssd1306_SetCursor(20, 10);
    ssd1306_WriteString("PikaDevBoard", Font_11x18, White);
    
    // Subtitle
    ssd1306_SetCursor(25, 35);
    ssd1306_WriteString("OLED Display", Font_7x10, White);
    
    // Version
    ssd1306_SetCursor(40, 50);
    ssd1306_WriteString("v1.0", Font_6x8, White);
    
    ssd1306_UpdateScreen();
}

void Display_Text_Demo(void) {
    ssd1306_Fill(Black);
    
    // Different font sizes
    ssd1306_SetCursor(0, 0);
    ssd1306_WriteString("Font 6x8", Font_6x8, White);
    
    ssd1306_SetCursor(0, 15);
    ssd1306_WriteString("Font 7x10", Font_7x10, White);
    
    ssd1306_SetCursor(0, 30);
    ssd1306_WriteString("Font 11x18", Font_11x18, White);
    
    // Numbers and symbols
    ssd1306_SetCursor(0, 50);
    ssd1306_WriteString("12345 !@#$%", Font_6x8, White);
    
    ssd1306_UpdateScreen();
    HAL_Delay(3000);
}

void Display_Graphics_Demo(void) {
    ssd1306_Fill(Black);
    
    // Draw rectangles
    ssd1306_DrawRectangle(10, 10, 30, 20, White);
    ssd1306_DrawFilledRectangle(40, 10, 60, 20, White);
    
    // Draw circles
    ssd1306_DrawCircle(80, 15, 8, White);
    ssd1306_DrawFilledCircle(100, 15, 8, White);
    
    // Draw lines
    ssd1306_Line(0, 35, 127, 35, White);
    ssd1306_Line(0, 40, 127, 50, White);
    ssd1306_Line(127, 55, 0, 63, White);
    
    ssd1306_UpdateScreen();
    HAL_Delay(3000);
}

void Display_Sensor_Dashboard(void) {
    char buffer[32];
    
    ssd1306_Fill(Black);
    
    // Title
    ssd1306_SetCursor(20, 0);
    ssd1306_WriteString("Sensor Data", Font_7x10, White);
    
    // Temperature (simulated - replace with actual BME680 reading)
    snprintf(buffer, sizeof(buffer), "Temp: %.1f C", 23.5);
    ssd1306_SetCursor(0, 15);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Humidity (simulated - replace with actual BME680 reading)  
    snprintf(buffer, sizeof(buffer), "Humidity: %.1f%%", 45.2);
    ssd1306_SetCursor(0, 25);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Pressure (simulated - replace with actual BME680 reading)
    snprintf(buffer, sizeof(buffer), "Press: %.0f hPa", 1013.2);
    ssd1306_SetCursor(0, 35);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Light level (simulated - replace with actual LDR reading)
    snprintf(buffer, sizeof(buffer), "Light: %d%%", 78);
    ssd1306_SetCursor(0, 45);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // System uptime
    uint32_t uptime = HAL_GetTick() / 1000;
    snprintf(buffer, sizeof(buffer), "Uptime: %lus", uptime);
    ssd1306_SetCursor(0, 55);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    ssd1306_UpdateScreen();
    HAL_Delay(1000); // Update every second
}

void OLED_Demo_Sequence(void) {
    Display_Text_Demo();
    Display_Graphics_Demo();
    
    // Show sensor dashboard for 10 seconds
    for (int i = 0; i < 10; i++) {
        Display_Sensor_Dashboard();
    }
}
```

### I2C Configuration

```c
// I2C1 initialization for OLED display
void MX_I2C1_Init(void) {
    hi2c1.Instance = I2C1;
    hi2c1.Init.ClockSpeed = 400000;        // 400kHz fast mode
    hi2c1.Init.DutyCycle = I2C_DUTYCYCLE_2;
    hi2c1.Init.OwnAddress1 = 0;
    hi2c1.Init.AddressingMode = I2C_ADDRESSINGMODE_7BIT;
    hi2c1.Init.DualAddressMode = I2C_DUALADDRESS_DISABLE;
    hi2c1.Init.OwnAddress2 = 0;
    hi2c1.Init.GeneralCallMode = I2C_GENERALCALL_DISABLE;
    hi2c1.Init.NoStretchMode = I2C_NOSTRETCH_DISABLE;
    
    if (HAL_I2C_Init(&hi2c1) != HAL_OK) {
        Error_Handler();
    }
}
```

### GPIO Configuration for I2C

```c
void HAL_I2C_MspInit(I2C_HandleTypeDef* i2cHandle) {
    GPIO_InitTypeDef GPIO_InitStruct = {0};
    
    if(i2cHandle->Instance == I2C1) {
        // Enable peripheral clocks
        __HAL_RCC_GPIOB_CLK_ENABLE();
        __HAL_RCC_I2C1_CLK_ENABLE();
        
        // Configure I2C pins
        // PB6 - SCL, PB7 - SDA
        GPIO_InitStruct.Pin = GPIO_PIN_6 | GPIO_PIN_7;
        GPIO_InitStruct.Mode = GPIO_MODE_AF_OD;
        GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_HIGH;
        HAL_GPIO_Init(GPIOB, &GPIO_InitStruct);
    }
}
```

## Advanced Features

### Menu System with Buttons

```c
typedef enum {
    MENU_MAIN,
    MENU_SENSORS,
    MENU_SETTINGS,
    MENU_ABOUT
} MenuState_t;

MenuState_t current_menu = MENU_MAIN;

void Handle_Button_Press(uint8_t button) {
    switch(button) {
        case 1: // KEY1 - Next menu
            current_menu = (current_menu + 1) % 4;
            break;
        case 2: // KEY2 - Select
            Execute_Menu_Action();
            break;
        case 3: // KEY3 - Back
            current_menu = MENU_MAIN;
            break;
        case 4: // KEY4 - Settings
            current_menu = MENU_SETTINGS;
            break;
    }
    Update_Display();
}

void Update_Display(void) {
    ssd1306_Fill(Black);
    
    switch(current_menu) {
        case MENU_MAIN:
            Display_Main_Menu();
            break;
        case MENU_SENSORS:
            Display_Sensor_Menu();
            break;
        case MENU_SETTINGS:
            Display_Settings_Menu();
            break;
        case MENU_ABOUT:
            Display_About_Menu();
            break;
    }
    
    ssd1306_UpdateScreen();
}
```

### Real-time Graph Display

```c
#define GRAPH_WIDTH 120
#define GRAPH_HEIGHT 30
#define GRAPH_X 4
#define GRAPH_Y 30

uint8_t graph_data[GRAPH_WIDTH];
uint8_t graph_index = 0;

void Add_Graph_Point(uint8_t value) {
    graph_data[graph_index] = value;
    graph_index = (graph_index + 1) % GRAPH_WIDTH;
}

void Draw_Graph(void) {
    // Clear graph area
    ssd1306_DrawRectangle(GRAPH_X-1, GRAPH_Y-1, 
                         GRAPH_X+GRAPH_WIDTH+1, GRAPH_Y+GRAPH_HEIGHT+1, White);
    
    // Draw data points
    for (int i = 0; i < GRAPH_WIDTH-1; i++) {
        int idx1 = (graph_index + i) % GRAPH_WIDTH;
        int idx2 = (graph_index + i + 1) % GRAPH_WIDTH;
        
        int y1 = GRAPH_Y + GRAPH_HEIGHT - (graph_data[idx1] * GRAPH_HEIGHT / 100);
        int y2 = GRAPH_Y + GRAPH_HEIGHT - (graph_data[idx2] * GRAPH_HEIGHT / 100);
        
        ssd1306_Line(GRAPH_X + i, y1, GRAPH_X + i + 1, y2, White);
    }
}
```

## Configuration in STM32CubeMX

1. **Enable I2C1**:
   - Mode: I2C
   - Speed: Fast Mode (400kHz)
   - Pins: PB6 (SCL), PB7 (SDA)

2. **GPIO Configuration**:
   - PB6, PB7: Alternate Function Open Drain
   - Speed: High
   - Pull-up: Enable internal pull-ups

3. **Clock Configuration**:
   - Ensure APB1 clock is configured properly
   - I2C clock derived from APB1

## Required Library Files

Add these files to your project:
- `ssd1306.h` / `ssd1306.c` - OLED driver
- `fonts.h` / `fonts.c` - Font definitions  
- Include paths for HAL I2C libraries

## Troubleshooting

### Display Not Initializing
1. **Check I2C connections** - Verify SDA/SCL pins
2. **Check I2C address** - Use I2C scanner to verify 0x3C
3. **Check power supply** - Ensure 3.3V is stable
4. **Check pull-up resistors** - Verify 4.7kΩ on SDA/SCL lines

### Display Flickering
1. **Reduce update frequency** - Don't update every loop iteration
2. **Use partial updates** - Only update changed areas
3. **Check power supply** - Ensure adequate current capability

### Garbled Display
1. **Check I2C timing** - Reduce I2C speed if needed
2. **Check for interference** - Keep wires short and shielded  
3. **Verify initialization** - Ensure proper display configuration

### Font/Graphics Issues
1. **Check font files** - Ensure fonts are included in project
2. **Verify coordinates** - Display is 128×64 pixels (0-127, 0-63)
3. **Check string length** - Ensure strings fit within display bounds

## Extensions and Improvements

### Add Touch Sensitivity
```c
// Use button inputs to simulate touch
void Handle_Touch_Input(void) {
    if (Button_Pressed(KEY1)) {
        Handle_Touch_Area(0, 0, 64, 32);    // Top left
    }
    if (Button_Pressed(KEY2)) {
        Handle_Touch_Area(64, 0, 127, 32);  // Top right  
    }
    // ... handle other areas
}
```

### Power Management
```c
void OLED_Sleep_Mode(void) {
    ssd1306_Fill(Black);
    ssd1306_UpdateScreen();
    // Put display in sleep mode to save power
    ssd1306_WriteCommand(0xAE); // Display OFF
}

void OLED_Wake_Up(void) {
    ssd1306_WriteCommand(0xAF); // Display ON
    // Restore previous display state
}
```

### Custom Graphics
```c
// Draw custom bitmap (e.g., logos, icons)
void Draw_Custom_Icon(uint8_t x, uint8_t y, const uint8_t* bitmap, uint8_t width, uint8_t height) {
    for (uint8_t i = 0; i < height; i++) {
        for (uint8_t j = 0; j < width; j++) {
            if (bitmap[i * (width/8) + j/8] & (1 << (j%8))) {
                ssd1306_DrawPixel(x + j, y + i, White);
            }
        }
    }
}
```

## Related Examples

- **`../sensor-dashboard/`** - Combine with BME680/MPU6050 readings
- **`../button-menu/`** - Advanced menu navigation system
- **`../data-logger/`** - Display logged sensor data graphs
- **`../clock-display/`** - Real-time clock with time/date display

This OLED example provides a foundation for creating rich user interfaces on the PikaDevBoard's built-in display system.