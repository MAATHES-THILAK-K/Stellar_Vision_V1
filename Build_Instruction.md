# Stellar Vision V1 - Build Instructions 

## Prerequisites

### Required Components
- ESP32-C6 Development Board
- Digispark ATTiny85
- PCF8575 GPIO Expander (I2C)
- MPU6050 Accelerometer/Gyro (I2C)
- microSD Card Module
- MAX98357A I2S Audio Amplifier
- 3W Speaker (4Ω)
- Vibration Motor
- 14x Tactile Switches
- Custom 3D Printed Enclosure

## Step 1: Circuit Assembly

### Wiring Diagram
```
ESP32-C6 (Main Controller)
├── I2C Bus
│   ├── PCF8575 (0x20) - GPIO Expander
│   └── MPU6050 (0x68) - Motion Sensor
├── SPI Bus
│   └── microSD Card (CS: GPIO5)
├── I2S Audio
│   └── MAX98357A 
├── GPIO
│   ├── VIB_PIN (GPIO2) - Vibration Motor
│   └── LED Indicators
└── USB
    └── Power & Programming

Digispark (HID Keyboard)
└── USB Connection (I2C Address: 0x23)
```

**As Shown in the Circuit**

## Step 2: Software Setup

### Install Arduino IDE
1. Download Arduino IDE from https://www.arduino.cc/
2. Install ESP32 Board Support:
   - Go to **File → Preferences**
   - Add to Additional Board Manager URLs:
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
   - Go to **Tools → Board → Board Manager**
   - Search for "ESP32" and install

### Required Libraries
Install these libraries via **Tools → Manage Libraries**:
```cpp
#include <Arduino.h>
#include <map>
#include <string>
#include <FS.h>
#include <SD.h>
#include <SPI.h>
#include <vector>
#include <algorithm>
#include <WiFi.h>
#include <HTTPClient.h>
#include <ArduinoJson.h>        
#include <ChronosESP32.h>       
#include <WebServer.h>
#include <WiFiClientSecure.h>
#include <Adafruit_MPU6050.h>   
#include <Adafruit_Sensor.h>
#include <AudioGeneratorWAV.h>  
#include <AudioFileSourceSD.h>
#include <AudioOutputI2S.h>
#include <Update.h>
#include <esp_sleep.h>
```

## Step 3: ESP32-C6 Firmware Setup

### Board Configuration
1. Select Board: **ESP32C6 Dev Module**
2. **Partition Scheme**: Custom CSV
3. Download partitions.csv from:
   ```
   https://github.com/MAATHES-THILAK-K/Stellar_Vision_V1/blob/main/src/Latest_Firmware/ESP32C6/partitions.csv
   ```
4. Place `partitions.csv` in your sketch folder

### Quick Partition Setup
Use online tool: https://thelastoutpostworkshop.github.io/microcontroller_devkit/esp32partitionbuilder/

Recommended partition layout:
- **App**: 2MB
- **SPIFFS**: 4
- **OTA**: 512KB

### Configuration Setup

1.follow the instruction to persionalise the code such as API keys and Wifi Credentials.
   ```
   https://github.com/MAATHES-THILAK-K/Stellar_Vision_V1/blob/main/src/configuration.md
   ```
3. Update with your credentials:
```cpp
// Network
const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";

// API Keys
#define OCR_API_KEY "YOUR_OCR_SPACE_KEY"
#define VOICERSS_API_KEY "YOUR_VOICERSS_KEY"
#define GEMINI_API_KEY "YOUR_GEMINI_AI_KEY"

// Twilio (SOS Feature)
const char* twilio_account_sid = "YOUR_TWILIO_SID";
const char* twilio_auth_token = "YOUR_TWILIO_TOKEN";
const char* twilio_to_number = "+1234567890";  // Your emergency contact
```

### Upload Firmware
1. Connect ESP32-C6 via USB
2. Select correct COM port
3. Click **Upload**
4. Wait for compilation and upload to complete

## Step 4: Digispark Setup

### Install Digispark Support
1. Add to Board Manager URLs:
   ```
   http://digistump.com/package_digistump_index.json
   ```
2. Install "Digistump AVR Boards"
3. Select Board: **Digispark (Default - 16.5mhz)**

### Upload HID Code
1. Get Digispark code from:
   ```
   https://github.com/MAATHES-THILAK-K/Stellar_Vision_V1/blob/main/src/Latest_Firmware/Digispark/Stellar_Vision_Digispark.ino
   ```
2. Follow flashing instructions:
   ```
   https://github.com/MAATHES-THILAK-K/Stellar_Vision_V1/blob/main/src/Latest_Firmware/Digispark/Flash.md
   ```

### Digispark Flashing Process
```bash
# Unplug Digispark first
./micronucleus --run Stellar_Vision_Digispark.ino.hex
# Immediately plug in Digispark when prompted
```

## Step 5: Mechanical Assembly

### 3D Printing
1. Download STL files from:
   ```
   https://github.com/MAATHES-THILAK-K/Stellar_Vision_V1/tree/main/3D%20Models/Printables
   ```
2. Print with recommended settings:
   - **Layer Height**: 0.2mm
   - **Infill**: 20%
   - **Supports**: Yes (for complex parts)
   - **Material**: PLA or ABS

### Assembly Steps
1. **Main Board Mounting**:
   - Secure ESP32-C6 with standoffs
   - Mount PCF8575 near switch matrix
   - Position MPU6050 centrally (lenght should be faced ours)

2. **Switch Matrix**:
   - Install 14 tactile switches
   - Connect to PCF8575 according to pin mapping
   - Ensure smooth key press operation

3. **Audio System**:
   - Mount MAX98357A amplifier
   - Connect 3W speaker with proper polarity

4. **Power System**:
   - Connect vibration motor
   - Add power switch if desired
   - Secure all wiring with cable ties

## Step 6: First Boot & Testing

### Initial Setup
1. Power on via USB
2. Wait for boot sequence (vibration + audio)
3. Connect to device's WiFi AP (if configured)
4. Access web interface at assigned IP

### Functionality Tests
- **Braille Input**: Test all 6 dots + modifier keys
- **Audio Output**: Verify WAV playback
- **HID Keyboard**: Confirm Digispark communication
- **SD Card**: Check file reading/writing
- **Motion Sensing**: Test tilt gestures

## Step 7: OTA Updates

### Future Updates
Your device will automatically:
- Check for updates on boot
- Notify when new firmware available
- Allow one-click OTA updates
- Preserve user data during updates

Update repository:
```
https://github.com/MAATHES-THILAK-K/Stellar_Vision_V1/tree/main/src/Latest_Firmware
```

## Troubleshooting

### Common Issues
1. **I2C Conflicts**: Check addresses (0x20, 0x68, 0x23)
2. **SD Card Not Detected**: Verify SPI pins and card format (FAT32)
3. **Audio Distortion**: Check I2S wiring and power supply
4. **HID Not Working**: Reflash Digispark with correct timing

## Safety Notes
- Ensure no short circuits in wiring

---

**🎉 Congratulations! Your Stellar Vision device is now ready to use.**

For support and updates, visit the GitHub repository:
https://github.com/MAATHES-THILAK-K/Stellar_Vision_V1

Wait for OTA notifications for future feature updates! 
