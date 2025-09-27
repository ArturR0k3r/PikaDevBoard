# Multi-Sensor Dashboard Example

This example creates a comprehensive sensor monitoring system using all the onboard sensors: BME680 (environmental), MPU6050 (motion), and LDR (light sensor).

## Purpose

This example demonstrates:
- Reading data from multiple I2C sensors simultaneously
- Environmental monitoring (temperature, humidity, pressure, air quality)
- Motion detection and orientation tracking
- Light level measurement and automatic display brightness
- Real-time data visualization on OLED display
- Data logging and trend analysis

## Hardware Required

- PikaDevBoard with STM32F103C8T6
- Onboard sensors:
  - BME680 (environmental sensor)
  - MPU6050 (6-axis motion sensor)  
  - LDR (light-dependent resistor)
  - SSD1306 OLED display (128×64)
- ST-Link V2 programmer
- USB power connection

## Sensor Specifications

### BME680 Environmental Sensor
- **I2C Address**: 0x77 (or 0x76)
- **Temperature**: -40 to +85°C (±1°C accuracy)
- **Humidity**: 0-100% RH (±3% accuracy)
- **Pressure**: 300-1100 hPa (±1 hPa accuracy)  
- **Gas Sensor**: VOC detection (air quality index)
- **Power**: 3.3V, ~2.1mA active

### MPU6050 Motion Sensor  
- **I2C Address**: 0x68
- **Accelerometer**: ±2/4/8/16g selectable range
- **Gyroscope**: ±250/500/1000/2000°/s selectable range
- **Temperature**: Internal temperature sensor
- **Power**: 3.3V, ~3.9mA active
- **Features**: Built-in Digital Motion Processor (DMP)

### LDR Light Sensor
- **Type**: GL5528 photoresistor
- **Range**: 10-1000 LUX approximately  
- **Resistance**: 10-20kΩ in light, 1MΩ+ in dark
- **Connection**: Voltage divider with 100kΩ resistor
- **ADC Channel**: Connected to STM32 ADC input

## Expected Behavior

1. **System initialization** with sensor detection and calibration
2. **Real-time dashboard** showing all sensor readings
3. **Automatic display brightness** based on ambient light
4. **Motion alerts** when movement detected
5. **Environmental warnings** for extreme conditions
6. **Data logging** with trend indicators
7. **Button navigation** between different views

## Code Implementation

### Main Application Structure

```c
#include "main.h"
#include "sensor_dashboard.h"
#include "bme680_driver.h"
#include "mpu6050_driver.h"
#include "ssd1306.h"

// Sensor data structures
typedef struct {
    float temperature;
    float humidity; 
    float pressure;
    float gas_resistance;
    uint8_t air_quality_index;
} BME680_Data_t;

typedef struct {
    int16_t accel_x, accel_y, accel_z;
    int16_t gyro_x, gyro_y, gyro_z;
    float temperature;
    float pitch, roll, yaw;
} MPU6050_Data_t;

typedef struct {
    uint16_t raw_value;
    uint8_t light_percentage;
    float lux_estimate;
} LDR_Data_t;

// Global sensor data
BME680_Data_t env_data;
MPU6050_Data_t motion_data;
LDR_Data_t light_data;

int main(void) {
    // System initialization
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();
    MX_I2C1_Init();
    MX_ADC1_Init();
    
    // Initialize display
    ssd1306_Init();
    Display_Startup_Screen();
    
    // Initialize sensors
    if (BME680_Init() != HAL_OK) {
        Display_Error("BME680 Init Failed");
        while(1);
    }
    
    if (MPU6050_Init() != HAL_OK) {
        Display_Error("MPU6050 Init Failed");  
        while(1);
    }
    
    // Calibrate sensors
    Calibrate_Sensors();
    
    // Main application loop
    uint32_t last_update = 0;
    while (1) {
        // Read sensors every 100ms
        if (HAL_GetTick() - last_update > 100) {
            Read_All_Sensors();
            Update_Dashboard();
            last_update = HAL_GetTick();
        }
        
        // Handle button inputs
        Handle_Button_Inputs();
        
        // Check for alerts
        Check_Environmental_Alerts();
        Check_Motion_Alerts();
    }
}
```

### Sensor Reading Functions

