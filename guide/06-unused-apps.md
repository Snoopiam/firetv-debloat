# 6. Group B — Unused Amazon apps

**Tested:** all 21 were disabled on the tested device (Fire TV Stick 4K, Fire OS 7.6.9.7), and nothing else on it stopped working. That doesn't guarantee another app on *your* stick doesn't use one of them, so **skip any app you use**, and check the apps you rely on afterwards.

| Package | What it is | Skip if you use… |
|---|---|---|
| `com.amazon.imdb.tv.android.app` | Freevee / IMDb TV | Freevee |
| `com.amazon.minitv.android.app` | Amazon miniTV | miniTV |
| `com.amazon.shoptv.client` | Amazon Shopping | Shopping on TV |
| `com.amazon.shoptv.firetv.client` | Amazon Shopping (Fire TV client) | Shopping on TV |
| `com.amazon.bueller.photos` | Amazon Photos | Photos / photo screensaver |
| `com.amazon.bueller.music` | Amazon Music | Amazon Music |
| `com.amazon.gamehub` | Games hub | Amazon games |
| `com.amazon.cloud9` | Silk browser | The built-in browser |
| `com.amazon.tv.website_launcher` | Website tiles | — |
| `com.amazon.tahoe` | Amazon Kids | Kids profiles |
| `com.amazon.storm.lightning.tutorial` | Tutorial | — |
| `com.amazon.tmm.tutorial` | Tutorial | — |
| `com.amazon.tv.releasenotes` | "What's new" screens | — |
| `com.amazon.stillwatching.activity` | "Are you still watching?" prompt | — |
| `com.amazon.tv.support` | Help | — |
| `com.amazon.whasettings` | Multi-room audio settings | Echo multi-room audio |
| `com.amazon.whad` | Multi-room audio service | Echo multi-room audio |
| `com.amazon.wha.mediabrowserservice` | Multi-room audio media service | Echo multi-room audio |
| `com.amazon.spotify.mediabrowserservice` | Alexa ↔ Spotify bridge | Asking Alexa to play Spotify |
| `com.android.printspooler` | Printing | — |
| `com.android.bips` | Printing | — |

### 6.1 Save the "before" list
```bash
adb shell pm list packages -d > before-B.txt
```
**Expected:** no output. A file `before-B.txt` appears in your working folder.

### 6.2 Disable, one at a time — skip what you use

**Expected** for each: `Package <name> new state: disabled-user`

```bash
# Freevee / IMDb TV
adb shell pm disable-user --user 0 com.amazon.imdb.tv.android.app
```
```bash
# Amazon miniTV
adb shell pm disable-user --user 0 com.amazon.minitv.android.app
```
```bash
# Amazon Shopping
adb shell pm disable-user --user 0 com.amazon.shoptv.client
```
```bash
# Amazon Shopping (Fire TV client)
adb shell pm disable-user --user 0 com.amazon.shoptv.firetv.client
```
```bash
# Amazon Photos — skip if you use the Amazon Photos screensaver
adb shell pm disable-user --user 0 com.amazon.bueller.photos
```
```bash
# Amazon Music
adb shell pm disable-user --user 0 com.amazon.bueller.music
```
```bash
# Games hub
adb shell pm disable-user --user 0 com.amazon.gamehub
```
```bash
# Silk browser
adb shell pm disable-user --user 0 com.amazon.cloud9
```
```bash
# Website tiles
adb shell pm disable-user --user 0 com.amazon.tv.website_launcher
```
```bash
# Amazon Kids
adb shell pm disable-user --user 0 com.amazon.tahoe
```
```bash
# Tutorial
adb shell pm disable-user --user 0 com.amazon.storm.lightning.tutorial
```
```bash
# Tutorial
adb shell pm disable-user --user 0 com.amazon.tmm.tutorial
```
```bash
# "What's new" screens
adb shell pm disable-user --user 0 com.amazon.tv.releasenotes
```
```bash
# "Are you still watching?" prompt
adb shell pm disable-user --user 0 com.amazon.stillwatching.activity
```
```bash
# Help
adb shell pm disable-user --user 0 com.amazon.tv.support
```
```bash
# Multi-room audio settings
adb shell pm disable-user --user 0 com.amazon.whasettings
```
```bash
# Multi-room audio service
adb shell pm disable-user --user 0 com.amazon.whad
```
```bash
# Multi-room audio media service
adb shell pm disable-user --user 0 com.amazon.wha.mediabrowserservice
```
```bash
# Alexa <-> Spotify bridge
adb shell pm disable-user --user 0 com.amazon.spotify.mediabrowserservice
```
```bash
# Printing
adb shell pm disable-user --user 0 com.android.printspooler
```
```bash
# Printing
adb shell pm disable-user --user 0 com.android.bips
```

