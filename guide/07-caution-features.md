# 7. Group C — Features you might want to lose

**Risk: caution.** Each package here *works fine* — disabling it removes a feature. Decide feature by feature. Everything is still reversible.

### Tested: these four features were disabled in the original session with no side effects
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

### 7.1 Save the undo file first

**Windows (PowerShell):**
```powershell
@'
adb shell pm enable com.amazon.vizzini
adb shell pm enable com.amazon.vizzini.ftvcds
adb shell pm enable com.amazon.alexamediaplayer.runtime.ftv
adb shell pm enable com.amazon.alexa.externalmediaplayer.fireos
adb shell pm enable com.amazon.tv.alexaalerts
adb shell pm enable com.amazon.tv.alexanotifications
adb shell pm enable com.amazon.tv.livetv
adb shell pm enable com.amazon.ftv.screensaver
adb shell pm enable com.svox.pico
adb shell pm enable com.ivona.tts.oem
adb shell pm enable com.ivona.orchestrator
'@ | Set-Content undo-C-caution.txt
```

**macOS / Linux:**
```bash
cat > undo-C-caution.txt <<'EOF'
adb shell pm enable com.amazon.vizzini
adb shell pm enable com.amazon.vizzini.ftvcds
adb shell pm enable com.amazon.alexamediaplayer.runtime.ftv
adb shell pm enable com.amazon.alexa.externalmediaplayer.fireos
adb shell pm enable com.amazon.tv.alexaalerts
adb shell pm enable com.amazon.tv.alexanotifications
adb shell pm enable com.amazon.tv.livetv
adb shell pm enable com.amazon.ftv.screensaver
adb shell pm enable com.svox.pico
adb shell pm enable com.ivona.tts.oem
adb shell pm enable com.ivona.orchestrator
EOF
```

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
Use this **instead of** 7.2–7.5. Save the undo file (7.1) first.

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

### ✅ Check
- Press the mic button on the remote — nothing should happen (if you disabled Alexa)
- Open YouTube or Netflix — they should launch normally
```bash
adb shell pm list packages -d
```
**Expected:** everything from A and B, plus what you disabled here. All three groups in full: **41 lines** (40 + `com.android.nfc`).

---
⬅️ [6. Group B](06-unused-apps.md) · ➡️ Next: [8. Do not touch](08-do-not-touch.md)
