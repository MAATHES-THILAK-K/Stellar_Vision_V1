## **NETWORK & API CREDENTIALS** 📡

1. **WiFi Credentials** (Line ~694):
```cpp
const char* gemini_ssid = "XXX XXXXX";
const char* gemini_password = "XXXXXXXXXX";
```

2. **Twilio SMS Service** (Lines ~116-119):
```cpp
const char* twilio_account_sid = "ACXXXXXXXXXXXXXXXXXXXXXXXXXXXXX";
const char* twilio_auth_token = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX";
const char* twilio_from_number = "+19XXXXXXX70";
const char* twilio_to_number = "+91XXXXXXXXXX";
```

3. **API Keys**:
- **OCR.space API** (Line ~123):
  ```cpp
  String ocrApiKey = "K8XXXXXXXXXXXXX";
  ```
- **VoiceRSS TTS** (Line ~125):
  ```cpp
  #define VOICERSS_API_KEY "75XXXXXXXXXXXXXXXXXXXXXXXXXX"
  ```
- **Gemini AI** (Line ~693):
  ```cpp
  const char* gemini_api_key = "AIXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX-SMk";
  ```

## **HARDWARE CONFIGURATION** 🔧

4. **I2C Addresses**:
```cpp
#define PCF8575_ADDR 0x20      // GPIO Expander
#define DIGISPARK_ADDR 0x23    // HID Keyboard
#define MPU_ADDR 0x68          // MPU6050
```


5. **MPU6050 Calibration Offsets** (Lines ~17-22):
```cpp
#define ACCEL_X_OFFSET 0.00
#define ACCEL_Y_OFFSET 0.00
#define ACCEL_Z_OFFSET 0.00
#define GYRO_X_OFFSET 0.00
#define GYRO_Y_OFFSET 0.00
#define GYRO_Z_OFFSET 0.00
```
