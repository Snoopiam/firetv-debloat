# 9. Reboot and verify

A reboot shows whether your changes stick. In testing, Group A always survived reboots, but **Group B was reset to enabled after a reboot at least once** (it held on later reboots).

The check works by saving the state you **want**, once, in `desired-state.txt`, and comparing every later reboot against that file by package name. A count isn't enough: one package switching on while another switches off leaves the count unchanged.

> **Important:** Only save `desired-state.txt` when the stick is exactly the way you want it (9.1), or after you deliberately change something and have verified the result (9.6). **Never** re-save it right after a reboot. That would record whatever the reboot reset as the new "desired" state and hide the problem.

> The helper commands from [step 4.0](04-snapshot.md) must be loaded in this terminal.

### 9.1 Save your desired state (first time only)
Do this once, after finishing steps 5–7.

**Windows (PowerShell):**
```powershell
Save-FireSnapshot desired-state.txt
```
**macOS / Linux:**
```bash
fire_save desired-state.txt
```
**Expected:** `Saved desired-state.txt (N disabled)`

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

### 9.4 Compare with your desired state

**Windows (PowerShell):**
```powershell
Compare-FireSnapshot desired-state.txt
```
**macOS / Linux:**
```bash
fire_compare desired-state.txt
```

| You see | Meaning | Do |
|---|---|---|
| `MATCH: the stick is exactly as saved.` | ✅ Everything survived | Done. Don't re-save anything. |
| Packages under **Switched back ON** | The reboot reset them | 9.5 |
| Packages under **Newly disabled** | Something other than you disabled them (rare) | Don't change anything. Note them and ask for help ([step 12](12-troubleshooting.md)). |
| `FAILED: ...` | The comparison couldn't run (not connected, or file missing) | Fix what it names and run 9.4 again |

### 9.5 Restore what was switched back on
Re-run the disable command for **just the packages under "Switched back ON"**, using the commands in steps 5–7. For example:
```bash
adb shell pm disable-user --user 0 com.amazon.cloud9
```
Then run **9.4 again** until it says `MATCH`. Your undo files don't need changing; they already contain these packages.

### After any later reboot
Power cuts and Fire OS updates reboot the stick without asking. Afterwards, reconnect and start at **9.3**: confirm the reboot, then compare (9.4) and restore (9.5). **Skip 9.1**, because your saved `desired-state.txt` is the evidence you're comparing against.

> **Note:** In testing, packages that came back after a reboot **stayed off while the stick was running**. The reset only happened at boot. We also noticed the resets stopped after turning off a VPN app's auto-connect on the stick, but couldn't prove that was the cause.

### 9.6 If you change your setup later
If you deliberately re-enable something (e.g. with an undo file from [step 11](11-undo.md)) or disable something new, the next comparison will flag it. Once the stick is the way you now want it, **and** you've checked it on the TV, save the new desired state:

**Windows (PowerShell):**
```powershell
Save-FireSnapshot desired-state.txt
```
**macOS / Linux:**
```bash
fire_save desired-state.txt
```

### 9.7 Final checks on the TV
- The home screen loads normally, with no blank rows
- Your streaming apps open and play
- Sponsored autoplay previews are gone (the static sponsored tiles stay; see [step 5](05-ads-and-telemetry.md))

### ✅ Check
After a confirmed reboot (uptime of a few minutes), 9.4 says `MATCH`.

---
⬅️ [8. Do not touch](08-do-not-touch.md) · ➡️ Next: [10. Free up storage](10-free-storage.md)
