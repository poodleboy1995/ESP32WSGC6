# ESP32WSGC6 — ESP32-S3 6-Channel Relay Controller

Firmware and web UI for the **Waveshare ESP32-S3-Relay-6CH** board, set up to run
grow-room style devices (light, water, pump, spray, and two aux outputs) from a
built-in web interface — with timed runs, automatic light schedules, and
sensor-driven safety logic for the pump and water.

## Repository layout

| Path | What it is |
|------|------------|
| [`ESP32-S3-Relay-Controller/`](ESP32-S3-Relay-Controller/) | The Arduino sketch and firmware |
| [`ESP32-S3-Relay-Controller/ESP32-S3-Relay-Controller.ino`](ESP32-S3-Relay-Controller/ESP32-S3-Relay-Controller.ino) | Main sketch (`setup()` / `loop()`) |
| [`ESP32-S3-Relay-Controller/config.h`](ESP32-S3-Relay-Controller/config.h) | Pin map, WiFi credentials, timing, light/timer state |
| [`ESP32-S3-Relay-Controller/helpers.h`](ESP32-S3-Relay-Controller/helpers.h) | Relay read/write + control logic |
| [`ESP32-S3-Relay-Controller/web_server.h`](ESP32-S3-Relay-Controller/web_server.h) | HTTP endpoints + the on-device web UI |
| [`ESP32-S3-Relay-Controller/README.md`](ESP32-S3-Relay-Controller/README.md) | **Full documentation** (setup, endpoints, safety behavior) |
| [`preview.html`](preview.html) | Standalone browser preview/simulator of the UI (not flashed to the device) |

## Quick start

1. Open [`ESP32-S3-Relay-Controller/config.h`](ESP32-S3-Relay-Controller/config.h) and set your WiFi `ssid` / `password`.
2. Build and flash to the ESP32-S3 board.
3. Open the Serial Monitor at **115200 baud** and note the printed IP address.
4. Open that IP in a browser to reach the control panel.

For the complete guide — channel map, HTTP endpoints, and the pump/water safety
logic — see the **[full README in `ESP32-S3-Relay-Controller/`](ESP32-S3-Relay-Controller/README.md)**.

## Channels

| Channel | GPIO | Role |
|---------|------|------|
| CH1 | GPIO1 | Light |
| CH2 | GPIO2 | Water |
| CH3 | GPIO41 | Pump |
| CH4 | GPIO42 | Spray |
| CH5 | GPIO45 | AUX 1 |
| CH6 | GPIO46 | AUX 2 |

## Sensor / control inputs

| Pin | Role |
|-----|------|
| GPIO4 | Water **LOW** switch — reservoir low → stop pump, refill (latch water ON) |
| GPIO5 | Water **HIGH** switch — tank full → water off (release latch) |
| GPIO40 | Water manual **OFF** override (highest priority) |
| GPIO39 | Water manual **ON** override |

## Preview / simulator

[`preview.html`](preview.html) is a self-contained page that mimics the web UI and
simulates the firmware logic (debounce, pump auto-stop, water latch, manual-override
priority). Open it in any browser — or with VS Code's Live Server — to click through
the controls and the interactive pin breakout diagram without any hardware. It is a
test tool only and is **not** served by the device.
