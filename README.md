# Atom Echo S3R Voice

ESPHome firmware for the **M5Stack Atom Echo S3R** (ESP32-S3) as a Home Assistant voice satellite.

Extends [M5Stack’s official satellite package](https://github.com/m5stack/esphome-yaml/blob/main/common/atom-echos3r-satellite-base.yaml) with:

- **Alexa** on-device wake word (`id: alexa`) as the default, plus **Okay Nabu** and **Hey Jarvis**
- **Button-to-talk** on the top button (GPIO41): short press starts/stops Assist; hold 10s factory reset
- **Product distribution**: browser installer on GitHub Pages + HTTP OTA updates in Home Assistant

## Hardware

- M5Stack **Atom Echo S3R** / Atom VoiceS3R (ESP32-S3-PICO, 8MB flash, 8MB PSRAM)
- USB-C **data** cable for first flash

There is no ESP32-controllable RGB status LED on this board (unlike the original Atom Echo).

## End users

### Install firmware (first time)

1. Open the browser installer: [dflourusso.github.io/atom-echos3r-voice](https://dflourusso.github.io/atom-echos3r-voice/) (Chrome/Edge), **or** download the latest `*.factory.bin` from [Releases](https://github.com/dflourusso/atom-echos3r-voice/releases).
2. Connect USB-C. If the flasher cannot connect, hold **RST** until the green LED lights, then release, and try again.
3. After flashing, prefer **Configure Wi-Fi** in the installer dialog (USB still connected). SoftAP fallback: join the device AP and open http://192.168.4.1/.
4. In Home Assistant, add the device via **Settings → Devices & services → ESPHome**.
5. Set **Wake word engine location** to **On device**. Alexa is on by default; enable Okay Nabu / Hey Jarvis if you want them too.

### Button

| Action | Behavior |
|--------|----------|
| Short press | Dismiss ringing timer, or start/stop a conversation |
| Hold ~10s | Factory reset (clears Wi-Fi) |

### OTA updates

When a new version is published, Home Assistant shows a **Firmware** update on the device. Install from the device page or **Settings → Updates**.

OTA manifest: `https://dflourusso.github.io/atom-echos3r-voice/firmware/manifest.json`

## Developers

### Project layout

```
atom-echos3r-voice/
├── atom-echos3r-voice.yaml          # Core (M5Stack package + Alexa + button)
├── atom-echos3r-voice.factory.yaml  # Distribution (HTTP OTA + update entity)
├── atom-echos3r-voice.dev.yaml      # Local Wi-Fi from secrets
├── secrets.template.yaml
├── static/                          # GitHub Pages installer
└── .github/workflows/               # CI, release, Pages
```

### Local compile

**Factory image (what you ship):**

```bash
docker run --rm -v "$PWD:/config" ghcr.io/esphome/esphome:2026.6.2 compile atom-echos3r-voice.factory.yaml
```

**Dev build (your Wi-Fi credentials):**

```bash
cp secrets.template.yaml secrets.yaml
# edit secrets.yaml
docker run --rm -v "$PWD:/config" ghcr.io/esphome/esphome:2026.6.2 compile atom-echos3r-voice.dev.yaml
```

### Publish a release

Do **not** bump `esphome.project.version` in YAML. Factory builds keep `version: dev`; the release workflow replaces that with the version you type in.

1. Go to **Actions → Release Firmware → Run workflow**.
2. Enter a version (e.g. `1.0.0`) and optional release notes.
3. The workflow writes that version into the factory build, creates a GitHub Release with `.factory.bin` / `.ota.bin`, and deploys GitHub Pages with the OTA manifest (root `version` matches the release).

Always use a new semver for each release so devices see the update.

### GitHub setup (once)

- **Settings → Pages → Build and deployment**: Source = **GitHub Actions**
- Ensure Actions are enabled for the repository

### Flashing on macOS

Docker cannot pass USB serial reliably on macOS. Compile in Docker/CI, then flash via:

- The [GitHub Pages installer](https://dflourusso.github.io/atom-echos3r-voice/)
- [ESPHome Web](https://web.esphome.io) with a downloaded `.factory.bin`
