# 2. Install Google's platform tools (ADB)

ADB comes in Google's **Android SDK Platform-Tools**. Install it with your system's package manager — it's the quickest way and puts `adb` on your PATH.

### Windows
```powershell
winget install Google.PlatformTools
```
Then **close and reopen PowerShell** so the new `adb` command is found, and `cd` back into your working folder:
```powershell
cd $HOME\firetv-debloat
```

### macOS (Homebrew)
```bash
brew install --cask android-platform-tools
```

### Linux (Debian / Ubuntu)
```bash
sudo apt install adb
```

> **No package manager?** Download the ZIP for your OS from Google's official page — [developer.android.com/tools/releases/platform-tools](https://developer.android.com/tools/releases/platform-tools) — unzip it, and run the commands from inside the unzipped `platform-tools` folder (on macOS/Linux type `./adb` instead of `adb`).

### ✅ Check
```bash
adb version
```
**Expected:** `Android Debug Bridge version 1.0.41` (or newer) and a version line.

If you get *"adb is not recognized"* / *"command not found"*: on Windows reopen PowerShell; otherwise use the ZIP method above.

---
⬅️ [1. Enable ADB](01-enable-adb.md) · ➡️ Next: [3. Connect](03-connect.md)
