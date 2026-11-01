# Fire TV Debloat — Step-by-Step ADB Guide

A hands-on, copy-paste guide to switching off Amazon's ads, tracking and unused apps on a **Fire TV Stick 4K Max (1st gen, Fire OS 7)** using raw ADB commands. No root, no big automated script: you run one command at a time, see what it did, and keep an undo file for every change.

> **Tested on:** Fire TV Stick 4K, Fire OS 7.6.9.7 (PS7697/4800). Every command, expected output and "protected package" note in this guide comes from that real session.
> **Also for:** Fire TV Stick 4K Max (1st gen) — it runs the same Fire OS 7 family. Package lists can differ slightly between models and firmware, so step 4 has you check what's actually on *your* stick before changing anything.

## What this does — and doesn't do

| ✅ Does | ❌ Doesn't |
|---|---|
| Disables Amazon ad, tracking and telemetry services | Remove the sponsored tiles on Amazon's home screen (those come from Amazon's servers) |
| Disables Amazon apps you don't use | Free the space those apps take (they live on the read-only system partition) |
| Optionally disables Alexa, Live TV, screensaver, text-to-speech | Uninstall anything: every disable can be reversed |
| Optionally frees storage by clearing caches and leftover installers (**permanent**, see step 10) | Need root |

The debloat steps (5–7) use `pm disable-user`, which switches an app off for your user profile. One `pm enable` command switches it back on. The optional storage cleanup (step 10) is different: it deletes files, and that can't be undone.

## The guide

Work through these in order. Each page ends with a ✅ check so you know it worked before moving on.

| Step | Page | Time |
|---|---|---|
| 0 | [Before you start](guide/00-before-you-start.md) | 2 min |
| 1 | [Enable ADB debugging on the Fire TV](guide/01-enable-adb.md) | 2 min |
| 2 | [Install Google's platform tools (ADB) on your computer](guide/02-install-platform-tools.md) | 5 min |
| 3 | [Connect to the Fire TV](guide/03-connect.md) | 2 min |
| 4 | [Snapshot: list what's on your stick](guide/04-snapshot.md) | 3 min |
| 5 | [Group A — Ads, tracking & telemetry](guide/05-ads-and-telemetry.md) · *no problems on the tested device* | 5 min |
| 6 | [Group B — Unused Amazon apps](guide/06-unused-apps.md) · *skip what you use* | 10 min |
| 7 | [Group C — Features you might want to lose](guide/07-caution-features.md) · *removes features* | 5 min |
| 8 | [⛔ Do not touch](guide/08-do-not-touch.md) · *read this* | 2 min |
| 9 | [Reboot and verify](guide/09-reboot-and-verify.md) | 5 min |
| 10 | [Free up storage](guide/10-free-storage.md) · *optional, permanent* | 5 min |
| 11 | [Undo everything (or anything)](guide/11-undo.md) | — |
| 12 | [Troubleshooting & lessons learned](guide/12-troubleshooting.md) | — |

## Using an AI agent instead

If you'd like an AI assistant (Claude, ChatGPT, etc.) to walk you through it interactively — asking before every change, explaining each result, and saving your undo files — give it **[AGENT.md](AGENT.md)** along with this repo. It contains the rules and the exact flow the agent should follow.

## How undo works

In step 4 you save a list of what's **already** disabled on your stick. Before each group you save a "before" list, and after it you generate that group's undo file, which contains only the packages the group actually changed. Undoing therefore puts your stick back exactly as it was, and never switches on something that was off to begin with. Even if you lose the group files, [step 11](guide/11-undo.md) can rebuild a full undo from your step 4 baseline.

The saving and comparing is done by a few small helper commands you paste once ([step 4.0](guide/04-snapshot.md)). They only read from the stick, and they refuse to write anything if a file is missing or the stick can't be read, so a failed read can never turn into a wrong undo file.

No ready-made undo files are included in the repo on purpose: they couldn't know what was already disabled on *your* stick.

## ⚠️ Disclaimer

**Use this guide entirely at your own risk.** It is provided **"as is", without warranty of any kind**, express or implied. The author(s) and contributors are **not responsible or liable** for any damage, data loss, malfunction, bricked or unusable devices, voided warranties, lost services, account issues, or any other consequence of following, or failing to follow, any part of this guide, including the AI-agent instructions in [AGENT.md](AGENT.md). You alone are responsible for the commands you run on your devices.

- The steps were tested on one device and one firmware version. Amazon can change Fire OS at any time, and results on your device may differ.
- Disabling system components may affect features, updates or support from Amazon.
- This project is **not affiliated with, endorsed by, or sponsored by Amazon.com, Inc.** or its affiliates. Amazon, Fire TV, Fire OS, Alexa and Prime Video are trademarks of Amazon.com, Inc. or its affiliates. Other names belong to their respective owners.
- Nothing here is professional advice. If you're not comfortable running ADB commands, don't.

The debloat steps are designed to be reversible (undo files, no uninstalling). The optional storage cleanup isn't. A factory reset (Settings → My Fire TV → Reset to Factory Defaults) restores the stick to stock. But **by using this guide, you accept full responsibility for the outcome.** See [DISCLAIMER.md](DISCLAIMER.md) for the full text.
