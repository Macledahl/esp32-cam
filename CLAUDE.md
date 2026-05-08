# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ESPHome firmware configuration for AI-Thinker ESP32-CAM with OV2640 camera module, integrated with Home Assistant.

## Build & Deploy

```bash
esphome run esp32-cam.yaml
```

Other useful commands:
- `esphome compile esp32-cam.yaml` - compile without flashing
- `esphome upload esp32-cam.yaml` - upload to device on same network
- `esphome logs esp32-cam.yaml` - view device logs

## Configuration Structure

Single YAML file (`esp32-cam.yaml`) defines the entire firmware. ESPHome compiles this into a custom ESP32 firmware. See file for detailed comments (in Danish).

**Key sections:**
- `esp32_camera:` - Camera pins (AI-Thinker standard), 800x600 resolution, JPEG quality 10
- `psram:` - Enabled (required for image buffering)
- `wifi:` - Uses `!secret` references, includes fallback AP with captive portal
- `api:` - Home Assistant API with encryption key from secrets
- `light:` - Flashlight on GPIO4, exposed to Home Assistant

## Secrets Setup

Requires `secrets.yaml` in this directory with: `wifi_ssid`, `wifi_password`, `api_encryption_key` (base64, 32 bytes), `ap_password`.

Generate API key:
```bash
python3 -c "import base64, os; print(base64.b64encode(os.urandom(32)).decode())"
```

## Hardware Notes

- Board: `esp32cam` (NOT `esp32dev` - must use esp32cam for correct PSRAM/flash config)
- Framework: Arduino
- Camera I2C: SDA=GPIO26, SCL=GPIO27
- Camera data pins: GPIO5, 18, 19, 21, 36, 39, 34, 35
- External clock: GPIO0 @ 20MHz
- Power down pin: GPIO32 (active LOW - set LOW to enable camera)
- Reset pin: GPIO15 (required for sensor initialization)
- Flash LED: GPIO4 (controlled via Home Assistant light entity)
- PSRAM must be enabled for camera to function

## Troubleshooting

**White image + "Inactive" sensor in Home Assistant:**
- Ensure `board: esp32cam` is set (not `esp32dev`)
- Add `reset_pin: GPIO15` to `esp32_camera:` section
- The OV2640 sensor needs a reset pulse at boot to initialize correctly

**Testing camera interface:**
- Temporarily add `test_pattern: true` under `esp32_camera:` to verify the interface works (shows color bars instead of real image)
