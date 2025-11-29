# TACTI-WAVE Circuit Connection Guide

## ESP32-C6 Pin Configuration

### Power Connections
| Component | ESP32-C6 Pin | Connection Type |
|-----------|--------------|-----------------|
| 3.3V Power | 3V3 | 3.3V Output |
| Ground | GND | Common Ground |

### I2C Bus (Shared between multiple devices)
| Component | ESP32-C6 Pin | Function | Notes |
|-----------|--------------|----------|-------|
| **PCF8575 GPIO Expander** | GPIO0 | SDA | I2C Data |
| **PCF8575 GPIO Expander** | GPIO1 | SCL | I2C Clock |
| **MPU6050 Accelerometer** | GPIO0 | SDA | I2C Data (shared) |
| **MPU6050 Accelerometer** | GPIO1 | SCL | I2C Clock (shared) |
| **DigiSpark HID** | GPIO0 | SDA | I2C Data (shared) |
| **DigiSpark HID** | GPIO1 | SCL | I2C Clock (shared) |

**I2C Addresses:**
- PCF8575: `0x20`
- MPU6050: `0x68` 
- DigiSpark: `0x23`

### SPI Bus (SD Card)
| Component | ESP32-C6 Pin | Function |
|-----------|--------------|----------|
| **SD Card** | GPIO5 | CS (Chip Select) |
| **SD Card** | GPIO23 | SCK (Clock) |
| **SD Card** | GPIO21 | MISO (Master In Slave Out) |
| **SD Card** | GPIO22 | MOSI (Master Out Slave In) |

### I2S Audio (MAX98357A Amplifier)
| Component | ESP32-C6 Pin | Function |
|-----------|--------------|----------|
| **MAX98357A** | GPIO12 | BCLK (Bit Clock) |
| **MAX98357A** | GPIO13 | LRC (Left/Right Clock) |
| **MAX98357A** | GPIO15 | DIN (Data In) |

### GPIO Outputs
| Component | ESP32-C6 Pin | Function |
|-----------|--------------|----------|
| **Vibration Motor** | GPIO2 | VIB_PIN |

## PCF8575 GPIO Expander Pinout

The PCF8575 provides 16 GPIO pins for Braille input and control buttons:

### Braille Input Pins (0-5)
| Pin | Function | Braille Dot |
|-----|----------|-------------|
| 0 | Braille Dot 1 | ⠁ (Upper Left) |
| 1 | Braille Dot 2 | ⠂ (Middle Left) |
| 2 | Braille Dot 3 | ⠄ (Lower Left) |
| 3 | Braille Dot 4 | ⠈ (Upper Right) |
| 4 | Braille Dot 5 | ⠐ (Middle Right) |
| 5 | Braille Dot 6 | ⠠ (Lower Right) |

### Control Buttons (6-13)
| Pin | Function | Description |
|-----|----------|-------------|
| 6 | CTRL | Control modifier key |
| 7 | BACKSPACE | Backspace/Delete |
| 8 | SPACE_LEFT | Left space bar |
| 9 | SPACE_RIGHT | Right space bar |
| 10 | PREV | Previous/Navigation back |
| 11 | UP | Up navigation |
| 12 | SELECT | Select/Enter |
| 13 | DOWN | Down navigation |

## Component Wiring Instructions

### PCF8575 GPIO Expander
```
PCF8575 Pin → ESP32-C6
VCC → 3.3V
GND → GND
SDA → GPIO0
SCL → GPIO1
A0-A2 → GND (for address 0x20)
```

### MPU6050 Accelerometer/Gyroscope
```
MPU6050 Pin → ESP32-C6
VCC → 3.3V
GND → GND
SDA → GPIO0
SCL → GPIO1
```

### SD Card Module
```
SD Card Pin → ESP32-C6
VCC → 5V
GND → GND
CS → GPIO5
SCK → GPIO23
MISO → GPIO21
MOSI → GPIO22
```

### MAX98357A I2S Amplifier
```
MAX98357A Pin → ESP32-C6
VIN → 5V (for amplifier power)
GND → GND
BCLK → GPIO12
LRC → GPIO13
DIN → GPIO15
SD → Leave floating (shutdown pin)
GAIN → Leave floating (default gain)
```

### Vibration Motor
```
Vibration Motor → ESP32-C6
Positive → GPIO2 (via transistor if needed)
Negative → GND
```

### DigiSpark (HID Keyboard)
```
DigiSpark Pin → ESP32-C6
GND → GND
SDA → GPIO0
SCL → GPIO1
```

## Power Considerations

- **ESP32-C6**: 3.3V operation
- **PCF8575**: 3.3V or 5V compatible
- **MPU6050**: 3.3V operation  
- **SD Card**: 3.3V operation
- **MAX98357A**: 3.3V logic, 5V amplifier power
- **Vibration Motor**: Check voltage requirements (may need separate power)

## Important Notes

1. **I2C Bus**: All I2C devices share the same bus (GPIO0 and GPIO1)
2. **Pull-up Resistors**: Add 4.7kΩ pull-up resistors to SDA and SCL lines
3. **Audio Power**: MAX98357A requires 5V for proper audio amplification
4. **SD Card**: Use 3.3V logic level, don't use 5V modules
5. **Button Wiring**: All buttons use pull-up resistors (internal to PCF8575)
6. **I2C Address Conflicts**: Ensure no address conflicts between devices

## Pin Usage Summary

| GPIO | Function | Type |
|------|----------|------|
| 0 | I2C SDA | Bidirectional |
| 1 | I2C SCL | Bidirectional |
| 2 | Vibration Motor | Output |
| 5 | SD Card CS | Output |
| 12 | I2S BCLK | Output |
| 13 | I2S LRC | Output |
| 15 | I2S DIN | Output |
| 21 | SD Card MISO | Input |
| 22 | SD Card MOSI | Output |
| 23 | SD Card SCK | Output |

This configuration provides comprehensive Braille input, audio output, storage, and connectivity while maintaining proper power and signal integrity.