```c
void Read_All_Sensors(void) {
    // Read BME680 environmental data
    BME680_Read_Data(&env_data);
    
    // Read MPU6050 motion data
    MPU6050_Read_Data(&motion_data);
    
    // Calculate orientation
    Calculate_Orientation(&motion_data);
    
    // Read light sensor via ADC
    Read_Light_Sensor(&light_data);
    
    // Adjust display brightness based on ambient light
    Adjust_Display_Brightness(light_data.light_percentage);
}

HAL_StatusTypeDef BME680_Read_Data(BME680_Data_t *data) {
    uint8_t buffer[8];
    
    // Trigger measurement
    if (HAL_I2C_Mem_Write(&hi2c1, BME680_I2C_ADDR, BME680_REG_CTRL_MEAS, 
                         1, &ctrl_meas_value, 1, 1000) != HAL_OK) {
        return HAL_ERROR;
    }
    
    // Wait for measurement completion
    HAL_Delay(200);
    
    // Read measurement data
    if (HAL_I2C_Mem_Read(&hi2c1, BME680_I2C_ADDR, BME680_REG_TEMP_MSB, 
                        1, buffer, 8, 1000) != HAL_OK) {
        return HAL_ERROR;
    }
    
    // Convert raw data to physical values
    data->temperature = BME680_Convert_Temperature(buffer);
    data->humidity = BME680_Convert_Humidity(buffer);
    data->pressure = BME680_Convert_Pressure(buffer);
    data->gas_resistance = BME680_Convert_Gas(buffer);
    data->air_quality_index = Calculate_AQI(data->gas_resistance);
    
    return HAL_OK;
}

HAL_StatusTypeDef MPU6050_Read_Data(MPU6050_Data_t *data) {
    uint8_t buffer[14];
    
    // Read all sensor registers at once
    if (HAL_I2C_Mem_Read(&hi2c1, MPU6050_I2C_ADDR, MPU6050_REG_ACCEL_XOUT_H, 
                        1, buffer, 14, 1000) != HAL_OK) {
        return HAL_ERROR;
    }
    
    // Convert raw data
    data->accel_x = (int16_t)(buffer[0] << 8 | buffer[1]);
    data->accel_y = (int16_t)(buffer[2] << 8 | buffer[3]);
    data->accel_z = (int16_t)(buffer[4] << 8 | buffer[5]);
    
    data->gyro_x = (int16_t)(buffer[8] << 8 | buffer[9]);
    data->gyro_y = (int16_t)(buffer[10] << 8 | buffer[11]);
    data->gyro_z = (int16_t)(buffer[12] << 8 | buffer[13]);
    
    // Convert to physical units
    // Accelerometer: ±2g range, 16384 LSB/g
    // Gyroscope: ±250°/s range, 131 LSB/°/s
    
    return HAL_OK;
}

void Read_Light_Sensor(LDR_Data_t *data) {
    // Start ADC conversion
    HAL_ADC_Start(&hadc1);
    
    // Wait for conversion
    if (HAL_ADC_PollForConversion(&hadc1, 100) == HAL_OK) {
        data->raw_value = HAL_ADC_GetValue(&hadc1);
        
        // Convert to percentage (0-100%)
        data->light_percentage = (data->raw_value * 100) / 4095;
        
        // Estimate LUX value (approximate)
        data->lux_estimate = Calculate_Lux_From_ADC(data->raw_value);
    }
    
    HAL_ADC_Stop(&hadc1);
}
```

### Dashboard Display Functions

