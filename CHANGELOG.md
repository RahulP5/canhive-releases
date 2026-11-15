# Changelog

## 0.3.0 (2026-11-15)

### Analyzer
The Monitor page becomes the **Analyzer**, built on the hub's exact bus timestamps.

- **Trace** with message names from DBC files, time since start or time of day, hex or decimal,
  and a filter: ID ranges, `x` for extended IDs, prefixes, names, exclusions.
- **Messages**: one row per identifier with cycle time average, min, max and jitter. Cycles that
  differ from the database's are flagged.
- **Signals**: decoded values of the selected frame; watched signals with min/max and a live
  plot (one lane per signal or one shared axis, 2 s to 5 min, freeze, zoom, pan, crosshair,
  CSV export). Bus load and frame rate can be plotted too.
- **Statistics**: bus load and frame rate (now, average, peak), frames by kind, error frames,
  port states.
- **DBC databases**: Intel and Motorola, signed, float and multiplexed signals. Import errors
  name their line and column; unsupported parts are skipped with a warning.
- **Record** the bus to an ASC log with bus timestamps.

### Send panel
Everything that puts frames on the bus is in one panel, with a Running list of what is active.
- Single and cyclic frames.
- Database messages by signal value, with range checks.
- Generated signals: sine, triangle, sawtooth, square, on the hub's cyclic schedule.
- **Replay** of ASC logs with their recorded timing: faster or slower, a part of the log, Rx/Tx or
  channels only, once or repeated. The hub releases each frame exactly when it is due, so the
  timing does not depend on the PC's load.

### Command line tool
`canhive-cli.exe` is installed with the app, and its folder is added to the system PATH (removed
again on uninstall). Commands: `status`, `send` (one frame, a counted series at an exact period,
or cyclic), `dump` (with DBC decoding and ID filter), `record`, `replay`, `decode`, `dbc`. Text,
or one JSON object per line with `--json`. Exit codes 0 done, 1 failed, 2 wrong arguments,
3 hub not running.

### Other changes
- A cyclic message keeps its schedule when only its data changes.
- The side panel can be collapsed to icons (Ctrl+B); hovering an icon shows its name. The
  Analyzer uses the full window width.
- Error messages say what happened and what to do: lost events, closed connections, error frames
  with their likely cause.

## 0.2.0 (2026-11-15)

VirtualCAN is now **CANHIVE**, with a new name, logo, app icon and installer.

- The installer removes an installed VirtualCAN first. Buses and ports are kept: the
  configuration is copied from `%ProgramData%\VirtualCAN` to `%ProgramData%\CANHIVE`.
- Programs that use the SDK need the new names; the functions, structures and error codes are
  otherwise the same:

| VirtualCAN 0.1 | CANHIVE 0.2 |
|---|---|
| `vcan.dll`, `vcan.lib` | `canhive.dll`, `canhive.lib` |
| `virtualcan.h`: `vcan_open()`, `VCAN_*` | `canhive.h`: `canhive_open()`, `CANHIVE_*` |
| `python-can-virtualcan`, `interface="virtualcan"` | `python-can-canhive`, `interface="canhive"` |
| Service `VirtualCANHub` | Service `CanhiveHub` ("CANHIVE Hub") |
| Environment variables `VIRTUALCAN_*` | `CANHIVE_*` |

- Port names (`vcan0`, `vcan1`, …), the SLCAN ports (2001, 2002, …) and socketcand (29536) are
  unchanged. Tools that connect over TCP need no change.

## 0.1 (2026-10-02)

First version, as VirtualCAN: virtual CAN and CAN FD buses, SLCAN over TCP, socketcand, the C API
and python-can plugin, and bridges to PEAK, Kvaser and Vector interfaces.
