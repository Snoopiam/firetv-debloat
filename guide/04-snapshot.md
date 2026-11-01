# 4. Snapshot: list what's on your stick

Before changing anything, save a record of your stick's firmware and every enabled system package. This is your "before" picture — useful for comparing later and for anyone helping you.

### 4.0 Load the helper commands (once per terminal window)
The guide keeps track of what you change by saving lists of disabled packages and comparing them. Doing that safely takes more than a one-liner: if a file is missing, or the stick isn't connected, the result must be **refused**, not quietly wrong. So paste these helper commands once. They only ever **read** from the stick and write text files in your working folder; they never change the stick.

| Helper (PowerShell / macOS-Linux) | What it does |
|---|---|
| `Save-FireSnapshot` / `fire_save` | Saves the stick's current list of disabled packages to a file. Refuses (and saves nothing) if adb fails or returns anything that isn't a package line. |
| `New-FireUndo` / `fire_undo` | Compares a saved snapshot with the stick now and writes an undo file containing **only** the packages that changed. Refuses (and leaves any existing undo file alone) if the snapshot is missing, isn't one saved by this guide, or adb fails. |
| `Compare-FireSnapshot` / `fire_compare` | Shows packages that differ from a saved snapshot, by name. |

Every saved snapshot starts with the line `# fire-tv snapshot`. That line is how the helpers know a file is a real snapshot, including a snapshot of an empty list.

**Windows (PowerShell):** paste this whole block, then press Enter:
```powershell
function Get-FireDisabled {
    $out = @(adb shell pm list packages -d 2>&1)
    if ($LASTEXITCODE -ne 0) { Write-Host "FAILED: adb could not read the package list: $($out -join ' ')" -ForegroundColor Red; return $null }
    $pk = @($out | ForEach-Object { "$_".Trim() } | Where-Object { $_ })
    $bad = @($pk | Where-Object { $_ -notlike 'package:*' })
    if ($bad.Count) { Write-Host "FAILED: unexpected adb output: $($bad[0])" -ForegroundColor Red; return $null }
    return ,$pk
}
function Read-FireSnapshot($File) {
    if (-not (Test-Path -LiteralPath $File)) { Write-Host "FAILED: $File not found" -ForegroundColor Red; return $null }
    $lines = @(Get-Content -LiteralPath $File | ForEach-Object { "$_".Trim() } | Where-Object { $_ })
    if ($lines.Count -eq 0 -or $lines[0] -ne '# fire-tv snapshot') { Write-Host "FAILED: $File is not a snapshot saved by this guide" -ForegroundColor Red; return $null }
    $pk = @($lines | Select-Object -Skip 1)
    if (@($pk | Where-Object { $_ -notlike 'package:*' }).Count) { Write-Host "FAILED: $File contains lines that aren't packages" -ForegroundColor Red; return $null }
    return ,$pk
}
function Save-FireSnapshot($File) {
    $now = Get-FireDisabled; if ($null -eq $now) { Write-Host "Nothing saved." -ForegroundColor Red; return }
    Set-Content -LiteralPath $File -Value (@('# fire-tv snapshot') + $now)
    Write-Host "Saved $File ($($now.Count) disabled)" -ForegroundColor Green
}
function New-FireUndo($Before, $UndoFile) {
    $was = Read-FireSnapshot $Before; if ($null -eq $was) { Write-Host "Undo file NOT created or changed." -ForegroundColor Red; return }
    $now = Get-FireDisabled; if ($null -eq $now) { Write-Host "Undo file NOT created or changed." -ForegroundColor Red; return }
    $undo = @($now | Where-Object { $was -notcontains $_ } | ForEach-Object { 'adb shell pm enable ' + ($_ -replace '^package:', '') })
    Set-Content -LiteralPath $UndoFile -Value $undo
    Write-Host "Saved $UndoFile ($($undo.Count) to re-enable):" -ForegroundColor Green; $undo
}
function Compare-FireSnapshot($File) {
    $want = Read-FireSnapshot $File; if ($null -eq $want) { return }
    $now = Get-FireDisabled; if ($null -eq $now) { return }
    $on = @($want | Where-Object { $now -notcontains $_ }); $new = @($now | Where-Object { $want -notcontains $_ })
    '--- Switched back ON (should be off):'; $on
    '--- Newly disabled (not in your saved state):'; $new
    if ($on.Count -eq 0 -and $new.Count -eq 0) { Write-Host 'MATCH: the stick is exactly as saved.' -ForegroundColor Green }
}
```

