# 10. Free up storage (optional)

Disabling apps **does not free space** — their files sit on the read-only system partition. These steps recover space from caches and leftover files instead. In testing they took free space from **1.3 GB to 3.1 GB**. Installed apps, logins and app data are not touched.

### 10.1 Check free space
```bash
adb shell df -h /data
```
Look at the **Avail** column.

### 10.2 Clear every app's cache
Caches are temporary files that apps rebuild automatically.
```bash
adb shell pm trim-caches 999G
```
**Expected:** no output.

### 10.3 Find leftover installer files
Sideloading apps (e.g. with the *Downloader* app) leaves `.apk` files behind. The installed app doesn't need them.
```bash
adb shell ls -la /sdcard/Download /sdcard/Download/Downloader
```
```bash
adb shell "ls -la /sdcard/*.apk"
```

### 10.4 Delete the ones you don't need — one at a time
Use the exact name from the listing. Keep both sets of quotes exactly as shown: the outer double quotes pass the command to the stick, and the inner single quotes protect names with spaces or brackets.
```bash
adb shell "rm -f '/sdcard/Download/Downloader/<FILE_NAME>.apk'"
```
**Expected:** no output. Repeat for each file.

> **Important:** Only delete `.apk`, `.apkm` and `.xapk` installer files. Leave backup files (e.g. `*_Backup*.json`, `*.txt`) — some apps keep their settings backups in Download.

### 10.5 Clear the package installer's leftovers
```bash
adb shell pm clear com.android.packageinstaller
```
**Expected:** `Success`

### 10.6 Check again
```bash
adb shell df -h /data
```

> **Important:** Don't try to "remove updates" of the Amazon apps you disabled (`pm uninstall <package>` or `cmd package uninstall-system-updates <package>`). Fire OS refuses with `DELETE_FAILED_INTERNAL_ERROR` — **and the failed attempt re-enables the app**. This happened in testing to all 9 apps tried. See [step 12](12-troubleshooting.md).

### ✅ Check
**Avail** in 10.6 is higher than in 10.1.

---
⬅️ [9. Reboot and verify](09-reboot-and-verify.md) · ➡️ Next: [11. Undo](11-undo.md)