### 6.3 Or: disable the whole group in one go (optional)
Use this **instead of** 6.2. Save the "before" list (6.1) first.

> **Important:** This block disables **all 21**. Before pasting, **delete the line of any app you use** (the comments at the end of each line tell you which app it is). Paste the edited block in one go; each line runs as its own command.

```bash
adb shell pm disable-user --user 0 com.amazon.imdb.tv.android.app        # Freevee
adb shell pm disable-user --user 0 com.amazon.minitv.android.app         # miniTV
adb shell pm disable-user --user 0 com.amazon.shoptv.client              # Shopping
adb shell pm disable-user --user 0 com.amazon.shoptv.firetv.client       # Shopping
adb shell pm disable-user --user 0 com.amazon.bueller.photos             # Amazon Photos
adb shell pm disable-user --user 0 com.amazon.bueller.music              # Amazon Music
adb shell pm disable-user --user 0 com.amazon.gamehub                    # Games hub
adb shell pm disable-user --user 0 com.amazon.cloud9                     # Silk browser
adb shell pm disable-user --user 0 com.amazon.tv.website_launcher        # Website tiles
adb shell pm disable-user --user 0 com.amazon.tahoe                      # Amazon Kids
adb shell pm disable-user --user 0 com.amazon.storm.lightning.tutorial   # Tutorial
adb shell pm disable-user --user 0 com.amazon.tmm.tutorial               # Tutorial
adb shell pm disable-user --user 0 com.amazon.tv.releasenotes            # What's new
adb shell pm disable-user --user 0 com.amazon.stillwatching.activity     # Still watching?
adb shell pm disable-user --user 0 com.amazon.tv.support                 # Help
adb shell pm disable-user --user 0 com.amazon.whasettings                # Multi-room audio
adb shell pm disable-user --user 0 com.amazon.whad                       # Multi-room audio
adb shell pm disable-user --user 0 com.amazon.wha.mediabrowserservice    # Multi-room audio
adb shell pm disable-user --user 0 com.amazon.spotify.mediabrowserservice  # Alexa-Spotify bridge
adb shell pm disable-user --user 0 com.android.printspooler              # Printing
adb shell pm disable-user --user 0 com.android.bips                      # Printing
```
**Expected:** one `Package <name> new state: disabled-user` line per command you kept. Scroll up and check every line.

> **Note:** `com.amazon.tv.csapp` (customer support) is left out on purpose — Fire OS refused it as a protected package in testing.

### 6.4 Create your undo file
Writes an `adb shell pm enable` line for **each package this group actually changed**. Apps you skipped, and anything that was already disabled before you started, stay out of it.

**Windows (PowerShell):**
```powershell
$before = @(Get-Content before-B.txt | ForEach-Object { $_.Trim() }); $undo = @(adb shell pm list packages -d | ForEach-Object { $_.Trim() } | Where-Object { $_ -and ($before -notcontains $_) } | ForEach-Object { 'adb shell pm enable ' + ($_ -replace '^package:', '') }); Set-Content undo-B.txt -Value $undo; Get-Content undo-B.txt
```

**macOS / Linux:**
```bash
adb shell pm list packages -d | tr -d '\r' | grep -vxFf <(tr -d '\r' < before-B.txt) | sed 's/^package:/adb shell pm enable /' > undo-B.txt; cat undo-B.txt
```

**Expected:** one `adb shell pm enable ...` line per app you disabled in this group.

### ✅ Check
- `undo-B.txt` lists exactly the apps you disabled
- Open the apps you use. They should start and play normally.

> **Important:** In testing, this group was **reset to enabled after a reboot** at least once, while Group A stayed off. Do [step 9](09-reboot-and-verify.md) to check yours.

---
⬅️ [5. Group A](05-ads-and-telemetry.md) · ➡️ Next: [7. Group C — Caution](07-caution-features.md)
