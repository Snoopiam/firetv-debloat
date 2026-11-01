# 7. Group C — Features you might want to lose

**Caution: each package here works. Disabling it removes a feature**, so decide feature by feature. Every change here can be undone with the undo file in 7.7.

### Tested features
These four were disabled on the tested device (Fire TV Stick 4K, Fire OS 7.6.9.7). Apart from losing the feature itself, no side effects were observed. That's one device, so check your apps afterwards.

| Feature | Packages | What you lose |
|---|---|---|
| **Alexa voice** | `com.amazon.vizzini`, `com.amazon.vizzini.ftvcds`, `com.amazon.alexamediaplayer.runtime.ftv`, `com.amazon.alexa.externalmediaplayer.fireos`, `com.amazon.tv.alexaalerts`, `com.amazon.tv.alexanotifications` | The remote's Alexa/mic button stops working; Echo devices can't control the stick |
| **Live TV** | `com.amazon.tv.livetv` | The Live TV tab and channel guide |
| **Screensaver** | `com.amazon.ftv.screensaver` | Amazon's photo screensaver (a basic one replaces it) |
| **Text-to-speech** | `com.svox.pico`, `com.ivona.tts.oem`, `com.ivona.orchestrator` | VoiceView screen reader / accessibility speech |

### Not recommended — listed so you know what they are
| Feature | Packages | Why not |
|---|---|---|
| Prime Video | `com.amazon.avod` | The home screen is built around it; rows may show blank tiles |
| Casting from phones | `com.amazon.dialservice`, `com.amazon.cast.sink`, `com.amazon.awvflingreceiver` | Casting from YouTube/Netflix apps stops |
| Phone remote app | `com.amazon.whisperlink.core.android` | The Fire TV phone app can't find the stick |
| System updates | `com.amazon.device.software.ota`, `com.amazon.tv.forcedotaupdater.v2` | Blocks security patches; Fire OS likely refuses anyway |
| Metrics API | `com.amazon.client.metrics.api` | Some apps reportedly crash without it |

### 7.1 Save the "before" list
```bash
adb shell pm list packages -d > before-C.txt
```
**Expected:** no output. A file `before-C.txt` appears in your working folder.

### 7.2 Alexa voice (6 commands — do all six or none)
**Expected** for each: `Package <name> new state: disabled-user`
```bash
adb shell pm disable-user --user 0 com.amazon.vizzini
```
```bash
adb shell pm disable-user --user 0 com.amazon.vizzini.ftvcds
```
```bash
adb shell pm disable-user --user 0 com.amazon.alexamediaplayer.runtime.ftv
```
```bash
adb shell pm disable-user --user 0 com.amazon.alexa.externalmediaplayer.fireos
```
```bash
adb shell pm disable-user --user 0 com.amazon.tv.alexaalerts
```
```bash
adb shell pm disable-user --user 0 com.amazon.tv.alexanotifications
```

### 7.3 Live TV
```bash
adb shell pm disable-user --user 0 com.amazon.tv.livetv
```

### 7.4 Amazon screensaver
```bash
adb shell pm disable-user --user 0 com.amazon.ftv.screensaver
```

### 7.5 Text-to-speech (3 commands — do all three or none)
```bash
adb shell pm disable-user --user 0 com.svox.pico
```
```bash
adb shell pm disable-user --user 0 com.ivona.tts.oem
```
```bash
adb shell pm disable-user --user 0 com.ivona.orchestrator
```

### 7.6 Or: disable all four features in one go (optional)
Use this **instead of** 7.2–7.5. Save the "before" list (7.1) first.

> **Important:** This block disables **Alexa voice, Live TV, the screensaver and text-to-speech**. To keep a feature, **delete its whole set of lines** before pasting; don't keep half a feature. Paste the edited block in one go; each line runs as its own command.

```bash
adb shell pm disable-user --user 0 com.amazon.vizzini                        # Alexa voice
adb shell pm disable-user --user 0 com.amazon.vizzini.ftvcds                 # Alexa voice
adb shell pm disable-user --user 0 com.amazon.alexamediaplayer.runtime.ftv   # Alexa voice
adb shell pm disable-user --user 0 com.amazon.alexa.externalmediaplayer.fireos  # Alexa voice
adb shell pm disable-user --user 0 com.amazon.tv.alexaalerts                 # Alexa voice
adb shell pm disable-user --user 0 com.amazon.tv.alexanotifications          # Alexa voice
adb shell pm disable-user --user 0 com.amazon.tv.livetv                      # Live TV
adb shell pm disable-user --user 0 com.amazon.ftv.screensaver                # Screensaver
adb shell pm disable-user --user 0 com.svox.pico                             # Text-to-speech
adb shell pm disable-user --user 0 com.ivona.tts.oem                         # Text-to-speech
adb shell pm disable-user --user 0 com.ivona.orchestrator                    # Text-to-speech
```
**Expected:** one `Package <name> new state: disabled-user` line per command you kept. Scroll up and check every line.

### 7.7 Create your undo file
Writes an `adb shell pm enable` line for **each package this group actually changed**.

**Windows (PowerShell):**
```powershell
$before = @(Get-Content before-C.txt | ForEach-Object { $_.Trim() }); $undo = @(adb shell pm list packages -d | ForEach-Object { $_.Trim() } | Where-Object { $_ -and ($before -notcontains $_) } | ForEach-Object { 'adb shell pm enable ' + ($_ -replace '^package:', '') }); Set-Content undo-C.txt -Value $undo; Get-Content undo-C.txt
```

**macOS / Linux:**
```bash
adb shell pm list packages -d | tr -d '\r' | grep -vxFf <(tr -d '\r' < before-C.txt) | sed 's/^package:/adb shell pm enable /' > undo-C.txt; cat undo-C.txt
```

**Expected:** one `adb shell pm enable ...` line per package you disabled in this group.

### ✅ Check
- Press the mic button on the remote — nothing should happen (if you disabled Alexa)
- Open YouTube or Netflix — they should launch normally
- `undo-C.txt` lists exactly the packages you disabled in this group

---
⬅️ [6. Group B](06-unused-apps.md) · ➡️ Next: [8. Do not touch](08-do-not-touch.md)
