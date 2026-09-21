<p align="center">
  <img src="headerlogo.png" alt="MM3 Hub" width="240">
</p>

<h1 align="center">MM3 Hub</h1>

<p align="center">
  A lightweight Windows control center for monitoring hardware, diagnosing common system issues, and managing MM3 devices.
</p>

<p align="center">
  <img alt="Platform" src="https://img.shields.io/badge/platform-Windows%2010%20%7C%2011-2979ff">
  <img alt="Architecture" src="https://img.shields.io/badge/architecture-x64-555">
  <img alt="Electron" src="https://img.shields.io/badge/Electron-41-47848f">
  <img alt="Status" src="https://img.shields.io/badge/status-beta-c92b2b">
  <img alt="License" src="https://img.shields.io/badge/license-ISC-4caf88">
</p>

> [!IMPORTANT]
> MM3 Hub is currently in beta. Some pages, including Driver Center, are foundations for features planned in future releases.

## What is MM3 Hub?

MM3 Hub is the Windows companion application for MM3 hardware. Today it provides a local-first system dashboard, cross-vendor hardware monitoring, issue scanning, optional OpenRGB integration, and a customizable interface. Longer term, it will become the central place for configuring and supporting MM3 devices.

The project is designed around a simple rule: background work should only run when it is useful. Hardware polling pauses outside System Monitor, enhanced sensors start on demand, and optional features remain disabled until the user enables them.

## Highlights

- Live CPU, GPU, RAM, storage, and network monitoring
- CPU and GPU temperature readings through LibreHardwareMonitor
- AMD Radeon edge/core and hotspot telemetry
- Optional top-process and multi-disk views
- System overview with firmware, Windows, uptime, and hardware information
- Issue Scanner for thermal, storage, security, and hardware checks
- OpenRGB SDK integration with device selection, custom colors, and presets
- Celsius and Fahrenheit support
- Adjustable monitoring intervals for lower resource usage
- Dark and light modes
- Three structural interface styles:
  - **MM3 Studio** - balanced and polished
  - **Leaf Glass** - floating, translucent, and layered
  - **Control Deck** - dense and technical
- Animated startup sequence with local checks, rotating tips, and optional update notices
- Detached startup-preview window for safely testing animations and update states
- Portable, NSIS, and MSI build targets

## Download and installation

Prebuilt releases should be downloaded from the repository's **Releases** page.

MM3 Hub supports three Windows packages:

| Package | Best for |
| --- | --- |
| Portable EXE | Running MM3 Hub without a traditional installation |
| Setup EXE | Desktop/Start Menu shortcuts and a normal per-user installation |
| MSI | Managed or alternative Windows installation workflows |

Completely exit an older MM3 Hub instance from the system tray before replacing or launching a new portable build.

## Ryzen CPU temperature support

Windows does not expose reliable CPU temperature data through one universal API. MM3 Hub therefore includes an on-demand sensor bridge based on [LibreHardwareMonitor](https://github.com/LibreHardwareMonitor/LibreHardwareMonitor).

Some AMD Ryzen systems also require the signed [PawnIO](https://github.com/namazso/PawnIO.Setup) hardware-access driver because Windows blocks legacy low-level sensor access. If CPU temperature displays `N/A`:

1. Open **Settings → Enhanced Sensors**.
2. Select **Install CPU Sensor Support**.
3. MM3 Hub downloads PawnIO from its official GitHub release and verifies its pinned SHA-256 hash.
4. Complete the PawnIO installer.
5. Select **Retry Ryzen Sensor** and approve the Windows prompt.
6. Restart Windows if the driver installer requests it.

PawnIO is optional. MM3 Hub continues working without CPU temperature data, and nothing is installed silently.

## OpenRGB setup

RGB control uses the OpenRGB SDK and does not bundle or silently install OpenRGB.

1. Install and launch [OpenRGB](https://openrgb.org/).
2. Open **Settings → SDK Server** in OpenRGB.
3. Start the SDK server on the default port, `6742`.
4. Open **RGB & Utilities** in MM3 Hub and select **Retry**.

Device support depends on OpenRGB and the connected hardware.

## Privacy and resource usage

- No analytics or anonymous usage reporting
- No advertising or cloud hardware profiles
- No hardware data leaves the machine
- Hardware polling runs only when required
- The enhanced sensor process starts only while System Monitor is active
- Processes, storage details, and optional monitor components can be disabled
- Update checks only contact the project's GitHub Releases endpoint
- Settings and logs are stored under `%APPDATA%\MM3Hub`

## Development

### Requirements

- Windows 10 or Windows 11 x64
- Node.js 20 or newer
- npm
- The .NET Framework x64 C# compiler included with Windows at:
  `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\csc.exe`

### Install dependencies

```powershell
npm install
```

### Run in development

```powershell
npm start
```

### Validate the source

```powershell
npm run check
```

### Build packages

```powershell
# Rebuild the native sensor bridge
npm run build:sensors

# Unpacked application directory
npm run build:dir

# Portable executable
npm run build:portable

# NSIS setup executable
npm run build

# MSI package
npm run build:msi

# All configured Windows targets
npm run build:all
```

Every packaging command validates the JavaScript and rebuilds `MM3.SensorBridge.exe` before running electron-builder. Artifacts are written to `dist/`.

## Project structure

```text
MM3-Control-Center/
├─ main.js                         Electron main process and Windows integrations
├─ preload.js                      Sandboxed renderer IPC bridge
├─ renderer.js                     Main interface behavior
├─ index.html                      Application interface
├─ style.css                       Themes, layout, and animation system
├─ boot-preview.html               Detached startup preview
├─ boot-preview.js                 Preview animation controller
├─ tools/MM3.SensorBridge/         Native C# sensor bridge source
├─ resources/lhm/                  LibreHardwareMonitor runtime and notices
└─ scripts/build-sensor-bridge.ps1 Sensor bridge build script
```

The renderer runs with `nodeIntegration: false`, `contextIsolation: true`, and Electron sandboxing enabled. Renderer access to native functionality is restricted to an explicit IPC allowlist in `preload.js`.

## Current status

MM3 Hub is under active development. Features marked as beta or coming soon may change before a stable release.

Planned areas include:

- MM3 device discovery and configuration
- Driver Center implementation
- Expanded diagnostics and guided repairs
- Additional RGB and utility integrations
- Signed production builds and smoother application updates

## Support

- [MM3 website](https://shopmm3.com/)
- [Discord community](https://discord.gg/BFGeZy7yPt)
- Use GitHub Issues for reproducible bugs and feature requests

When reporting a monitoring problem, include your Windows version, CPU/GPU model, whether PawnIO is installed, and the relevant log from `%APPDATA%\MM3Hub\logs`.

## License and acknowledgements

MM3 Hub source code is licensed under the [ISC License](https://opensource.org/license/isc-license-txt/).

Bundled and optional third-party components retain their own licenses:

- [LibreHardwareMonitor](https://github.com/LibreHardwareMonitor/LibreHardwareMonitor) — MPL-2.0 and bundled third-party notices
- [OpenRGB SDK](https://github.com/OpenRGBDev/openrgb-sdk) — used for optional OpenRGB communication
- [PawnIO](https://github.com/namazso/PawnIO) — optional signed hardware-access driver
- [systeminformation](https://github.com/sebhildebrandt/systeminformation) — cross-platform system information library

LibreHardwareMonitor license material and third-party notices are included in `resources/lhm/`.

---

<p align="center">
  Built by Leaf for MM3 Studios.
</p>
