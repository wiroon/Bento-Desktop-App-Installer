# Bento Desktop Buddy

> A quiet, ambient desktop companion for your **BentoClaw** edge-AI board.
> Provision WiFi, mirror notifications, run AI agents, and update firmware —
> all from your desk, wirelessly.

![Platforms](https://img.shields.io/badge/platforms-macOS%20%7C%20Windows%20%7C%20Linux-blue)
![License](https://img.shields.io/badge/license-MIT-green)

---

## What is this?

**Bento Desktop Buddy** is the official companion app for the **BentoClaw**
edge-AI development board. The app connects to your BentoClaw wirelessly and
turns the board into an ambient AI assistant on your desk:

- 📡 **WiFi setup** — type your WiFi name + password once on your computer;
  the board remembers it and reconnects on every boot.
- 🔔 **Notification mirror** — incoming messages, calendar events, and
  reminders from your desktop appear on the BentoClaw screen as glanceable
  cards.
- 🤖 **AI companion** — connect Buddy to your favourite AI assistant
  (OpenAI, Anthropic, DeepSeek, Ollama, …) and the board hosts a small
  on-desk character you can talk to.
- 🛠 **Firmware updates** — keep your BentoClaw on the latest firmware with
  a couple of clicks, either over USB or remotely through the BentoClaw
  web app.

---

## Install

Pick the installer for your operating system below, download it from the
[**Releases page**](https://github.com/wiroon/Bento-Desktop-App-Installer/releases/latest),
and run it.

| OS | File | How to install |
|---|---|---|
| **macOS** | `Bento.Desktop.Buddy_<version>_macOS-aarch64.dmg` | Open the `.dmg`, drag the app into your **Applications** folder. |
| **Windows** | `Bento.Desktop.Buddy_<version>_x64-setup.exe` (or `.msi`) | Run the installer; follow the wizard. |
| **Linux** | `bento-desktop-buddy_<version>_amd64.deb` / `.rpm` / `.AppImage` | `.deb` for Debian / Ubuntu, `.rpm` for Fedora / RHEL, `.AppImage` for everything else (`chmod +x` then double-click). |

### First launch — macOS

macOS will warn that the app is from an "unidentified developer" the first
time you open it. Right-click the app → **Open** → **Open** to bypass the
warning. (Apple notarisation is on our roadmap.)

### First launch — Linux

After installing the `.deb`, log out and log back in once so your user has
permission to talk to the USB debugger.

---

## Quick start

1. **Power on your BentoClaw** (USB-C from any 5 V source works).
2. **Launch Bento Desktop Buddy** from your Applications folder / Start Menu.
3. The Home screen shows a card grid — pick a feature to start.

### Pair your BentoClaw

On first launch the app walks you through pairing:

1. Pick your BentoClaw from the device list (it advertises as `Bento-XXXX`).
2. A 6-digit passkey appears on the BentoClaw screen — type it into the
   pairing dialog on your computer.
3. Done. The pairing is remembered; future launches reconnect automatically.

### Set up WiFi

1. Open **Preferences → WiFi**.
2. The card at the top is the network your computer is currently on — tap it.
3. Type the password and click **Connect**.
4. The board joins the network and reconnects automatically every time it
   powers on.

### Update firmware

1. **Local mode** — connect your BentoClaw to the computer via USB, open
   **Preferences → Firmware → Flash Firmware**, pick a firmware file, and
   click **Flash Firmware**.
2. **Remote mode** — open the BentoClaw web app, generate a pair code, and
   type it into the Remote tab in Buddy. The new firmware is delivered
   wirelessly and applied to your board.

---

## Privacy

- **Your data stays on your device.** Sensor readings, notifications, and
  chat history live in your OS user folder — they are never sent to our
  servers unless you explicitly enable a cloud AI provider.
- **API keys** for AI providers are stored in your operating system's
  built-in credential store (macOS Keychain / Windows Credential Manager /
  Linux Secret Service). The app does not transmit them anywhere except
  directly to the provider you selected.
- **Pairing** uses standard Bluetooth Secure Connections — the link is
  encrypted end-to-end.

---

## Troubleshooting

| Symptom | What to check |
|---|---|
| Can't find the board over Bluetooth | Make sure Bluetooth is on. On macOS, the first launch requires explicit permission — System Settings → Privacy & Security → Bluetooth → enable for Buddy. |
| Firmware update fails to connect | Press the small **reset** button on the BentoClaw, wait two seconds, and click Flash again. |
| Notifications don't mirror | macOS requires Accessibility + Automation permission for the app. Grant via System Settings → Privacy & Security. |
| WiFi setup says it's unauthorised | Make sure you're running the latest firmware. Use **Local mode** to install the latest firmware image, then re-pair the board. |

For other issues, please file a report at the
[**Issues page**](https://github.com/wiroon/Bento-Desktop-App-Installer/issues).
When filing, please include your OS version, the BentoClaw firmware version
(shown on the **About** card on the device's screen), and the Buddy version
from the **About** menu in the app.

---

## Support

- **Latest release:** [Releases](https://github.com/wiroon/Bento-Desktop-App-Installer/releases/latest)
- **Discussions / questions:** [Discussions tab](https://github.com/wiroon/Bento-Desktop-App-Installer/discussions)
- **Website:** <https://tesaiot.dev>
