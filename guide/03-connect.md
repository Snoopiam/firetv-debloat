# 3. Connect to the Fire TV

### 3.1 Connect
```bash
adb connect <FIRETV_IP>:5555
```
**Expected:** `connected to <FIRETV_IP>:5555`

### 3.2 Approve the prompt on the TV
The TV shows **"Allow USB debugging?"** with your computer's key fingerprint.
- Tick **Always allow from this computer**
- Select **OK**

### 3.3 Confirm
```bash
adb devices
```
**Expected:**
```
List of devices attached
<FIRETV_IP>:5555    device
```

| If you see | Do this |
|---|---|
| `unauthorized` | The TV prompt wasn't accepted. Look at the TV, approve it, run `adb devices` again. |
| `offline` | Wait 10 seconds and run `adb devices` again; if it stays offline, run `adb disconnect` then step 3.1 again. |
| `failed to connect` / `connection refused` | Check the IP, check ADB Debugging is ON, check both devices are on the same network. |

> **Note:** The rest of the guide uses plain `adb shell ...`. That works when exactly **one** device is connected. If `adb devices` lists more than one, add `-s <FIRETV_IP>:5555` after `adb` in every command (e.g. `adb -s 192.168.1.57:5555 shell ...`).

### ✅ Check
`adb devices` shows your Fire TV followed by the word **device**.

---
⬅️ [2. Platform tools](02-install-platform-tools.md) · ➡️ Next: [4. Snapshot](04-snapshot.md)