```c
void Update_Dashboard(void) {
    static uint8_t display_mode = 0;
    
    ssd1306_Fill(Black);
    
    switch(display_mode) {
        case 0:
            Display_Environmental_Data();
            break;
        case 1:
            Display_Motion_Data();
            break;
        case 2:
            Display_Light_Data();
            break;
        case 3:
            Display_Summary_View();
            break;
    }
    
    // Display mode indicator
    Display_Mode_Indicator(display_mode);
    
    ssd1306_UpdateScreen();
}

void Display_Environmental_Data(void) {
    char buffer[32];
    
    // Title
    ssd1306_SetCursor(25, 0);
    ssd1306_WriteString("Environment", Font_7x10, White);
    
    // Temperature
    snprintf(buffer, sizeof(buffer), "T: %.1f C", env_data.temperature);
    ssd1306_SetCursor(0, 15);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Humidity  
    snprintf(buffer, sizeof(buffer), "H: %.1f%%", env_data.humidity);
    ssd1306_SetCursor(70, 15);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Pressure
    snprintf(buffer, sizeof(buffer), "P: %.0f hPa", env_data.pressure);
    ssd1306_SetCursor(0, 25);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Air Quality
    snprintf(buffer, sizeof(buffer), "AQ: %d/100", env_data.air_quality_index);
    ssd1306_SetCursor(0, 35);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Gas resistance (raw)
    snprintf(buffer, sizeof(buffer), "Gas: %.0f Ohm", env_data.gas_resistance);
    ssd1306_SetCursor(0, 45);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Comfort indicator
    Display_Comfort_Level();
}

void Display_Motion_Data(void) {
    char buffer[32];
    
    // Title
    ssd1306_SetCursor(35, 0);
    ssd1306_WriteString("Motion", Font_7x10, White);
    
    // Acceleration (in g)
    float accel_x_g = motion_data.accel_x / 16384.0f;
    float accel_y_g = motion_data.accel_y / 16384.0f; 
    float accel_z_g = motion_data.accel_z / 16384.0f;
    
    snprintf(buffer, sizeof(buffer), "X:%.2fg Y:%.2fg", accel_x_g, accel_y_g);
    ssd1306_SetCursor(0, 15);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    snprintf(buffer, sizeof(buffer), "Z:%.2fg", accel_z_g);
    ssd1306_SetCursor(0, 25);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Orientation
    snprintf(buffer, sizeof(buffer), "Pitch: %.1f", motion_data.pitch);
    ssd1306_SetCursor(0, 35);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    snprintf(buffer, sizeof(buffer), "Roll: %.1f", motion_data.roll);
    ssd1306_SetCursor(0, 45);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Movement indicator
    Display_Movement_Indicator();
}

void Display_Light_Data(void) {
    char buffer[32];
    
    // Title
    ssd1306_SetCursor(40, 0);
    ssd1306_WriteString("Light", Font_7x10, White);
    
    // Light percentage
    snprintf(buffer, sizeof(buffer), "Level: %d%%", light_data.light_percentage);
    ssd1306_SetCursor(0, 15);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Estimated LUX
    snprintf(buffer, sizeof(buffer), "~%.0f LUX", light_data.lux_estimate);
    ssd1306_SetCursor(0, 25);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Raw ADC value (for debugging)
    snprintf(buffer, sizeof(buffer), "ADC: %d/4095", light_data.raw_value);
    ssd1306_SetCursor(0, 35);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Light bar graph
    Display_Light_Bar_Graph();
    
    // Auto-brightness indicator
    ssd1306_SetCursor(0, 55);
    ssd1306_WriteString("Auto Brightness: ON", Font_6x8, White);
}

void Display_Summary_View(void) {
    char buffer[20];
    
    // Compact view with all key data
    ssd1306_SetCursor(30, 0);
    ssd1306_WriteString("Summary", Font_7x10, White);
    
    // Environmental (compact)
    snprintf(buffer, sizeof(buffer), "%.1fC %.0f%%", 
             env_data.temperature, env_data.humidity);
    ssd1306_SetCursor(0, 15);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Pressure and AQI
    snprintf(buffer, sizeof(buffer), "%.0fhPa AQ:%d", 
             env_data.pressure, env_data.air_quality_index);
    ssd1306_SetCursor(0, 25);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Motion status
    float total_accel = sqrt(pow(motion_data.accel_x/16384.0f, 2) + 
                            pow(motion_data.accel_y/16384.0f, 2) + 
                            pow(motion_data.accel_z/16384.0f, 2));
    
    snprintf(buffer, sizeof(buffer), "Motion: %.2fg", total_accel);
    ssd1306_SetCursor(0, 35);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // Light level
    snprintf(buffer, sizeof(buffer), "Light: %d%%", light_data.light_percentage);
    ssd1306_SetCursor(0, 45);
    ssd1306_WriteString(buffer, Font_6x8, White);
    
    // System status
    snprintf(buffer, sizeof(buffer), "Up: %lus", HAL_GetTick()/1000);
    ssd1306_SetCursor(0, 55);
    ssd1306_WriteString(buffer, Font_6x8, White);
}
```

### Alert and Monitoring Functions

