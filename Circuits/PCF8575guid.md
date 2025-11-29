## PCF8575 GPIO Expander Connection Details

### PCF8575 Pinout and Connections
```
PCF8575 Pin → ESP32-C6 → Function
─────────────────────────────────────────────────────────
1  (A0)   → GND        → Address bit 0 (GND for address 0x20)
2  (A1)   → GND        → Address bit 1 (GND for address 0x20)
3  (A2)   → GND        → Address bit 2 (GND for address 0x20)
4  (P0)   → Braille Dot 1 input
5  (P1)   → Braille Dot 2 input  
6  (P2)   → Braille Dot 3 input
7  (P3)   → Braille Dot 4 input
8  (P4)   → Braille Dot 5 input
9  (P5)   → Braille Dot 6 input
10 (P6)   → CTRL button input
11 (P7)   → BACKSPACE button input
12 (P8)   → SPACE_LEFT button input
13 (P9)   → SPACE_RIGHT button input
14 (P10)  → PREV button input
15 (P11)  → UP button input
16 (P12)  → SELECT button input
17 (P13)  → DOWN button input
18 (P14)  → Not used (can leave floating)
19 (P15)  → Not used (can leave floating)
20 (VCC)  → 3.3V        → Power supply
21 (GND)  → GND         → Ground
22 (SDA)  → GPIO0       → I2C Data
23 (SCL)  → GPIO1       → I2C Clock
24 (INT)  → Not used    → Interrupt (optional)
```

### PCF8575 Button Matrix Wiring
```
PCF8575 Pin → Button Type → Physical Connection
──────────────────────────────────────────────
P0-P5      → Braille Dots 1-6 → 6 tactile buttons to GND
P6         → CTRL        → Tactile button to GND
P7         → BACKSPACE   → Tactile button to GND  
P8         → SPACE_LEFT  → Tactile button to GND
P9         → SPACE_RIGHT → Tactile button to GND
P10        → PREV        → Tactile button to GND
P11        → UP          → Tactile button to GND
P12        → SELECT      → Tactile button to GND
P13        → DOWN        → Tactile button to GND
```

### PCF8575 Internal Pull-up Configuration
- All PCF8575 pins have **internal pull-up resistors enabled**
- Buttons are wired to **GND** (active LOW)
- When no button is pressed: pin reads HIGH (1)
- When button is pressed: pin reads LOW (0)

### I2C Bus Configuration
```
ESP32-C6 I2C Bus (GPIO0-SDA, GPIO1-SCL)
├── PCF8575 GPIO Expander (Address 0x20)
├── MPU6050 Accelerometer (Address 0x68) 
└── DigiSpark HID (Address 0x23)
```

### Power Supply Connections
```
3.3V Power Rail
├── ESP32-C6 3.3V pin
├── PCF8575 VCC 
├── MPU6050 VCC
└── SD Card VCC


```

### Recommended External Components
- **I2C Pull-up resistors**: 4.7kΩ from SDA to 3.3V and SCL to 3.3V

### Connection Verification
After wiring, the PCF8575 should respond to I2C address `0x20`. You can verify with an I2C scanner. The initial state should read `0xFFFF` (all pins HIGH) when no buttons are pressed.

### Physical Layout Recommendation
```
[BRAILLE INPUT AREA]
Dot1 Dot4   [CTRL] [BACKSPACE]
Dot2 Dot5   [SPACE_L] [SPACE_R]  
Dot3 Dot6   [PREV] [SELECT]
            [UP]   [DOWN]
```

This connection diagram integrates seamlessly with your existing TACTI-WAVE circuit and the provided Arduino code. The PCF8575 effectively expands the ESP32-C6's limited GPIO pins to handle all 14 input devices (6 braille dots + 8 control buttons) while sharing the I2C bus with other peripherals.
