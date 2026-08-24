# ESP32 Dual LED - Wokwi Simulation

A PlatformIO + Wokwi project with an ESP32 Dev Kit V1 making two red LEDs, each through its own current-limiting resistor.

## Hardware

| Part | Wokwi ID | Notes |
|---|---|---|
| ESP32 DevKit V1 | `esp` | `wokwi-esp32-devkit-v1` |
| Red LED | `led1` | cathode wired to GND |
| Resistor | `r1` | in series with `led1`, feeds GPIO D2 |
| Red LED | `led2` | cathode wired to GND |
| Resistor | `r2` | the 470Ω, in series with `led2`, feeds GPIO D4 |

## Wiring

Both LEDs share a common ground and are switched independently from two GPIO pins:

- **LED1**: `GND` to LED1 cathode, LED1 anode to R1 to `D2`
- **LED2**: `GND` to LED2 cathode, LED2 anode to R2 (470Ω) to `D4`

The board's `TX0`/`RX0` pins are also wired to the Wokwi serial monitor, so `Serial.print()` output from the firmware will show up there.

## Project files

```
.
├── wokwi.toml           # this points to the Wokwi at the built firmware and the diagram
├── test/
│   └── diagram.json      # for the circuit layout which is board + LEDs + resistors + wiring
└── src/
    └── main.cpp           # this is where the logic and instructions can be seen
```

The `wokwi.toml` file tells the simulator where to find the compiled firmware for the `esp32doit-devkit-v1` PlatformIO environment.

```toml
[wokwi]
version = 1
firmware = ".pio/build/esp32doit-devkit-v1/firmware.bin"
elf = ".pio/build/esp32doit-devkit-v1/firmware.elf"
diagram = "test/diagram.json"
```

## Code

```cpp
#include <Arduino.h>

#define LED 2
#define LED2 4

void setup() {
  pinMode(LED, OUTPUT);
  pinMode(LED2, OUTPUT);
}

void loop() {
  digitalWrite(LED, HIGH);
  digitalWrite(LED2, HIGH);
  delay(500);
  digitalWrite(LED, LOW);
  digitalWrite(LED2, HIGH);
  delay(500);
}

```

## Encountered errors which leads to being careful and double checking things again are the following.

1. Install [PlatformIO](https://platformio.org/) and the [Wokwi for VS Code](https://marketplace.visualstudio.com/items?itemName=wokwi.wokwi-vscode) extension.
2. Make sure `platformio.ini` has an `esp32doit-devkit-v1` environment (matches the paths in `wokwi.toml`).
3. Build the project (`PlatformIO: Build`) so `firmware.bin` and `firmware.elf` exist under `.pio/build/esp32doit-devkit-v1/`.
4. Open `test/diagram.json` or run "Wokwi and Start Simulator".


