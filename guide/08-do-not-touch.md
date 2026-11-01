# 8. ⛔ Do not touch

> **Important:** Never disable anything on this page. Several of these make the stick unusable — no home screen, no remote, no Wi-Fi — and you may need a factory reset to recover (Settings → My Fire TV → Reset to Factory Defaults, which you can't reach if the home screen is gone).

| Area | Packages | Why |
|---|---|---|
| Home screen & setup | `com.amazon.tv.launcher`, `com.amazon.firehomestarter`, `com.amazon.tv.oobe` | The home screen and first-time setup |
| Settings | `com.amazon.tv.settings.v2`, `com.amazon.tv.settings.core` | Settings app — and it also handles booting and the long-press-Home menu |
| Keyboard | `com.amazon.tv.ime` | On-screen keyboard |
| App store | `com.amazon.venezia` | Appstore — breaks app installs and updates |
| Account & sync | `com.amazon.identity.*`, `com.amazon.dcp`, `com.amazon.device.sync`, `com.amazon.tcomm` | Your Amazon sign-in |
| Remote & pairing | `com.amazon.fireinputdevices`, `com.amazon.autopairservice`, `com.amazon.device.bluetoothpa`, `com.amazon.device.blepa`, `com.amazon.tv.keypolicymanager`, `com.amazon.device.bluetoothkeymaplib` | Remote control and Bluetooth |
| TV control | `com.amazon.tv.devicecontrol` | TV power/volume control over HDMI-CEC |
| Profiles | `com.amazon.ftv.profilepicker` | "Who's watching" — the home screen depends on it |
| Core system | `com.amazon.webview.chromium`, `com.mediatek.tvinput`, and **every** `com.android.*` package not listed in Group B | Android itself |
| Unidentified Amazon internals | `com.amazon.spiderpork`, `com.amazon.tigris`, `com.amazon.katoch`, `com.amazon.naatyam`, `com.amazon.diode`, `com.amazon.aria`, `com.amazon.cardinal`, `com.amazon.recess`, `com.amazon.logan`, `com.amazon.d3` and similar codenames | Purpose unknown — not worth the risk |

**Not worth touching:** `com.amazon.dummy.*` packages are empty placeholders. Disabling them saves nothing.

**Hide, don't disable:** Some system entries show up in alternative launchers (e.g. *System Tracing*, *Fire TV Player*, *Fire TV Channels*, *Fire TV Profile UI*). Hide them in the launcher instead of disabling the package.

---
⬅️ [7. Group C](07-caution-features.md) · ➡️ Next: [9. Reboot and verify](09-reboot-and-verify.md)
