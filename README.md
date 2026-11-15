<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/canhive-logo-dark.svg">
  <img alt="CANHIVE" src="assets/canhive-logo.svg" width="360">
</picture>

# CANHIVE

**Virtual CAN and CAN FD buses for Windows, with a bus analyzer and a command line tool.**

Create virtual CAN networks on your PC and connect your tools to them as if they were real
interfaces: BusMaster, python-can, your own C/C++, C#, Python or Rust programs, and anything that
speaks TCP. Bridge real PEAK, Kvaser and Vector interfaces into the same bus, and every program
sees the real network.

**[Download the latest version](https://github.com/RahulP5/canhive-releases/releases/latest)**

This repository holds CANHIVE's installers and release notes. CANHIVE is proprietary software,
free to download and use under its [licence agreement](EULA.txt); its source code is not public.

## Features

**Virtual buses with real timing**
- Classical CAN and CAN FD buses with any number of ports (`vcan0`, `vcan1`, …).
- Frames take their exact on-the-wire time: bit stuffing, CAN FD bit-rate switching, ID
  arbitration and bus load are modelled, and timestamps are exact.
- Cyclic messages are timed by the bus itself, not by your application, so periods stay exact
  even when the PC is busy.
- Runs entirely in user mode: no kernel driver, no driver signing, no Secure Boot changes.

**Connect any tool**

| Tool | How |
|---|---|
| BusMaster | Driver **VScom CAN-API**, device `127.0.0.1:2001` (one TCP port per CANHIVE port) |
| python-can | `interface="canhive"` (plugin included), or the built-in `slcan` and `socketcand` interfaces |
| C / C++ | `canhive.h` and `canhive.lib`: a SocketCAN-style C API |
| C#, Rust, Java, Go, LabVIEW, MATLAB, … | `canhive.dll` through the language's C interface |
| Any language with sockets | SLCAN over TCP, or socketcand (`127.0.0.1:29536`) |

The installer includes samples for C, C++, C#, Python, Rust, Node.js and PowerShell. The app's
**SDK & samples** page shows them filled in with your PC's own paths and ports.

**Real hardware**
- Bridge **PEAK (PCAN-Basic)**, **Kvaser (CANlib)** and **Vector (XL Driver Library)**
  interfaces into a virtual bus through their vendors' installed drivers.
- Several programs can use one physical interface at the same time.
- Two interfaces on one bus work as a gateway between two real networks.

**Analyzer**
- **Trace** of every frame with message names from DBC files and a filter (ID ranges,
  extended IDs, names, exclusions).
- **Messages**: one row per identifier with cycle time, min/max and jitter.
- **Signals**: decoded values (Intel/Motorola, signed, float, multiplexed) and live plots with
  zoom, pan, crosshair and CSV export.
- **Statistics**: bus load, frame rate, frames by kind, error frames, port states.
- **Record** to ASC logs, and **Replay** ASC logs with their recorded timing, faster or slower,
  once or repeated.
- **Send** single, cyclic and database messages by signal value, or generated waveforms (sine,
  triangle, sawtooth, square).

**Command line: `canhive-cli`**

Installed with the app and added to the system PATH. Text output, or one JSON object per line
with `--json` for scripts, CI and AI agents.

```powershell
canhive-cli status                                  # buses, ports, connections
canhive-cli send vcan0 123#11223344                 # one frame (can-utils notation)
canhive-cli send vcan0 123##1AABBCC -p 10 -c 100    # 100 CAN FD frames, exactly 10 ms apart
canhive-cli dump Powertrain --dbc car.dbc --id 100-1FF
canhive-cli record Powertrain drive.asc -d 60
canhive-cli replay drive.asc vcan1 --speed 2
```

Exit codes: 0 done, 1 failed, 2 wrong arguments, 3 hub not running.

## Install

1. Download `CANHIVE_<version>_x64-setup.exe` from the
   [latest release](https://github.com/RahulP5/canhive-releases/releases/latest).
2. Run it. It asks for administrator rights because it installs the CANHIVE Hub service and
   `canhive.dll` in the Windows system folders.
3. Open CANHIVE from the Start menu. A 500 kbit/s bus with two ports, `vcan0` and `vcan1`, is
   ready to use.

The installer is not code-signed yet, so Windows SmartScreen may show "Windows protected your
PC". Choose **More info → Run anyway**. Each release lists the installer's SHA-256 checksum so
you can check the file:

```powershell
Get-FileHash .\CANHIVE_0.3.0_x64-setup.exe -Algorithm SHA256
```

To uninstall, use **Settings → Apps → Installed apps → CANHIVE**.

**Requirements:** Windows 10 or 11, 64-bit. Microsoft Edge WebView2, which Windows 11 includes
(on Windows 10 the installer downloads it when missing). For real interfaces: the vendor's driver (PEAK PCAN-Basic,
Kvaser CANlib or Vector XL Driver Library).

**Upgrading:** run the new installer over the old version. Your buses and ports are kept. An
installed VirtualCAN (CANHIVE's earlier name) is removed and its configuration taken over.

## Release notes

See [CHANGELOG.md](CHANGELOG.md) and the [releases](https://github.com/RahulP5/canhive-releases/releases).

## Report a problem

Found a bug or missing a feature? [Open an issue](https://github.com/RahulP5/canhive-releases/issues/new/choose)
and choose **Bug report** or **Feature request**. The form asks for the CANHIVE version (shown in
the app under **Settings → About**, or by `canhive-cli --version`), your Windows version and, for
hardware problems, the interface and its driver version.

This repository is for downloads and issue reports only. CANHIVE's source code is not public, and
this repository does not take pull requests or other contributions.

## Licence

CANHIVE is © 2026 Rahul Pachpind, India. All rights reserved. It is licensed under the
[CANHIVE End-User Licence Agreement](EULA.txt). The open-source components it uses are listed
with their licences in [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt).

PCAN (PEAK-System Technik GmbH), Kvaser (Kvaser AB), Vector (Vector Informatik GmbH), VScom
(Vision Systems GmbH), BUSMASTER and all other product names are trademarks of their respective
owners. They are used only to identify compatible products.