**macOS / Linux (bash):** paste this whole block, then press Enter:
```bash
fire_get() {
  local out
  if ! out=$(adb shell pm list packages -d 2>&1); then echo "FAILED: adb could not read the package list: $out" >&2; return 1; fi
  out=$(printf '%s\n' "$out" | tr -d '\r' | sed 's/[[:space:]]*$//; /^$/d')
  if [ -n "$out" ] && printf '%s\n' "$out" | grep -qv '^package:'; then
    echo "FAILED: unexpected adb output: $(printf '%s\n' "$out" | grep -v '^package:' | head -1)" >&2; return 1; fi
  if [ -n "$out" ]; then printf '%s\n' "$out"; fi
}
fire_read() {
  local body
  if [ ! -f "$1" ]; then echo "FAILED: $1 not found" >&2; return 1; fi
  if [ "$(tr -d '\r' < "$1" | sed '/^[[:space:]]*$/d' | head -1)" != "# fire-tv snapshot" ]; then echo "FAILED: $1 is not a snapshot saved by this guide" >&2; return 1; fi
  body=$(tr -d '\r' < "$1" | sed 's/[[:space:]]*$//; /^$/d' | tail -n +2)
  if [ -n "$body" ] && printf '%s\n' "$body" | grep -qv '^package:'; then echo "FAILED: $1 contains lines that aren't packages" >&2; return 1; fi
  if [ -n "$body" ]; then printf '%s\n' "$body"; fi
}
fire_save() {
  local now
  now=$(fire_get) || { echo "Nothing saved." >&2; return 1; }
  { echo '# fire-tv snapshot'; if [ -n "$now" ]; then printf '%s\n' "$now"; fi; } > "$1"
  echo "Saved $1 ($(grep -c '^package:' "$1") disabled)"
}
fire_undo() {
  local was now undo
  was=$(fire_read "$1") || { echo "Undo file NOT created or changed." >&2; return 1; }
  now=$(fire_get) || { echo "Undo file NOT created or changed." >&2; return 1; }
  undo=$(printf '%s\n' "$now" | grep -vxF -f <(printf '%s\n' "$was") | sed '/^$/d; s/^package:/adb shell pm enable /')
  if [ -n "$undo" ]; then printf '%s\n' "$undo" > "$2"; else : > "$2"; fi
  echo "Saved $2 ($(grep -c . "$2") to re-enable):"; cat "$2"
}
fire_compare() {
  local want now on new
  want=$(fire_read "$1") || return 1
  now=$(fire_get) || return 1
  on=$(printf '%s\n' "$want" | grep -vxF -f <(printf '%s\n' "$now") | sed '/^$/d')
  new=$(printf '%s\n' "$now" | grep -vxF -f <(printf '%s\n' "$want") | sed '/^$/d')
  echo '--- Switched back ON (should be off):'; if [ -n "$on" ]; then printf '%s\n' "$on"; fi
  echo '--- Newly disabled (not in your saved state):'; if [ -n "$new" ]; then printf '%s\n' "$new"; fi
  if [ -z "$on" ] && [ -z "$new" ]; then echo 'MATCH: the stick is exactly as saved.'; fi
}
```

**Expected:** no output. Check they're loaded:

**Windows (PowerShell):**
```powershell
Get-Command Save-FireSnapshot, New-FireUndo, Compare-FireSnapshot
```
**macOS / Linux:**
```bash
type fire_save fire_undo fire_compare | grep -c function
```
**Expected:** three commands listed (PowerShell), or `3` (macOS / Linux).

> **Important:** The helpers last until you close the terminal. If you open a new one later, `cd` into your working folder and paste this block again. You'll know you need to if you see *"not recognized"* or *"command not found"*.

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
**Windows (PowerShell):**
```powershell
Save-FireSnapshot disabled-baseline.txt
```
**macOS / Linux:**
```bash
fire_save disabled-baseline.txt
```
**Expected:** `Saved disabled-baseline.txt (N disabled)`. If you see `FAILED`, nothing was saved: reconnect ([step 3](03-connect.md)) and run it again.

Look at what's disabled:
```bash
adb shell pm list packages -d
```
**On the tested stick:** just `package:com.android.nfc`. Amazon disables NFC at the factory because the stick has no NFC chip. Yours may list more, for example if someone has debloated it before. That's fine: anything in this file stays exactly as it is.

> **Important:** Don't delete or overwrite `disabled-baseline.txt`. [Step 11](11-undo.md) uses it to restore everything, even if you lose the other files.

### ✅ Check
- The helpers are loaded (4.0)
- `before-enabled.txt` and `disabled-baseline.txt` exist in your working folder, and the baseline was reported `Saved`
- You know your firmware version and roughly which packages exist

---
⬅️ [3. Connect](03-connect.md) · ➡️ Next: [5. Group A — Ads & telemetry](05-ads-and-telemetry.md)
