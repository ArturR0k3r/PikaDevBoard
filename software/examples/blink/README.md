# LED Blink Example

This is the classic "Hello World" example for embedded systems - a simple LED blink program.

## Purpose

This example demonstrates:
- Basic GPIO configuration
- HAL library usage
- Main program loop structure
- Timing with HAL_Delay()

## Hardware Required

- PikaDevBoard with STM32 microcontroller
- ST-Link V2 programmer
- Built-in LED on the board (or external LED on GPIO pin)

## Connections

- **LED**: Connected to PA5 (modify in code if different)
- No external connections required if using onboard LED

## Expected Behavior

- LED blinks on and off every 500ms
- Continuous operation
- Visual confirmation that your STM32 and programming setup works

## Code Structure

### main.c
```c
#include "main.h"

void SystemClock_Config(void);
static void MX_GPIO_Init(void);

int main(void)
{
  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();

  while (1)
  {
    HAL_GPIO_WritePin(LED_GPIO_Port, LED_Pin, GPIO_PIN_SET);
    HAL_Delay(500);
    HAL_GPIO_WritePin(LED_GPIO_Port, LED_Pin, GPIO_PIN_RESET);
    HAL_Delay(500);
  }
}
```

### main.h
```c
#ifndef __MAIN_H
#define __MAIN_H

#include "stm32f4xx_hal.h"  // Adjust for your STM32 family

// GPIO definitions
#define LED_Pin GPIO_PIN_5
#define LED_GPIO_Port GPIOA

void Error_Handler(void);

#endif /* __MAIN_H */
```

## Configuration Notes

### GPIO Setup
- Pin configured as output, push-pull
- No pull-up/pull-down required
- High speed not necessary for this example

### System Clock
- Uses default system clock configuration
- HSI (internal oscillator) typically sufficient
- Modify SystemClock_Config() for specific requirements

## Building and Flashing

1. **Import Project**:
   - File → Import → Existing Projects into Workspace
   - Browse to this directory
   - Select project and import

2. **Configure Target**:
   - Verify STM32 part number matches your hardware
   - Update pin definitions if needed

3. **Build**:
   - Project → Build Project
   - Or use Ctrl+B

4. **Flash**:
   - Run → Run As → STM32 C/C++ Application
   - Or click the green run button

## Customization

### Change Blink Rate
Modify the delay values in main():
```c
HAL_Delay(250);  // Faster blinking (250ms)
HAL_Delay(1000); // Slower blinking (1000ms)
```

### Use Different GPIO Pin
1. Update pin definitions in main.h:
```c
#define LED_Pin GPIO_PIN_6    // Use PA6 instead
#define LED_GPIO_Port GPIOA
```

2. Update GPIO initialization in stm32f4xx_hal_msp.c or main.c

### Add Multiple LEDs
```c
// In main loop:
HAL_GPIO_WritePin(LED1_GPIO_Port, LED1_Pin, GPIO_PIN_SET);
HAL_GPIO_WritePin(LED2_GPIO_Port, LED2_Pin, GPIO_PIN_RESET);
HAL_Delay(500);
HAL_GPIO_WritePin(LED1_GPIO_Port, LED1_Pin, GPIO_PIN_RESET);
HAL_GPIO_WritePin(LED2_GPIO_Port, LED2_Pin, GPIO_PIN_SET);
HAL_Delay(500);
```

## Troubleshooting

### LED Doesn't Blink
1. **Check Power**: Verify 3.3V is present on board
2. **Check Programming**: Ensure code uploaded successfully
3. **Check Pin Configuration**: Verify GPIO pin and port settings
4. **Check LED Connection**: Test LED with external power if using external LED
5. **Check Orientation**: Ensure LED polarity is correct

### Build Errors
1. **Wrong STM32 Family**: Update includes for your specific STM32 (F0, F1, F4, etc.)
2. **Missing HAL**: Ensure HAL library is included in project
3. **Pin Conflicts**: Check that pins aren't used by other peripherals

### Programming Fails
1. **ST-Link Connection**: Verify all 4 wires connected properly
2. **Power Issues**: Ensure board is powered
3. **Reset Issues**: Try connecting under reset
4. **Driver Issues**: Update ST-Link drivers if needed

## Next Steps

After getting the basic blink working:

1. **Try Button Input**: Modify to blink only when button is pressed
2. **Variable Speed**: Use potentiometer with ADC to control blink rate
3. **PWM Control**: Use PWM for LED brightness control
4. **Pattern Generation**: Create complex blinking patterns

## Related Examples

- **`../uart/`** - Add serial output for debugging
- **`../adc/`** - Read analog inputs
- **`../pwm/`** - Control LED brightness with PWM

## Files Included

- `.project` / `.cproject` - Eclipse project files
- `blink.ioc` - STM32CubeMX configuration file
- `Core/Src/main.c` - Main application code
- `Core/Inc/main.h` - Main header file
- Complete STM32CubeIDE project structure

This example serves as a foundation for more complex projects and helps verify that your development environment is working correctly.