# 4. Snapshot: list what's on your stick

Before changing anything, save a record of your stick's firmware and every enabled system package. This is your "before" picture — useful for comparing later and for anyone helping you.

### 4.1 Record the firmware version
```bash
adb shell getprop ro.build.version.name
```
**Expected:** something like `Fire OS 7.6.9.7 (PS7697/4800)`

### 4.2 Save the list of enabled system packages
```bash
adb shell pm list packages -s -e > before-enabled.txt
```
This writes a file `before-enabled.txt` in your working folder (one `package:...` line per app). Nothing prints on screen.

### 4.3 See which packages from this guide exist on your stick
This one command checks every package the guide touches and prints the ones your stick actually has:
```bash
adb shell "pm list packages | grep -E 'kso.blackbird|hybridadidservice|tv.acr|sneakpeek|dp.logger|tv.fw.metrics|wirelessmetrics|perfcollection|imdb.tv|minitv|shoptv|bueller|gamehub|cloud9|website_launcher|tahoe|lightning.tutorial|tmm.tutorial|releasenotes|stillwatching|tv.support|whasettings|whad|wha.mediabrowser|spotify.mediabrowser|printspooler|bips|vizzini|alexamediaplayer|externalmediaplayer|alexaalerts|alexanotifications|tv.livetv|ftv.screensaver|svox.pico|ivona'"
```
**Expected:** a list of `package:com.amazon...` lines. On the tested Fire TV Stick 4K this matched 40 packages.

> **Note:** If a package from the guide isn't on your stick, its command later simply prints an *"Unknown package"* error. That's harmless — skip it and carry on.

### 4.4 Save the list of packages that are already disabled (your baseline)
This is the most important file in the guide. It records what was **already** disabled before you changed anything, so undo can put your stick back exactly as it was and not switch on something that was off to begin with.
```bash
adb shell pm list packages -d > disabled-baseline.txt
```
Look at it:
```bash
adb shell pm list packages -d
```
**On the tested stick:** just `package:com.android.nfc`. Amazon disables NFC at the factory because the stick has no NFC chip. Yours may list more, for example if someone has debloated it before. That's fine: anything in this file stays exactly as it is.

> **Important:** Don't delete or overwrite `disabled-baseline.txt`. [Step 11](11-undo.md) uses it to restore everything, even if you lose the other files.

### ✅ Check
- `before-enabled.txt` and `disabled-baseline.txt` exist in your working folder
- You know your firmware version and roughly which packages exist

---
⬅️ [3. Connect](03-connect.md) · ➡️ Next: [5. Group A — Ads & telemetry](05-ads-and-telemetry.md)
