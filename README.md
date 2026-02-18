# Force / Pressure Sensor with Arduino (Uno R4 WiFi)

This project reads an analog pressure/force sensor on A0 of an Arduino Uno R4 WiFi and prints the raw ADC value to the serial console.

---

## Quick summary
- Board: Arduino Uno R4 WiFi (PlatformIO environment `uno_r4_wifi`)
- Framework: Arduino (PlatformIO)
- Serial baud: 9600
- Polling interval: 500 ms

---

## Using CLion
This project is configured for PlatformIO. You can use CLion as your editor/IDE and PlatformIO Core (CLI) to build/upload/monitor.

Recommended CLion version: CLion 2023.2 or newer (2023.3+ recommended).

Two ways to work in CLion:

1. Use PlatformIO plugin (if available in your JetBrains marketplace)
   - In CLion: `File > Settings > Plugins > Marketplace` and search for "PlatformIO".
   - If a plugin is available, install it and restart CLion. Open this folder as a project.

2. Use the PlatformIO Core (CLI) from CLion's integrated terminal
   - Open this project directory in CLion.
   - Open `View > Tool Windows > Terminal` and run PlatformIO (`pio`) commands described below.

Notes / assumptions:
- This README assumes you have a working Python installation and permission to install Python packages.
- If the PlatformIO plugin is not available for your CLion version, use the CLI workflow.

---

## Version information
- CLion: recommended 2023.2+ (explicitly check your install via `Help > About` in CLion)
- PlatformIO Core (CLI): install latest stable PlatformIO Core via pip (see commands below). The repo's PlatformIO environment is defined in `platformio.ini` (env: `uno_r4_wifi`).
- Arduino framework: selected in `platformio.ini` (`framework = arduino`). The exact Arduino core is resolved by PlatformIO when you build.

---

## Code
The main program is in `src/main.cpp`. It reads the analog voltage on `A0` and prints the raw ADC reading to Serial at 9600 baud every 500 ms.

Contents of `src/main.cpp`:

```cpp
#include <Arduino.h>
int value = 0;

void setup() {
    Serial.begin(9600);
    pinMode(A0, INPUT);
}

void loop() {
    value = analogRead(A0);
    Serial.println(value);
    delay(500);
}
```

What it does:
- `analogRead(A0)` reads the ADC value for the sensor connected to analog pin A0.
- The raw value (0..ADC_MAX) is printed to the Serial Monitor.
- `delay(500)` makes the loop pause for 500 ms between readings.

---

## Libraries and dependencies
This project does not use any external Arduino libraries beyond the core Arduino framework. PlatformIO will download the required board support packages automatically.

System / developer dependencies to build and upload:
- Python 3 (to install PlatformIO Core if you don't have it)
- PlatformIO Core (CLI)
- USB drivers for the Uno R4 WiFi (if required on your system)

PlatformIO environment (from `platformio.ini`):
```
[env:uno_r4_wifi]
platform = renesas-ra
board = uno_r4_wifi
framework = arduino
monitor_speed = 9600
upload_port = COM11
monitor_port = COM11
```

No additional PlatformIO libraries are declared in this project.

---

## Hardware components (what we used / recommended)
- Arduino Uno R4 WiFi (or any compatible board with an analog input and matching PlatformIO board config).
- Force Sensitive Resistor (FSR) or another analog pressure/force sensor (or a simple potentiometer for testing).
- 10 kΩ resistor (for a pull-down or to form a voltage divider with the FSR) — typical wiring uses a resistor in series to make a voltage divider.
- Breadboard and jumper wires.
- USB-A/USB-C cable to connect the board to your PC.

Typical wiring (voltage divider with an FSR):
- 5V (or 3.3V depending on board) -> one end of FSR -> other end of FSR -> A0 -> 10k resistor -> GND
- Alternatively: 5V -> 10k resistor -> A0 -> FSR -> GND
- A0 reads the divider voltage between the fixed resistor and the FSR.

Make sure to connect the board's GND to the sensor ground.

---

## Build, upload and serial monitor (PowerShell / CLion terminal)
Install PlatformIO Core (if needed):

```powershell
python -m pip install -U platformio
```

Verify PlatformIO is installed:

```powershell
pio --version
```

Build the project for the `uno_r4_wifi` environment:

```powershell
pio run -e uno_r4_wifi
```

Upload to the board (uses `upload_port` defined in `platformio.ini` — adjust if needed):

```powershell
pio run -e uno_r4_wifi -t upload
```

Open a serial monitor (using `monitor_port` & `monitor_speed` from `platformio.ini`):

```powershell
pio device monitor -p COM11 -b 9600
```

If your board appears on a different COM port, update `platformio.ini` or pass `-p COMx` to the monitor/upload commands.

---

## Troubleshooting
- If upload fails with a COM/port error: ensure the board is connected and the correct COM port is selected. Check Windows Device Manager.
- If readings look incorrect: check wiring and resistor values. Try swapping the sensor with a potentiometer to verify the ADC and serial output.
- If PlatformIO commands are not found: ensure `python -m pip install --user platformio` succeeded and that Python's Scripts folder is on your PATH.

---

## Author
- Author: (Your Name Here)

Replace the author name above with your name or project owner.

---

If you'd like, I can:
- Add a simple diagram image or ASCII wiring diagram.
- Add a minimal schematic and parts list with exact part numbers.
- Add a small section mapping analog raw values to estimated pressure (requires calibration data).

