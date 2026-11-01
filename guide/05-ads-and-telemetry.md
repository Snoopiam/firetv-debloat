# 5. Group A — Ads, tracking & telemetry

**Risk: safe.** These services serve sponsored content, track what you watch, and upload logs and metrics to Amazon. Nothing you use depends on them.

| Package | What it is |
|---|---|
| `com.amazon.kso.blackbird` | Sponsored/ad content on the home screen |
| `com.amazon.hybridadidservice` | Advertising ID service |
| `com.amazon.tv.acr` | Automatic content recognition (tracks what's on screen) |
| `com.amazon.sneakpeek` | Autoplay video previews on the home screen |
| `com.amazon.dp.logger` | Logging |
| `com.amazon.tv.fw.metrics` | Framework metrics |
| `com.amazon.wirelessmetrics.service` | Wi-Fi metrics |
| `com.amazon.perfcollection` | Performance data collection |

### 5.1 Save the undo file first

**Windows (PowerShell):**
```powershell
@'
adb shell pm enable com.amazon.kso.blackbird
adb shell pm enable com.amazon.hybridadidservice
adb shell pm enable com.amazon.tv.acr
adb shell pm enable com.amazon.sneakpeek
adb shell pm enable com.amazon.dp.logger
adb shell pm enable com.amazon.tv.fw.metrics
adb shell pm enable com.amazon.wirelessmetrics.service
adb shell pm enable com.amazon.perfcollection
'@ | Set-Content undo-A-ads-telemetry.txt
```

**macOS / Linux:**
```bash
cat > undo-A-ads-telemetry.txt <<'EOF'
adb shell pm enable com.amazon.kso.blackbird
adb shell pm enable com.amazon.hybridadidservice
adb shell pm enable com.amazon.tv.acr
adb shell pm enable com.amazon.sneakpeek
adb shell pm enable com.amazon.dp.logger
adb shell pm enable com.amazon.tv.fw.metrics
adb shell pm enable com.amazon.wirelessmetrics.service
adb shell pm enable com.amazon.perfcollection
EOF
```

### 5.2 Disable, one at a time

For every command below, **Expected:** `Package <name> new state: disabled-user`

```bash
# Sponsored home-screen content
adb shell pm disable-user --user 0 com.amazon.kso.blackbird
```
```bash
# Advertising ID
adb shell pm disable-user --user 0 com.amazon.hybridadidservice
```
```bash
# Automatic content recognition
adb shell pm disable-user --user 0 com.amazon.tv.acr
```
```bash
# Autoplay previews
adb shell pm disable-user --user 0 com.amazon.sneakpeek
```
```bash
# Logging
adb shell pm disable-user --user 0 com.amazon.dp.logger
```
```bash
# Framework metrics
adb shell pm disable-user --user 0 com.amazon.tv.fw.metrics
```
```bash
# Wi-Fi metrics
adb shell pm disable-user --user 0 com.amazon.wirelessmetrics.service
```
```bash
# Performance collection
adb shell pm disable-user --user 0 com.amazon.perfcollection
```

### 5.3 Or: disable all 8 in one go (optional)
Use this **instead of** 5.2 if you'd rather not paste eight times. Save the undo file (5.1) first. Paste the whole block at once; each line runs as its own command (Windows PowerShell, macOS and Linux).
```bash
adb shell pm disable-user --user 0 com.amazon.kso.blackbird
adb shell pm disable-user --user 0 com.amazon.hybridadidservice
adb shell pm disable-user --user 0 com.amazon.tv.acr
adb shell pm disable-user --user 0 com.amazon.sneakpeek
adb shell pm disable-user --user 0 com.amazon.dp.logger
adb shell pm disable-user --user 0 com.amazon.tv.fw.metrics
adb shell pm disable-user --user 0 com.amazon.wirelessmetrics.service
adb shell pm disable-user --user 0 com.amazon.perfcollection
```
**Expected:** 8 lines of `Package <name> new state: disabled-user`. Scroll up and check every line. If one shows an error, look it up in [step 12](12-troubleshooting.md).

### Protected — Fire OS refuses these
In testing, Fire OS 7.6 rejected these five with `java.lang.SecurityException: Cannot disable a protected package`. They can't be disabled without root, so they're not in the list above. Don't try workarounds (see [step 12](12-troubleshooting.md)).

| Package | What it is |
|---|---|
| `com.amazon.ftvads.deeplinking` | Ad deep-links |
| `com.amazon.device.crashmanager` | Crash report uploader |
| `com.amazon.device.logmanager` | Log uploader |
| `com.amazon.client.metrics` | Usage metrics |
| `com.amazon.device.metrics` | Device metrics |

> **Tip:** To silence those five, block Amazon's metrics/ad domains at your router or with a DNS filter (Pi-hole, NextDNS). Be careful: the home screen's ad tiles come from the same Amazon servers as the rest of the home screen, so aggressive blocking can break it.

### ✅ Check
```bash
adb shell pm list packages -d
```
**Expected:** the 8 packages above plus `com.android.nfc` — **9 lines** in total.

> **Note:** The big sponsored tiles on Amazon's home screen (e.g. "Watch on BBC iPlayer", "Tubi") **won't disappear**. They're delivered by Amazon's servers and drawn by the home screen itself, which can't be disabled. The privacy toggles in Settings → Preferences → Privacy only stop personalisation; they don't remove them either.

---
⬅️ [4. Snapshot](04-snapshot.md) · ➡️ Next: [6. Group B — Unused apps](06-unused-apps.md)
