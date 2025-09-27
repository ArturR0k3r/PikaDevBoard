# UART Communication Example

This example demonstrates serial communication using the UART peripheral on STM32.

## Purpose

This example shows how to:
- Configure UART for serial communication
- Send data from STM32 to computer
- Receive data from computer to STM32
- Handle basic string processing
- Debug embedded applications using serial output

## Hardware Required

- PikaDevBoard with STM32 microcontroller
- ST-Link V2 programmer
- USB-to-serial adapter or ST-Link virtual COM port
- Computer with terminal program

## Connections

### Option 1: Using ST-Link Virtual COM Port
- No additional connections needed
- ST-Link provides virtual COM port functionality
- UART data travels through ST-Link connection

### Option 2: Using External USB-Serial Adapter
- **STM32 TX (PA2)** → **USB-Serial RX**
- **STM32 RX (PA3)** → **USB-Serial TX**  
- **GND** → **GND** (common ground)

## UART Configuration

- **Baud Rate**: 115200
- **Data Bits**: 8
- **Parity**: None
- **Stop Bits**: 1
- **Flow Control**: None

## Expected Behavior

1. STM32 sends "Hello from PikaDevBoard!" on startup
2. Sends periodic status messages every 2 seconds
3. Echoes back any received characters
4. Responds to simple commands:
   - `LED ON` - Turn on user LED
   - `LED OFF` - Turn off user LED
   - `STATUS` - Send board status information

## Code Overview

### Key Functions

```c
// Initialize UART
void MX_USART2_UART_Init(void);

// Send string via UART
void UART_SendString(char* str);

// Process received commands
void ProcessCommand(char* command);

// Main communication loop
void UART_HandleCommunication(void);
```

### Main Application Loop

```c
int main(void) {
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();
    MX_USART2_UART_Init();
    
    UART_SendString("PikaDevBoard UART Example\\r\\n");
    UART_SendString("Send commands: LED ON, LED OFF, STATUS\\r\\n");
    
    while (1) {
        UART_HandleCommunication();
        HAL_Delay(100);
    }
}
```

## Terminal Setup

### Windows - PuTTY
1. Download and install PuTTY
2. Select "Serial" connection type
3. Enter COM port (check Device Manager)
4. Set speed to 115200
5. Click "Open"

### Windows - TeraTerm
1. Install TeraTerm
2. File → New Connection → Serial
3. Select port and set baud rate to 115200
4. Click OK

### Linux/Mac - Screen
```bash
# Find the device (usually /dev/ttyUSB0 or /dev/ttyACM0)
ls /dev/tty*

# Connect using screen
screen /dev/ttyUSB0 115200

# Exit screen: Ctrl+A, then K, then Y
```

### Arduino Serial Monitor
1. Open Arduino IDE
2. Tools → Serial Monitor
3. Select correct COM port
4. Set baud rate to 115200
5. Set line ending to "Both NL & CR"

## Code Implementation

### UART Initialization
```c
static void MX_USART2_UART_Init(void) {
    huart2.Instance = USART2;
    huart2.Init.BaudRate = 115200;
    huart2.Init.WordLength = UART_WORDLENGTH_8B;
    huart2.Init.StopBits = UART_STOPBITS_1;
    huart2.Init.Parity = UART_PARITY_NONE;
    huart2.Init.Mode = UART_MODE_TX_RX;
    huart2.Init.HwFlowCtl = UART_HWCONTROL_NONE;
    
    if (HAL_UART_Init(&huart2) != HAL_OK) {
        Error_Handler();
    }
}
```

### Sending Data
```c
void UART_SendString(char* str) {
    HAL_UART_Transmit(&huart2, (uint8_t*)str, strlen(str), HAL_MAX_DELAY);
}

void UART_SendNumber(int number) {
    char buffer[16];
    sprintf(buffer, "%d\\r\\n", number);
    UART_SendString(buffer);
}
```

