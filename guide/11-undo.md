# 11. Undo everything (or anything)

Nothing in this guide is permanent.

### Undo one app
```bash
adb shell pm enable <package.name>
```
**Expected:** `Package <package.name> new state: enabled`

### Undo a whole group
Open the undo file you saved for that group — `undo-A-ads-telemetry.txt`, `undo-B-unused-apps.txt` or `undo-C-caution.txt` — and paste its lines into your terminal (all at once is fine: each is a separate command).

**Windows (PowerShell)** — runs every line of a file for you:
```powershell
Get-Content undo-B-unused-apps.txt | ForEach-Object { Invoke-Expression $_ }
```

**macOS / Linux:**
```bash
sh undo-B-unused-apps.txt
```

Lost your undo files? Copies are in this repo's [`undo/`](../undo/) folder.

### Check
```bash
adb shell pm list packages -d
```
The packages you re-enabled no longer appear.

### Nuclear option
**Settings → My Fire TV → Reset to Factory Defaults** puts the stick back to stock (and wipes your apps and sign-ins).

### Turn ADB back off
When you're done, you can switch **Settings → My Fire TV → Developer Options → ADB Debugging** off. Your changes stay; you just won't be able to connect until you turn it on again.

---
⬅️ [10. Free up storage](10-free-storage.md) · ➡️ Next: [12. Troubleshooting](12-troubleshooting.md)
