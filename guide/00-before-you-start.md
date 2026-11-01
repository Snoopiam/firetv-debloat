# 0. Before you start

> ⚠️ **Use at your own risk.** This guide is provided as-is, without warranty. The authors are not responsible for any damage or consequences. Read the [full disclaimer](../DISCLAIMER.md) before continuing.

**You need:**
- The Fire TV and your computer on the **same Wi-Fi / home network**
- A Windows, macOS or Linux computer
- About 45 minutes for the full guide (the core debloat — steps 1–6 — takes about 25)

**How to read this guide**
- Every command is in its own code block. Copy one block, paste it into your terminal, press Enter, and compare what you see with the **Expected** line under it.
- `<FIRETV_IP>` means your Fire TV's IP address (you'll find it in step 1). Replace the whole thing, including the `< >`.
- Commands that start with `adb` work the same on Windows, macOS and Linux.
- Blocks marked **PowerShell** are Windows-only; each has a macOS/Linux alternative next to it.

**Make a working folder** for the files you'll save (undo files, package snapshot):

**Windows (PowerShell):**
```powershell
mkdir $HOME\firetv-debloat; cd $HOME\firetv-debloat
```

**macOS / Linux:**
```bash
mkdir -p ~/firetv-debloat && cd ~/firetv-debloat
```

> **Important:** Keep this terminal open in this folder for the whole guide. If you open a new terminal later, `cd` back into it first.

**Golden rules**
1. Only use `pm disable-user`. Never `pm uninstall` on Amazon apps — see [step 12](12-troubleshooting.md) for why it backfired in testing.
2. Never touch the packages on the [⛔ do-not-touch list](08-do-not-touch.md).
3. Save the **"before" list** before each group, and create that group's **undo file** right after it. Never delete `disabled-baseline.txt` (step 4).

---
➡️ Next: [1. Enable ADB debugging](01-enable-adb.md)
