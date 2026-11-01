# 9. Reboot and verify

A reboot shows whether your changes stick. In testing, Group A always survived reboots, but **Group B was reset to enabled after a reboot at least once** (it held on later reboots). Check yours.

### 9.1 Reboot
```bash
adb reboot
```
Wait **about 90 seconds** for the stick to boot fully. `adb reboot` returns immediately, so don't check straight away — the stick hasn't actually restarted yet. Then reconnect:
```bash
adb connect <FIRETV_IP>:5555
```
**Expected:** `connected to ...` (if `adb devices` says `offline`, wait 20 seconds and try again)

### 9.2 Confirm uptime (so you know the reboot really happened)
```bash
adb shell uptime
```
**Expected:** `up 1 min` or `up 2 min`. If it says something much longer, the reboot didn't happen — repeat 9.1.

### 9.3 Count disabled packages
```bash
adb shell pm list packages -d
```
Compare with the count you had before the reboot (41 if you did all three groups).

### 9.4 If some came back
Re-run the disable commands for just the packages missing from the list (from steps 5–7). They'll switch off again.

> **Note:** In testing, packages that came back after a reboot **did stay off while the stick was running** — the reset only happened at boot. So if it happens to you, re-run the commands whenever you've rebooted (Fire TV sticks rarely reboot on their own: power cuts and updates mostly). We also noticed the resets stopped after turning off a VPN app's auto-connect on the stick, but couldn't prove that was the cause.

### 9.5 Final checks on the TV
- Home screen loads normally, no blank rows
- Your streaming apps open and play
- Sponsored autoplay previews are gone (the static sponsored tiles stay — see [step 5](05-ads-and-telemetry.md))

### ✅ Check
`pm list packages -d` shows the same count after a confirmed reboot (uptime of a few minutes).

---
⬅️ [8. Do not touch](08-do-not-touch.md) · ➡️ Next: [10. Free up storage](10-free-storage.md)
