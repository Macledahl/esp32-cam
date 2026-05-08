# ESP32-CAM - ESPHome Configuration

Konfiguration til AI-Thinker ESP32-CAM beregnet til brug med Home Assistant via ESPHome.

## Hardware

- ESP32-CAM (AI-Thinker)
- OV2640 kamera modul
- Onboard flash LED på GPIO4

## Installation

1. Sørg for at `secrets.yaml` findes i samme mappe med følgende:
   ```yaml
   wifi_ssid: "dit_wifi_navn"
   wifi_password: "dit_wifi_kode"
   api_encryption_key: "din_base64_nøgle"
   ap_password: "fallback_password"
   ```
2. Generer API nøgle (valgfrit, en er allerede sat i yaml filen):
   ```bash
   python3 -c "import base64, os; print(base64.b64encode(os.urandom(32)).decode())"
   ```
3. Flash enheden via ESPHome dashboard eller CLI:
   ```bash
   esphome run esp32-cam.yaml
   ```

## Funktioner

- Live kamera stream til Home Assistant
- Flashlight (tænd/sluk via Home Assistant)
- PSRAM aktiveret for bedre billedhåndtering
- Optimeret idle framerate for at spare strøm

## Licens

Se [LICENSE](LICENSE) filen.
