# 10. Free up storage (optional)

> ⚠️ **This page is not part of the reversible debloat.** Steps 5–7 only *disable* apps, and `pm enable` switches them back on. Nothing on this page can be undone with `pm enable` or your undo files. Read each step's **Can you undo it?** line before running it.

Disabling apps **doesn't free space**, because their files sit on the read-only system partition. This page recovers space from caches and leftover installer files instead. On the tested stick, clearing caches and deleting old installers took free space from **1.3 GB to about 3 GB**. Your results depend on what's on your stick.

| Step | What it deletes | Can you undo it? |
|---|---|---|
| 10.2 Clear caches | Temporary files every app keeps | **No**, but apps rebuild their caches automatically as you use them. Expect slightly slower first launches and images reloading. |
| 10.4 Delete installer files | `.apk` files you choose, one by one | **No.** Deleted permanently. Installed apps are unaffected, but you'd have to download an installer again to reinstall that app. |

### 10.1 Check free space
```bash
adb shell df -h /data
```
Look at the **Avail** column.

### 10.2 Clear every app's cache
Caches are temporary files that apps rebuild automatically. App logins, settings and data are **not** touched.
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
An error such as *No such file or directory* just means that folder or file type doesn't exist on your stick.

### 10.4 Delete the ones you don't need, one at a time
**Permanent.** Read each name before deleting it. Use the exact name from the listing, and keep both sets of quotes exactly as shown: the outer double quotes pass the command to the stick, and the inner single quotes protect names with spaces or brackets.
```bash
adb shell "rm -f '/sdcard/Download/Downloader/<FILE_NAME>.apk'"
```
**Expected:** no output. Repeat for each file.

> **Important:** Only delete `.apk`, `.apkm` and `.xapk` installer files. Leave everything else, including backup files (e.g. `*_Backup*.json`, `*.txt`). Some apps keep their settings backups in Download.

### 10.5 Check again
```bash
adb shell df -h /data
```

> **Not included on purpose:** clearing the Android package installer's data (`pm clear com.android.packageinstaller`). It freed some space in testing, but it wipes everything that app stores, not just leftovers, and we can't say exactly what that is on every stick. It's not worth the risk for the space.

> **Important:** Don't try to "remove updates" of the Amazon apps you disabled (`pm uninstall <package>` or `cmd package uninstall-system-updates <package>`). Fire OS refuses with `DELETE_FAILED_INTERNAL_ERROR`, **and the failed attempt re-enables the app**. This happened in testing to all 9 apps tried. See [step 12](12-troubleshooting.md).

### ✅ Check
**Avail** in 10.5 is higher than in 10.1.

---
⬅️ [9. Reboot and verify](09-reboot-and-verify.md) · ➡️ Next: [11. Undo](11-undo.md)
