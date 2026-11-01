# 6. Group B — Unused Amazon apps

**Risk: safe** — as long as you don't use the app. Read the table and **skip any app you actually use**. Nothing else on the stick depends on these.

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

### 6.1 Save the undo file first
The undo file lists all 21. Re-enabling an app you skipped is harmless, so you don't need to edit it.

**Windows (PowerShell):**
```powershell
@'
adb shell pm enable com.amazon.imdb.tv.android.app
adb shell pm enable com.amazon.minitv.android.app
adb shell pm enable com.amazon.shoptv.client
adb shell pm enable com.amazon.shoptv.firetv.client
adb shell pm enable com.amazon.bueller.photos
adb shell pm enable com.amazon.bueller.music
adb shell pm enable com.amazon.gamehub
adb shell pm enable com.amazon.cloud9
adb shell pm enable com.amazon.tv.website_launcher
adb shell pm enable com.amazon.tahoe
adb shell pm enable com.amazon.storm.lightning.tutorial
adb shell pm enable com.amazon.tmm.tutorial
adb shell pm enable com.amazon.tv.releasenotes
adb shell pm enable com.amazon.stillwatching.activity
adb shell pm enable com.amazon.tv.support
adb shell pm enable com.amazon.whasettings
adb shell pm enable com.amazon.whad
adb shell pm enable com.amazon.wha.mediabrowserservice
adb shell pm enable com.amazon.spotify.mediabrowserservice
adb shell pm enable com.android.printspooler
adb shell pm enable com.android.bips
'@ | Set-Content undo-B-unused-apps.txt
```

**macOS / Linux:**
```bash
cat > undo-B-unused-apps.txt <<'EOF'
adb shell pm enable com.amazon.imdb.tv.android.app
adb shell pm enable com.amazon.minitv.android.app
adb shell pm enable com.amazon.shoptv.client
adb shell pm enable com.amazon.shoptv.firetv.client
adb shell pm enable com.amazon.bueller.photos
adb shell pm enable com.amazon.bueller.music
adb shell pm enable com.amazon.gamehub
adb shell pm enable com.amazon.cloud9
adb shell pm enable com.amazon.tv.website_launcher
adb shell pm enable com.amazon.tahoe
adb shell pm enable com.amazon.storm.lightning.tutorial
adb shell pm enable com.amazon.tmm.tutorial
adb shell pm enable com.amazon.tv.releasenotes
adb shell pm enable com.amazon.stillwatching.activity
adb shell pm enable com.amazon.tv.support
adb shell pm enable com.amazon.whasettings
adb shell pm enable com.amazon.whad
adb shell pm enable com.amazon.wha.mediabrowserservice
adb shell pm enable com.amazon.spotify.mediabrowserservice
adb shell pm enable com.android.printspooler
adb shell pm enable com.android.bips
EOF
```

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
Use this **instead of** 6.2. Save the undo file (6.1) first.

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

### ✅ Check
```bash
adb shell pm list packages -d
```
**Expected:** Group A's 8 + the Group B apps you disabled + `com.android.nfc`. If you did all of Group B: **30 lines**.

> **Important:** In testing, this group was **reset to enabled after a reboot** at least once, while Group A stayed off. Do [step 9](09-reboot-and-verify.md) to check yours.

---
⬅️ [5. Group A](05-ads-and-telemetry.md) · ➡️ Next: [7. Group C — Caution](07-caution-features.md)
