# MeshCore-Signal-Hunter
 RF Detector for MeshCore Network signals


   => Please let a Star if you like it
A single-file, browser-based signal monitor for [MeshCore](https://meshcore.co.uk/) companion radios. Plug in a MeshCore node over **USB** or pair it over **Bluetooth LE**, and get a live, walk-around-friendly view of everything its radio hears — signal strength, noise floor, SNR, and every packet on the channel — right in your browser. No app store, no installer, no build step.

![status](https://img.shields.io/badge/dependencies-zero-brightgreen) ![install](https://img.shields.io/badge/install-not%20required-blue) ![platform](https://img.shields.io/badge/runs%20in-your%20browser-orange)

---

## Why a single HTML file?

This entire tool — UI, logic, USB/BLE drivers, audio, everything — is **one `.html` file**. That's a deliberate choice, not a limitation:

- **Nothing to install.** No app, no `npm install`, no Python environment, no admin/root permissions, no app-store account. Download the file, double-click it (or open it in Chrome/Edge), done.
- **Nothing to trust blindly.** There's no compiled binary and no minified bundle hiding behind a build pipeline. Every line of HTML, CSS, and JavaScript is sitting right there in the file, in plain text. Open it in Notepad, VS Code, or `less` and read exactly what it does — top to bottom, no obfuscation. That's a very different trust model from "download and run this .exe."
- **No supply chain to worry about.** There's no `node_modules` folder, no chain of third-party packages that could be silently swapped out or compromised in an update. The only external resource it loads is Google Fonts, purely for the on-screen typography — and even that's optional; the page still works fine if it can't reach the internet.
- **No server, no telemetry, no data leaving your machine.** The page talks directly to your MeshCore radio over USB (Web Serial API) or Bluetooth (Web Bluetooth API) using your browser's own sandboxed device permissions — permissions you explicitly grant per device, per session, and can revoke at any time. There is no backend. Nothing is uploaded anywhere, because there's nowhere for it to go.
- **Runs anywhere Chrome does.** The exact same file works unmodified on Windows, macOS, Linux, and Android — desktop or phone, USB or BLE.

If you've ever been wary of installing yet another random tool from GitHub, this is the opposite of that: it's one inspectable text file that only ever talks to the device you point it at.

## What it does

Connect a MeshCore companion node (over USB or BLE) and the page becomes a live RF dashboard for that radio:

- **Analog signal gauge** — an SVG needle gauge showing the strength of the most recently received packet, in dBm, on a calibrated arc.
- **Noise-floor meter** — a separate segmented, LED-style bar (green → yellow → red) showing how "loud" the RF environment is, independent from the signal gauge, so you can tell at a glance whether a weak reading is a weak signal or just a noisy channel.
- **SDR-style waterfall** — a smoothly interpolated, continuously scrolling color waterfall of signal strength over time, with a color-scale legend.
- **Live packet log** — every packet the radio hears (adverts, messages, acks, path replies, requests…), each with signal strength, SNR, hop/relay info, and a packet-type badge. Sortable by most recent or by strongest signal.
- **"Last RF signal" readout** — the most recent hit's exact RSSI, SNR, and time since received, always visible.
- **Audio alerts** — an optional sound on every received packet, with three selectable tones (a sonar-style ping, a bright alert chirp, or a snappy double-blip), independent of the mute toggle.
- **Ambient radar backdrop** — a subtle animated radar-sweep background with a "contact" blip that lights up near the gauge each time a signal comes in.
- **JSON export** — dump the current packet log to a `.json` file with one click.
- **Raw protocol log** — a togglable panel showing the underlying companion-protocol traffic, for debugging.
- **Radio settings panel** — shows the connected node's current frequency/SF/CR, with one-tap presets for common regional configs (EU 868 MHz, US 915 MHz).
- **Screen wake lock** — keeps the screen from sleeping while connected (handy for walking around with a phone).
- **Mobile-first UI** — works as a normal desktop browser tab or as a full-screen tool on a phone.

## Connecting

Two transport options, side by side:

- **USB** — via the [Web Serial API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Serial_API). Plug the companion node in, click **Connect via USB**, pick the port.
- **Bluetooth LE** — via the [Web Bluetooth API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Bluetooth_API), talking to the device's Nordic UART Service (the same BLE interface documented at [docs.meshcore.io](https://docs.meshcore.io/)). Click **Connect via Bluetooth**, pick the device.

Either way, the page speaks MeshCore's official **companion protocol** (not a raw packet sniffer) — the same protocol used by the official MeshCore mobile/desktop apps — so it gets clean, decoded packet data straight from the node's firmware.

### Requirements

- **Chrome, Edge, or another Chromium-based browser** (Arc, Brave, Opera, Comet, etc.). Web Serial and Web Bluetooth are not implemented in Firefox or Safari.
- **Bluetooth doesn't work on iOS at all** — every browser on iPhone/iPad runs on Apple's WebKit engine under the hood, which doesn't support Web Bluetooth. USB-over-serial isn't available on iOS either, for the same reason. iOS users are, unfortunately, out of luck until Apple changes that — this is a platform limitation, not something this tool can work around.
- A MeshCore companion-firmware node, connected over USB or already paired over Bluetooth.

## Usage

1. Open `index.html` (or whatever you've named the file) in a supported browser.
2. Click **Connect via USB** or **Connect via Bluetooth** and select your MeshCore node.
3. Watch the gauge, waterfall, and packet log update live as the radio hears traffic.
4. Optional: pick a signal tone, toggle sound, export the log, or open the raw protocol view from the top bar.

No configuration files, no setup wizard — the page adapts to whatever the connected node reports.

## License

Do whatever you'd like with it — it's yours to read, modify, and use.
