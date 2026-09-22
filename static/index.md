# Atom Echo S3R Voice

ESPHome voice satellite firmware for the **M5Stack Atom Echo S3R** (ESP32-S3). On-device wake words (Alexa, Okay Nabu, Hey Jarvis), button-to-talk, and Home Assistant Assist.

## First-time installation

Connect the Echo S3R via USB-C, then use the button below to flash the factory firmware from your browser (Chrome or Edge required).

Put the device in download mode if needed: hold **RST** until the green LED lights, then release.

<script
  type="module"
  src="https://unpkg.com/esp-web-tools@10/dist/web/install-button.js?module"
></script>

<esp-web-install-button manifest="firmware/manifest.json">
  <button slot="activate">Install Atom Echo S3R Voice</button>
  <span slot="unsupported"
    >Your browser does not support WebSerial. Use Chrome or Edge on
    desktop.</span
  >
  <span slot="not-allowed">HTTPS is required (or use localhost).</span>
</esp-web-install-button>

## After flashing

**Preferred:** keep USB connected and use **Configure Wi-Fi** in the installer dialog (Improv over serial).

**Fallback (SoftAP):**

1. The device starts a Wi-Fi access point (name includes `atom-echos3r-voice` plus a MAC suffix).
2. Connect with your phone — the captive portal opens automatically (or go to http://192.168.4.1/).
3. Enter your home Wi-Fi credentials.

Then add the device in Home Assistant via **Settings → Devices & services → ESPHome**.

Set **Wake word engine location** to **On device**. Alexa is enabled by default; Okay Nabu and Hey Jarvis can be toggled on the device page.

Press the top button to start or stop a conversation (short press). Hold ~10 seconds for factory reset.

Firmware updates are offered automatically in Home Assistant when a new release is published.

## Factory reset

Hold the top button for **10 seconds**, then release. Wi-Fi credentials are cleared and the setup access point starts again.