```c
void Check_Environmental_Alerts(void) {
    static uint32_t last_alert = 0;
    
    // Check for extreme conditions
    if (env_data.temperature > 35.0f || env_data.temperature < 5.0f) {
        if (HAL_GetTick() - last_alert > 5000) { // Alert every 5 seconds
            Display_Alert("Temperature Alert!");
            Buzzer_Beep(2); // 2 beeps
            last_alert = HAL_GetTick();
        }
    }
    
    if (env_data.humidity > 80.0f || env_data.humidity < 20.0f) {
        if (HAL_GetTick() - last_alert > 5000) {
            Display_Alert("Humidity Alert!");
            Buzzer_Beep(3); // 3 beeps
            last_alert = HAL_GetTick();
        }
    }
    
    if (env_data.air_quality_index < 30) { // Poor air quality
        if (HAL_GetTick() - last_alert > 10000) { // Alert every 10 seconds
            Display_Alert("Poor Air Quality!");
            Buzzer_Beep(1); // 1 beep
            last_alert = HAL_GetTick();
        }
    }
}

void Check_Motion_Alerts(void) {
    static uint32_t last_motion_time = 0;
    
    // Calculate total acceleration magnitude
    float total_accel = sqrt(pow(motion_data.accel_x/16384.0f, 2) + 
                            pow(motion_data.accel_y/16384.0f, 2) + 
                            pow(motion_data.accel_z/16384.0f, 2));
    
    // Motion detection threshold (accounting for gravity)
    if (total_accel > 1.2f || total_accel < 0.8f) {
        last_motion_time = HAL_GetTick();
        
        // Visual indicator for motion
        LED_Set(LED2, LED_ON); // Turn on motion LED
    } else {
        // Turn off motion LED after 1 second of no movement
        if (HAL_GetTick() - last_motion_time > 1000) {
            LED_Set(LED2, LED_OFF);
        }
    }
    
    // Tap detection (high acceleration spikes)
    if (total_accel > 2.0f) {
        Display_Alert("Tap Detected!");
        Buzzer_Beep(1);
    }
}
```

### Data Logging and Trends

```c
#define LOG_SIZE 60 // Store 1 minute of data (1 sample per second)

typedef struct {
    float temperature[LOG_SIZE];
    float humidity[LOG_SIZE];  
    uint8_t light_level[LOG_SIZE];
    uint8_t log_index;
} DataLog_t;

DataLog_t data_log;

void Log_Sensor_Data(void) {
    static uint32_t last_log = 0;
    
    if (HAL_GetTick() - last_log > 1000) { // Log every second
        data_log.temperature[data_log.log_index] = env_data.temperature;
        data_log.humidity[data_log.log_index] = env_data.humidity;
        data_log.light_level[data_log.log_index] = light_data.light_percentage;
        
        data_log.log_index = (data_log.log_index + 1) % LOG_SIZE;
        last_log = HAL_GetTick();
    }
}

void Display_Temperature_Trend(void) {
    // Draw simple trend graph on OLED
    for (int i = 0; i < LOG_SIZE-1; i++) {
        int x1 = i * 2;
        int x2 = (i + 1) * 2;
        int y1 = 63 - (data_log.temperature[i] - 15) * 2; // Scale for display
        int y2 = 63 - (data_log.temperature[i+1] - 15) * 2;
        
        if (y1 >= 0 && y1 <= 63 && y2 >= 0 && y2 <= 63) {
            ssd1306_Line(x1, y1, x2, y2, White);
        }
    }
}
```

## Configuration in STM32CubeMX

1. **I2C1 Configuration**:
   - Mode: I2C
   - Speed: Standard (100kHz) or Fast (400kHz)
   - Pins: PB6 (SCL), PB7 (SDA)

2. **ADC1 Configuration**:
   - Mode: Independent mode
   - Resolution: 12-bit
   - Add channel for LDR input pin

3. **GPIO Configuration**:
   - Configure button pins as inputs with pull-ups
   - Configure LED pins as outputs

## Required Libraries

- BME680 sensor library
- MPU6050 sensor library  
- SSD1306 OLED driver
- Math library for calculations

## Troubleshooting

### Sensor Communication Issues
1. **Check I2C addresses** using I2C scanner
2. **Verify pull-up resistors** on SDA/SCL lines
3. **Check power supply** stability (3.3V)
4. **Reduce I2C speed** if communication fails

### Inaccurate Readings  
1. **Calibrate sensors** after power-up
2. **Allow warm-up time** (especially BME680)
3. **Check environmental conditions** during testing
4. **Implement filtering** for noisy signals

### Performance Issues
1. **Optimize display updates** - don't update every loop
2. **Use interrupts** for button handling
3. **Implement sensor reading scheduling**
4. **Consider low-power modes** when inactive

This multi-sensor dashboard provides a comprehensive monitoring solution showcasing the full capabilities of the PikaDevBoard's sensor suite.