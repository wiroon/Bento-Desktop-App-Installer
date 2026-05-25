# Bento Desktop Buddy

> A quiet, ambient desktop companion for the **BentoClaw** edge-AI development board.
> Provision WiFi, mirror notifications, run AI agents, and flash firmware — all
> from your desk, over Bluetooth.

![Platforms](https://img.shields.io/badge/platforms-macOS%20%7C%20Windows%20%7C%20Linux-blue)
![License](https://img.shields.io/badge/license-MIT-green)

---

## What is this?

**Bento Desktop Buddy** is the official companion app for **BentoClaw**, an
Edge-AI dev board built on Infineon PSoC Edge E84 (Cortex‑M55 with Helium DSP
+ Arm Ethos‑U55 NPU + Cortex‑M33) with a CYW55500 dual-band radio.

The Buddy connects to your BentoClaw over Bluetooth Low Energy (BLE NUS) and
turns the board into an ambient AI assistant on your desk:

- 📡 **WiFi provisioning** — type your SSID + password once on your Mac /
  Windows / Linux; the board persists them to on-chip non-volatile memory
  and auto-reconnects on every boot.
- 🔔 **OS notification mirroring** — incoming Slack / iMessage / Calendar
  events appear on the BentoClaw's LCD as glanceable cards.
- 🤖 **Pluggable LLM agents** — point Buddy at OpenAI / Anthropic /
  DeepSeek / Ollama and the board hosts a small AI character that can read
  sensors, control GPIO, and converse over the BLE link.
- 🛠 **Firmware updates** — flash new firmware directly from the desktop over
  USB (KitProg3) — Local mode for boards plugged into this computer, or
  Remote mode for boards paired through the TESAIoT web app.
- 🔗 **MCP (Model Context Protocol) server** — expose BentoClaw verbs as
  tools that Claude Desktop, Cursor, and other MCP-aware editors can call
  directly.

---

## Install

Pick the installer for your operating system below, download it from the
[**Releases page**](https://github.com/wiroon/Bento-Desktop-App-Installer/releases/latest),
and run it.

| OS | Installer | Notes |
|---|---|---|
| **macOS** (Apple Silicon) | `Bento Desktop Buddy_<version>_aarch64.dmg` | Open the `.dmg`, drag the app to `/Applications/`. |
| **macOS** (Intel) | included in the universal `.dmg` above | Universal binary — same file works on both. |
| **Windows 10/11** (x64) | `Bento Desktop Buddy_<version>_x64-setup.exe` (or `.msi`) | Run the installer; follow the wizard. |
| **Linux** (x86_64) | `bento-desktop-buddy_<version>_amd64.deb` / `.rpm` / `.AppImage` | `.deb` for Debian/Ubuntu, `.rpm` for Fedora/RHEL, `.AppImage` for everything else (chmod +x then run). |

### First launch (macOS)

macOS will say the app is from an "unidentified developer" the first time you
open it. Right-click the app → **Open** → **Open** to bypass the warning.
(We're working on Apple notarization for v1.6.)

### First launch (Linux)

The `.deb` ships a udev rule that gives your user permission to talk to the
KitProg3 USB debugger. After installing you may need to `logout` + `login`
once for the rule to apply.

---

## Quick start

1. **Plug in your BentoClaw** via USB (for Local firmware flashing) or
   power it from any USB-C source (for BLE-only workflows).
2. **Launch Bento Desktop Buddy** from your Applications folder / Start Menu.
3. The Home screen shows a card grid — pick a feature to start:
   - **Sensors** — live readings from the on-board sensors (IMU, magnetometer,
     pressure, humidity, 60 GHz radar, optional USB camera).
   - **Chat** — talk to the AI character running on the board. First-time
     setup asks for an LLM API key (stored locally in your OS keychain).
   - **Macros** — short scripts that fire on a single tap.
   - **Preferences → WiFi** — provision the WiFi credentials the board uses
     to talk to cloud services.
   - **Preferences → Firmware → Flash Firmware** — update the firmware
     image on the board (Local USB or Remote via the web app).

### Pair your BentoClaw

On first launch the app guides you through pairing:

1. The board advertises as `Bento-XXXX` over BLE — pick it from the list.
2. macOS / Windows shows a 6-digit passkey on the BentoClaw's screen — type
   it into the OS pairing dialog.
3. Done — the bond is stored, future launches reconnect automatically.

### Provision WiFi (DualBand variant)

1. Open **Preferences → WiFi**.
2. The card highlighted at top is the network your computer is currently
   on — tap it.
3. Type the password, click **Connect**.
4. The board joins the network, persists the credentials to on-chip NVM,
   and auto-reconnects on every reboot.

### Flash firmware

1. **Local mode** — plug the BentoClaw's KitProg3 port into your computer
   over USB. Open **Preferences → Firmware → Flash Firmware**, pick a
   `.hex` file, click **Flash Firmware**.
2. **Remote mode** — open the BentoClaw web app, generate a pair code,
   type it into Buddy's Remote tab. The web app pushes the hex over a
   TLS WebSocket to Buddy; Buddy flashes the local KitProg3.

---

## Privacy

- **Your data stays on your device.** Sensor readings, notifications, and
  chat history live in your OS user folder — never sent to TESAIoT servers
  unless you explicitly enable a cloud LLM provider.
- **LLM API keys** are stored in the OS keychain (macOS Keychain / Windows
  Credential Manager / Linux Secret Service). The app does not transmit
  them anywhere except directly to the provider you configured.
- **BLE pairing** uses Secure Connections with bonding — every NUS frame
  travels over an encrypted link tied to the LTK negotiated at pair time.
- **Firmware remote-flash** uses TLS (`wss://`) end-to-end. The pairing
  code is a one-time token consumed at session start.

---

## Hardware

Bento Desktop Buddy is designed for the **BentoClaw** development board
family:

- `KIT_PSE84_AI` — primary AI Kit (BMI270 IMU + BMM350 magnetometer +
  DPS368 pressure + SHT40 humidity + USB camera + 60 GHz radar).
- `KIT_PSE84_EVAL_EPC2` — Evaluation kit (CapSense + potentiometer for
  UI experiments).

Both kits ship with three firmware variants — the Buddy works with all
three. The DualBand-TinyPython variant in particular is what enables the
WiFi provisioning + cloud-MQTT story.

> Don't have a board yet? Visit **<https://tesaiot.dev>** for ordering
> information and the developer documentation portal.

---

## Troubleshooting

| Symptom | What to check |
|---|---|
| Can't find the board over BLE | Make sure Bluetooth is on in your OS settings. On macOS the first launch requires explicit permission — System Settings → Privacy & Security → Bluetooth → enable for Buddy. |
| WiFi provision says `not_permitted` | Firmware older than v1.1.0 had this bug. Update to the latest firmware (Local Flash Firmware screen, latest `app_combined.hex`). |
| Local flash fails with "cannot read IDR" | Press the **XRES** button on the BentoClaw and click Flash again. The previously-running firmware may have locked the SWD port. |
| Notifications don't mirror | macOS requires Accessibility + Automation permission for the Buddy bundle. Grant via System Settings → Privacy & Security. |

For other issues, please file a report at the [Issues
page](https://github.com/wiroon/Bento-Desktop-App-Installer/issues) — please
include your OS version, BentoClaw firmware version (shown on the LCD
**About** card), and the Buddy version from the **About** menu.

---

## Releases & support

- **Latest release:** see [Releases](https://github.com/wiroon/Bento-Desktop-App-Installer/releases/latest)
- **Discussions / questions:** [Discussions tab](https://github.com/wiroon/Bento-Desktop-App-Installer/discussions)
- **Contact the team:** sriborrirux@gmail.com

---

## About TESAIoT

TESAIoT (Thai Embedded Systems & AIoT) is a research group at Burapha
University focused on ambient AI hardware that meets you where you
already are — at your desk, in your home, on the workbench.

Bento Desktop Buddy is part of the TESAIoT product family for ambient
edge-AI hardware. For more about the team and the BentoClaw board,
visit **<https://tesaiot.dev>**.
