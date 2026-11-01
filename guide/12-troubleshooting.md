# 12. Troubleshooting & lessons learned

### Errors and what they mean
| You see | Meaning | Do |
|---|---|---|
| `new state: disabled-user` | Worked | Nothing |
| `Security exception: Cannot disable a protected package` | Fire OS protects this package | Skip it. It can't be disabled without root. |
| `Unknown package` / `IllegalArgumentException` | Not installed on your stick | Skip it |
| `device offline` | Stick is booting or the connection dropped | Wait 20 s, `adb connect <FIRETV_IP>:5555` |
| `unauthorized` | The TV prompt wasn't approved | Approve "Allow USB debugging?" on the TV |
| `more than one device/emulator` | Several devices connected | Add `-s <FIRETV_IP>:5555` after `adb` |

### Lessons from the original session

**1. Don't use `pm uninstall` on Amazon apps.**
`pm uninstall -k --user 0 <package>` (the usual Android debloat command) fails on Fire OS 7.6 with `DELETE_FAILED_INTERNAL_ERROR`. Worse, `pm uninstall <package>` and `cmd package uninstall-system-updates <package>` on an app you've already disabled **fail and switch the app back on**. Stick to `pm disable-user`.

**2. Some changes can be reset at boot.**
See [step 9](09-reboot-and-verify.md). Group A always held; Group B reset once. After any reboot, compare against your saved `desired-state.txt` (9.4) and re-disable only what switched back on. Don't re-save that file after a reboot.

**3. Home-screen ads can't be removed by debloating.**
The sponsored tiles are drawn by Amazon's home screen from server data. The only real fix is using a different launcher, and on Fire OS 7.6 that's limited:
- Third-party launchers install and run fine as apps (e.g. Projectivy Launcher).
- Making the **Home button** open them is mostly blocked: Fire OS doesn't start third-party accessibility services (the usual "override Home" method), Launcher Manager is blacklisted, and app-button remappers are blocked.
- What works: open your launcher from the app row, or pin it as the first app tile so it's one click from Home.

**4. Disabled apps still use storage.**
Their files live on the system partition, and updates that were already installed stay in user storage. Fire OS won't let you remove those updates (see lesson 1). Free space with [step 10](10-free-storage.md) instead.

**5. "Android Settings" shortcuts in other launchers open Amazon's Settings.**
On Fire OS the standard Android settings request goes to Amazon's settings app. Long-press **Home → Settings** is the reliable way in.

**6. A helper says `FAILED`, "not recognized" or "command not found".**
- *"not recognized" / "command not found"*: the helpers aren't loaded in this terminal. `cd` into your working folder and paste the block from [step 4.0](04-snapshot.md) again.
- *`FAILED: adb could not read the package list`*: the stick isn't connected. Reconnect ([step 3](03-connect.md)) and run the same command again.
- *`FAILED: <file> not found` / `is not a snapshot saved by this guide`*: you're in the wrong folder, or the file was edited or replaced. `cd` to your working folder. Don't recreate a baseline or desired-state file from the stick's current state to "fix" this; that would record your changes as the starting point.

When a helper fails, it writes nothing, so your existing files are untouched.

### Still stuck?
Run these and include the output when asking for help:
```bash
adb shell getprop ro.build.version.name
```
```bash
adb shell pm list packages -d
```

---
⬅️ [11. Undo](11-undo.md) · 🏠 [Back to start](../README.md)
