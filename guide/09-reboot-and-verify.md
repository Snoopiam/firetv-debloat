# 9. Reboot and verify

A reboot shows whether your changes stick. In testing, Group A always survived reboots, but **Group B was reset to enabled after a reboot at least once** (it held on later reboots). Check yours by comparing **package names** before and after. A count isn't enough: one package switching on while another switches off would leave the count unchanged.

### 9.1 Save the list before rebooting
```bash
adb shell pm list packages -d > before-reboot.txt
```
**Expected:** no output. A file `before-reboot.txt` appears.

### 9.2 Reboot
```bash
adb reboot
```
Wait **about 90 seconds** for the stick to boot fully. `adb reboot` returns immediately, so don't check straight away; the stick hasn't actually restarted yet. Then reconnect:
```bash
adb connect <FIRETV_IP>:5555
```
**Expected:** `connected to ...` (if `adb devices` says `offline`, wait 20 seconds and try again)

### 9.3 Confirm the reboot really happened
```bash
adb shell uptime
```
**Expected:** `up 1 min` or `up 2 min`. If it says something much longer, the reboot didn't happen, so repeat 9.2.

### 9.4 Compare the lists by name

**Windows (PowerShell):**
```powershell
$before = @(Get-Content before-reboot.txt | ForEach-Object { $_.Trim() } | Where-Object { $_ }); $after = @(adb shell pm list packages -d | ForEach-Object { $_.Trim() } | Where-Object { $_ }); '--- Switched back ON by the reboot:'; $before | Where-Object { $after -notcontains $_ }; '--- Newly disabled since before the reboot:'; $after | Where-Object { $before -notcontains $_ }; '--- end'
```

**macOS / Linux:**
```bash
adb shell pm list packages -d | tr -d '\r' | sort > after-reboot.txt; echo '--- Switched back ON by the reboot:'; comm -23 <(tr -d '\r' < before-reboot.txt | sort) after-reboot.txt; echo '--- Newly disabled since before the reboot:'; comm -13 <(tr -d '\r' < before-reboot.txt | sort) after-reboot.txt; echo '--- end'
```

**Expected:** the three heading lines with **nothing between them**. That means every package is in the same state as before the reboot.

| You see | Meaning | Do |
|---|---|---|
| Nothing under either heading | ✅ All your changes survived | Carry on |
| Packages under **Switched back ON** | The reboot reset them | 9.5 |
| Packages under **Newly disabled** | Something other than you disabled them (rare) | Don't change anything. Note them and ask for help ([step 12](12-troubleshooting.md)). |

### 9.5 If some were switched back on
Re-run the disable command for **just those packages**, using the commands in steps 5–7. For example:
```bash
adb shell pm disable-user --user 0 com.amazon.cloud9
```
Your undo files don't need changing; they already contain these packages.

> **Note:** In testing, packages that came back after a reboot **stayed off while the stick was running**. The reset only happened at boot. If it happens to you, repeat 9.1–9.5 whenever the stick has rebooted. Fire TV sticks rarely reboot on their own, mostly after power cuts and updates. We also noticed the resets stopped after turning off a VPN app's auto-connect on the stick, but couldn't prove that was the cause.

### 9.6 Final checks on the TV
- The home screen loads normally, with no blank rows
- Your streaming apps open and play
- Sponsored autoplay previews are gone (the static sponsored tiles stay; see [step 5](05-ads-and-telemetry.md))

### ✅ Check
After a confirmed reboot (uptime of a few minutes), 9.4 shows nothing under either heading.

---
⬅️ [8. Do not touch](08-do-not-touch.md) · ➡️ Next: [10. Free up storage](10-free-storage.md)