### Receiving Data
```c
void UART_ReceiveCommand(void) {
    uint8_t received_char;
    static char command_buffer[64];
    static uint8_t buffer_index = 0;
    
    if (HAL_UART_Receive(&huart2, &received_char, 1, 10) == HAL_OK) {
        if (received_char == '\\r' || received_char == '\\n') {
            command_buffer[buffer_index] = '\\0';
            ProcessCommand(command_buffer);
            buffer_index = 0;
        } else {
            command_buffer[buffer_index++] = received_char;
            if (buffer_index >= sizeof(command_buffer) - 1) {
                buffer_index = 0; // Reset on overflow
            }
        }
    }
}
```

## Advanced Features

### Interrupt-Based Reception
```c
// Enable UART interrupt
HAL_UART_Receive_IT(&huart2, &rx_buffer, 1);

// Interrupt callback
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart) {
    if (huart->Instance == USART2) {
        // Process received byte
        ProcessReceivedByte(rx_buffer);
        
        // Re-enable interrupt for next byte
        HAL_UART_Receive_IT(&huart2, &rx_buffer, 1);
    }
}
```

### DMA-Based Communication
```c
// Configure DMA for high-speed transfers
HAL_UART_Transmit_DMA(&huart2, tx_buffer, tx_length);
HAL_UART_Receive_DMA(&huart2, rx_buffer, rx_length);
```

## Testing Commands

Send these commands through your terminal:

```
LED ON      - Turn on user LED
LED OFF     - Turn off user LED
STATUS      - Get board information
HELP        - Show available commands
RESET       - Software reset
ADC         - Read ADC values (if ADC configured)
```

## Troubleshooting

### No Output in Terminal
1. **Check Connections**: Verify TX/RX wiring
2. **Check Baud Rate**: Ensure both sides use 115200
3. **Check COM Port**: Verify correct port selected
4. **Check Power**: Ensure board is powered
5. **Check Code**: Verify UART initialization

### Garbled Output
1. **Baud Rate Mismatch**: Check both STM32 and terminal settings
2. **Clock Issues**: Verify system clock configuration
3. **Wiring Issues**: Check for loose connections or noise
4. **Ground Connection**: Ensure common ground between devices

### Can't Send Commands
1. **RX Pin Configuration**: Check receive pin setup
2. **Terminal Settings**: Ensure correct line endings
3. **Buffer Overflow**: Check command length limits
4. **Timing Issues**: Add delays if needed

### Characters Missing or Duplicated
1. **Interrupt Conflicts**: Check for competing interrupts
2. **Timing Issues**: Verify adequate processing time
3. **Buffer Management**: Check for buffer overflows
4. **HAL Timeout**: Adjust timeout values

## Extensions and Modifications

### Add JSON Communication
```c
// Send JSON status
void SendJSONStatus(void) {
    UART_SendString("{\\r\\n");
    UART_SendString("  \\"board\\": \\"PikaDevBoard\\",\\r\\n");
    UART_SendString("  \\"firmware\\": \\"1.0\\",\\r\\n");
    UART_SendString("  \\"uptime\\": ");
    UART_SendNumber(HAL_GetTick());
    UART_SendString("\\r\\n}\\r\\n");
}
```

### Add Data Logging
```c
// Log sensor data periodically
void LogSensorData(void) {
    char log_buffer[128];
    sprintf(log_buffer, "SENSOR,%lu,%d,%d\\r\\n", 
            HAL_GetTick(), sensor1_value, sensor2_value);
    UART_SendString(log_buffer);
}
```

### Binary Protocol
```c
// Send binary data with framing
void SendBinaryPacket(uint8_t* data, uint16_t length) {
    uint8_t header[] = {0xAA, 0xBB, length >> 8, length & 0xFF};
    HAL_UART_Transmit(&huart2, header, 4, HAL_MAX_DELAY);
    HAL_UART_Transmit(&huart2, data, length, HAL_MAX_DELAY);
}
```

## Related Examples

- **`../blink/`** - Basic GPIO control (used for LED commands)
- **`../adc/`** - Add sensor readings to UART output
- **`../interrupt/`** - Implement interrupt-driven UART

## Files Included

- Complete STM32CubeIDE project
- `uart_example.ioc` - CubeMX configuration
- Command processing implementation
- Terminal setup instructions
- Protocol documentation

This example provides a foundation for many embedded projects that need PC communication for debugging, configuration, or data transfer.