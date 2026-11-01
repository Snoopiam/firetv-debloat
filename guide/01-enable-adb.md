# 1. Enable ADB debugging on the Fire TV

ADB (Android Debug Bridge) lets your computer send commands to the Fire TV over your network. It's switched off by default and hidden behind Developer Options.

### 1.1 Unlock Developer Options
1. On the Fire TV, open **Settings → My Fire TV → About**.
2. Highlight the first item (your device name, e.g. **Fire TV Stick 4K Max**) and press **Select on the remote 7 times**.
3. A message says **"No need, you are already a developer"** or **"You are now a developer"**.

### 1.2 Turn on ADB debugging
1. Press **Back** once to return to **My Fire TV**.
2. Open **Developer Options**.
3. Turn **ADB Debugging** → **ON**. Confirm the warning.

### 1.3 Find the Fire TV's IP address
1. **Settings → My Fire TV → About → Network**.
2. Write down the **IP Address** (looks like `192.168.1.57`). This is your `<FIRETV_IP>`.

> **Tip:** If your router lets you, reserve this IP for the Fire TV (DHCP reservation) so it doesn't change next week.

> **Note:** If you use a VPN app on the Fire TV with auto-connect, consider turning auto-connect off while you work through the guide. It didn't block ADB in testing, but see [step 12](12-troubleshooting.md) for an unexplained interaction we saw around reboots.

### ✅ Check
- Developer Options shows **ADB Debugging: ON**
- You have the IP address written down

---
⬅️ [0. Before you start](00-before-you-start.md) · ➡️ Next: [2. Install platform tools](02-install-platform-tools.md)
