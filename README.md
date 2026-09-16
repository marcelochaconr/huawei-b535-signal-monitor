# Enhanced Huawei B535 Signal Monitor

<p align="center">
  <img src="https://github.com/user-attachments/assets/cbf63acc-f139-4420-a88f-de48365dbde9" width="600"/>
</p>

An enhanced desktop signal monitor for the **Huawei 4G Router B535-232a**. While retaining the original real-time RSRP and SINR WebUI monitoring, this fork introduces built-in connection speed testing, a dynamic Connection Quality Score, and a fully customizable UI.

It is especially useful when adjusting external router antennas: start monitoring, slowly change the antenna direction or placement, and watch how RSRP, SINR, estimated speeds, and the overall Quality Score react in real time.

## Features

- 📶 **Real-time RSRP and SINR monitoring** directly from the router WebUI.
- 🚀 **Built-in Speed Testing**: Automated and manual download/upload/ping measurements with historical tracking.
- 📊 **Connection Quality Score**: A dynamic 0-100 rating based on real-time signal metrics, plus theoretical speed estimation (LTE Category).
- 🎨 **Customizable UI**: Adjustable font sizes (8px–20px), resizable split panels, and multiple window size presets (Compact, Standard, Large, Fullscreen).
- 🖥️ Modern PyQt desktop interface with metric cards, trend charts, and an event log.
- 🌓 Dark and light interface themes.
- ⚙️ Local configuration through `settings.ini` with a safe example config in `settings.example.ini`.

## Compatibility

Tested with:
- Huawei 4G Router B535-232a

This project is currently designed for Huawei B535-style WebUI pages that expose:
- Login page at `http://192.168.8.1/html/index.html`
- Password field selector `#login_password`
- Signal page at `/html/content.html#deviceinformation`
- RSRP/SINR fields compatible with `#deviceinformation_rsrp`, `#di-rsrp`, `#deviceinformation.sinr`, or `#di-sinr`

*Note: Other Huawei, SoyeaLink, or carrier-customized routers may use different admin panels. They might require a new login flow, different selectors, or a future API-based backend.*

## Installation

Download the latest Windows build from the project's **GitHub Releases** page:

1. Open **Releases** in the GitHub repository.
2. Download `rsrp-signal-monitor.zip` from the latest release.
3. Extract the archive to any folder.
4. Run `RSRP-Signal-Monitor.exe`.

The release archive includes the app and the Playwright browser files it needs, so a normal installation does not require Python, `pip`, or a separate Chromium install.

## Configuration

Create a local configuration file next to `RSRP-Signal-Monitor.exe`:

```powershell
Copy-Item settings.example.ini settings.ini
