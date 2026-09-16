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
```

Then edit `settings.ini` to match your setup:

```ini
[connection]
login_url = http://192.168.8.1/html/index.html
info_url = http://192.168.8.1/html/content.html#deviceinformation
password = your_router_password_here

[runtime]
refresh_seconds = 2
headless = true
theme = light

[display]
font_size = 12
window_width = 1280
window_height = 800

[speedtest]
auto_run = false
interval_seconds = 300
```

For better privacy, you can leave the password out of `settings.ini` and set it as an environment variable before launching the app:

```powershell
$env:RSRP_MODEM_PASSWORD = "your_router_password_here"
```

## Usage

Run `RSRP-Signal-Monitor.exe`, then press **Start** to begin monitoring and **Stop** to end the session.

While aiming an external antenna, keep the app open and adjust the antenna gradually. Better signal usually means a stronger RSRP value, a higher SINR value, and an improved Connection Quality Score. Wait a few refresh cycles after each movement before comparing readings. You can also trigger a manual **Speed Test** to confirm that improved signal metrics translate to actual throughput gains.

## Development

To run the app from source:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
playwright install chromium
python main.py
```

*Note: Ensure `requirements.txt` includes `speedtest-cli` alongside `PyQt5` and `playwright`*

To build the Windows release folder:

```powershell
.\.venv\Scripts\python.exe -m PyInstaller .\RSRP_checker.spec
Copy-Item settings.example.ini .\dist\RSRP-Signal-Monitor\settings.example.ini -Force
Compress-Archive -Path .\dist\RSRP-Signal-Monitor\* -DestinationPath .\dist\rsrp-signal-monitor.zip -Force
```

The build output is created under `dist/`. Upload `dist/rsrp-signal-monitor.zip` to GitHub Releases instead of committing it to the repository.

## Source Configuration

When running from source, `settings.ini` is read from the repository folder:

```ini
[connection]
login_url = http://192.168.8.1/html/index.html
info_url = http://192.168.8.1/html/content.html#deviceinformation
password = your_router_password_here

[runtime]
refresh_seconds = 2
headless = true
theme = light

[display]
font_size = 12
window_width = 1280
window_height = 800

[speedtest]
auto_run = false
interval_seconds = 300
```

For better privacy, avoid saving the password in `settings.ini` and use an environment variable instead:

```powershell
$env:RSRP_MODEM_PASSWORD = "your_router_password_here"
```

In the app, the **Save password in local settings.ini** checkbox controls whether Save or Start writes the current router password to `settings.ini`. If the checkbox is unchecked, the app keeps the password only for the current session and removes it from `settings.ini` on the next save.

`settings.ini` is listed in `.gitignore` and should not be committed.

## Notes

The app automates the router WebUI with Playwright. If your router admin panel looks different, the app may open the page but fail to find the login or signal fields. In that case, support can be added by introducing a model-specific WebUI adapter or a Huawei HiLink API backend.

Additionally, the built-in speed test requires an active internet connection and will temporarily consume bandwidth while running. Adjust the `interval_seconds` in `[speedtest]` to prevent excessive data usage on metered connections.
