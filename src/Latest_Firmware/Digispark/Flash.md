**Step-by-Step Guide: Uploading Code to Digispark ATTiny85**

## 1. Install Micronucleus CLI

**Download:**
- Go to the official Digispark Micronucleus releases page
- Download the Linux version (e.g., `micronucleus-cli-master-xxxxxxxx-x86_64-Linux.zip`)
- Save it to your Downloads folder

**Setup:**
```bash
cd ~/Downloads
unzip micronucleus-cli-*.zip
cd micronucleus-cli-*
chmod +x micronucleus
```

**Test Installation:**
```bash
./micronucleus --help
```
If you see usage instructions, the CLI is ready.

## 2. Prepare Your Arduino Code

**Install Digispark Board Support:**
- Open Arduino IDE
- Go to File → Preferences
- Add to Additional Board Manager URLs:
  ```
  http://digistump.com/package_digistump_index.json
  ```
- Go to Tools → Board → Board Manager
- Search for "Digistump AVR Boards" and install

**Select Correct Board:**
- Tools → Board → Digistump AVR Boards → Digispark (Default - 16.5mhz)
- Tools → Programmer → USBtinyISP

## 3. Compile Your Sketch

**Using Arduino IDE:**
- Write your code
- Click "Verify/Compile" (checkmark icon)
- Note the temporary folder where the .hex file is created

**Using Arduino CLI (Alternative):**
```bash
arduino-cli compile --fqbn digistump:avr:digispark-tiny YourSketchName.ino
```

## 4. Locate the HEX File

The compiled .hex file is typically found in:
- **Arduino IDE**: Temporary build folder (check compile output for location)
- **Arduino CLI**: `YourSketchName/build/digistump.avr.digispark-tiny/YourSketchName.ino.hex`

## 5. Upload to Digispark

**Important Timing:**
1. **Unplug** your Digispark from USB
2. Run the upload command:
   ```bash
   ./micronucleus --run path/to/your/sketch.ino.hex
   ```
3. **Immediately plug in** the Digispark when prompted
4. Wait for upload to complete

**Example:**
```bash
./micronucleus --run /home/username/Arduino/MyProject/build/digistump.avr.digispark-tiny/MyProject.ino.hex
```

## 6. Expected Output

Successful upload will show:
```
> Please plug in the device ... 
> Found USB device
> Writing: 100%
> Done.
```

## Troubleshooting Tips

**Common Issues:**
- **"Device not found"**: Plug in the Digispark faster after running command
- **Permission denied**: Run `sudo chmod a+rw /dev/ttyUSB*` (if needed)
- **Wrong board selected**: Ensure "Digispark (Default - 16.5mhz)" is selected

**Extended Timeout (if needed):**
```bash
./micronucleus --timeout 10 --run your_sketch.hex
```

**Verify Upload:**
- The onboard LED should show your program's behavior
- For a simple blink test, LED should blink according to your delay settings

## Quick Test Sketch

Create this to verify your setup works:
```cpp
// Simple blink test
void setup() {
  pinMode(1, OUTPUT); // Built-in LED on pin 1
}

void loop() {
  digitalWrite(1, HIGH);
  delay(500);
  digitalWrite(1, LOW);
  delay(500);
}
```
