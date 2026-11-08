# ToolLogPro

A Qt 6 desktop tool for Android developers and testers: stream and analyse `adb logcat` / kernel logs, inspect and change device settings, run `dumpsys`, manage Datalogic SDK configuration properties and cradles, and control one or many devices from one window.

The left sidebar has four tabs: **Logcat**, **Android**, **SDK** and **Devices**.

## Demo

- Full walkthrough: `demo/ToolLogPro_demo_full.mp4`
- One clip per feature: `demo/clips/01_overview.mp4` … `11_settings_theme.mp4`
- Voice-over script with timestamps: `demo/NARRATION.md`

## Features

### General
- **Device selector**: lists online and remembered offline devices; add a device by serial, forget offline ones. A dot shows the connection state.
- **Status bar**: encoding, line count, memory use, connected devices, monitor state, and a one-click **Light / Dark** theme toggle.
- **Settings dialog** (gear icon): log/output font, visible log columns, reset filter history, theme (Dark / Light / Auto).
- **Keyboard shortcuts**: `F1` lists them all (copy rows with `Ctrl+C`, filter history with `Up/Down`, …).

### Logcat tab
- **Live capture**: `Logcat` streams `adb logcat`; `Kernel` streams `dmesg -w` (needs root). If the device is offline, the capture waits and starts as soon as it comes online.
- **Follow reboots**: keeps capturing across reboots/disconnects and marks each gap in the log.
- **Open / save logs**: load threadtime or brief log files, save the buffer to a file.
- **Filter query**: words search Tag and Message. Supports `a | b` (OR), `-word` (exclude), `"exact phrase"`, `tag:` / `msg:` / `pkg:` / `pid:` / `tid:` fields, `tag=` exact match and `( )` groups. Hover the box for the syntax.
- **Quick filters**: right-click a Tag, Package, PID or TID cell to filter by it or exclude it; `▾ App` filters by an app running on the device.
- **Minimum level** (V/D/I/W/E/A) and **time range** (`HH:MM:SS[.mmm]`).
- **Highlight**: colour matching words and jump to the next/previous match.
- **Marked lines**: double-click rows to mark them; the Marked panel shows the time delta (ΔT) between marks, and any mark can be set as the start time. Click a mark to jump back to it; save marked lines to a file.
- **Crash / ANR detection**: `FATAL EXCEPTION`, native crashes (`Fatal signal`, tombstones) and `ANR in …` are marked automatically; `⚠ Crashes` jumps between them.
- **Split view**: a second pane (B) with its own data and filters, e.g. a live capture next to a saved file. `⇄ Sync` applies filter changes to both panes.
- **Table tools**: choose columns, auto-scroll, fit rows, view full cell content, copy rows.

### Android tab
- **Settings table**: all `system`, `secure` and `global` settings, with a filter (`wifi value=1 ns:secure`) and in-place editing written back to the device.
- **System properties table**: all `getprop` values, filterable and editable.
- **Live monitor**: re-fetches a table every N ms and highlights values that change.
- **Dumpsys**: run any service (with autocomplete) or one-click presets (`meminfo`, `battery`, `activity`, …); find in output with a results list; **Snapshot + Diff** to see what changed; save output; run any raw `adb` command.

### SDK tab (Datalogic devices)
- **Configuration properties** (`cmd configuration_manager`): list every property with value, default, type and notes; filter by name/type/value; show only changed, staged, writable, read-only, needs-reboot or unavailable properties.
- **Safe editing**: changes are staged until `Apply` (or applied immediately if chosen); discard, reset to default, reboot prompt when needed.
- **Property sets**: export/import JSON, save/load named sets in a local database.
- **Cradle**: query cradle info, firmware and update schedule; update cradle firmware.

### Devices tab
- **Devices manager**: tick one or more devices to act on them together.
- **Dashboard**: manufacturer, model, Android/SDK version, build, security patch, kernel, ABI, battery, IP and network state.
- **Quick actions**: reboot (normal / bootloader / fastboot / sideload), volume, power key, ADB root/unroot, ADB over Wi-Fi.
- **Wi-Fi connect** and **firmware flashing** (runs the firmware folder's `download.sh`) on all selected devices.
- **Device configuration**: toggle settings (stay awake, Wi-Fi, Bluetooth, airplane mode, NFC, dark mode, animations, show touches, …), preview as JSON, then **Deploy** to the selected devices. Export/import JSON and save/load presets.

## Requirements

- Qt 6.5+ (Core, Widgets, Concurrent, Sql) and CMake 3.19+
- `adb` on the `PATH`

## Build

```sh
cmake -S . -B build -DCMAKE_PREFIX_PATH=<path-to-Qt6>
cmake --build build --target ToolLogPro
```

A Linux AppImage build script is in `deploy/deploy.sh`.
