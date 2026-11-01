# 11. Undo everything (or anything)

Everything in steps 5–7 can be undone. (The storage cleanup in step 10 can't; see that page.)

Your undo files list **only the packages the guide actually changed** on your stick. Running them can't switch on anything that was already disabled before you started.

### Undo one app
```bash
adb shell pm enable <package.name>
```
**Expected:** `Package <package.name> new state: enabled`

### Undo a whole group
Your files are `undo-A.txt`, `undo-B.txt` and `undo-C.txt`, in your working folder.

**Windows (PowerShell):** runs every line of the file:
```powershell
Get-Content undo-B.txt | ForEach-Object { Invoke-Expression $_ }
```

**macOS / Linux:**
```bash
sh undo-B.txt
```
**Expected:** one `new state: enabled` line per package in the file.

### Undo everything: back to your starting point
This uses `disabled-baseline.txt` from [step 4.4](04-snapshot.md), and works even if you've lost your group undo files. It builds `undo-all.txt` with every package that's disabled now but **wasn't** when you started. The helper commands from [step 4.0](04-snapshot.md) must be loaded. If the baseline is missing or unreadable, or the stick isn't connected, the helper prints `FAILED` and creates nothing, so it can't produce a wrong list.

**Windows (PowerShell):**
```powershell
New-FireUndo disabled-baseline.txt undo-all.txt
```

**macOS / Linux:**
```bash
fire_undo disabled-baseline.txt undo-all.txt
```

**Read the list first.** If you disabled anything yourself outside this guide since step 4, it'll be in there too. Delete those lines if you want to keep them disabled. Then run it like a group file:

**Windows (PowerShell):**
```powershell
Get-Content undo-all.txt | ForEach-Object { Invoke-Expression $_ }
```
**macOS / Linux:**
```bash
sh undo-all.txt
```

### Check
After a full undo, the stick should match your starting point exactly:

**Windows (PowerShell):**
```powershell
Compare-FireSnapshot disabled-baseline.txt
```
**macOS / Linux:**
```bash
fire_compare disabled-baseline.txt
```
**Expected:** `MATCH: the stick is exactly as saved.`

> **Note:** After any undo, your `desired-state.txt` from step 9 no longer matches what you want, so the next reboot check would flag the packages you just re-enabled. Once you're happy with the stick, save a new desired state ([step 9.6](09-reboot-and-verify.md)).

### No baseline and no undo files?
Every package this guide can disable is listed in steps 5–7, and `pm enable` works on any of them. But without your baseline you can't tell which of them were already disabled before you started, so only re-enable the ones you know you changed.

### Nuclear option
**Settings → My Fire TV → Reset to Factory Defaults** puts the stick back to stock, and wipes your apps and sign-ins.

### Turn ADB back off
When you're done, you can switch **Settings → My Fire TV → Developer Options → ADB Debugging** off. Your changes stay; you just won't be able to connect until you turn it on again.

---
⬅️ [10. Free up storage](10-free-storage.md) · ➡️ Next: [12. Troubleshooting](12-troubleshooting.md)
