<p align="center">
  <a href="GUIDE.en.md"><strong>🇬🇧 English</strong></a> •
  <a href="GUIDE.az.md">🇦🇿 Azərbaycanca</a> •
  <a href="GUIDE.tr.md">🇹🇷 Türkçe</a>
</p>

<p align="center"><a href="README.en.md">← Back to About</a></p>

# Full Installation & Usage Guide

This guide is written so that **even if you know nothing about Termux**, you can follow it step by step from start to finish. Don't rush, go through each step in order.

> ⚠️ This server gives any AI connected to it **real bash shell access** to your device. Read the **Security** section below before sharing the link with anyone.

## Contents

1. [Install Termux](#1-install-termux)
2. [Download the project onto your phone](#2-download-the-project-onto-your-phone)
3. [Start the server](#3-start-the-server)
4. [Open a second window (session)](#4-open-a-second-window-session)
5. [Open a tunnel to the internet (cloudflared)](#5-open-a-tunnel-to-the-internet-cloudflared)
6. [Connecting an AI — Claude](#6-connecting-an-ai--claude)
7. [Connecting an AI — ChatGPT](#7-connecting-an-ai--chatgpt)
8. [Security & Capabilities](#8-security--capabilities)
9. [Disclaimer](#9-disclaimer)

---

## 1. Install Termux

⚠️ **DO NOT install Termux from the Google Play Store** — that version is outdated and no longer works properly. Only install from one of these sources:

- [F-Droid (recommended)](https://f-droid.org/packages/com.termux/)
- [GitHub Releases](https://github.com/termux/termux-app/releases)

If you don't have F-Droid installed, install the F-Droid app first (it's an app store itself), then search for and install Termux inside it. Or download the `.apk` directly from GitHub Releases and install it (your phone will ask you to allow "install from unknown sources" — accept that).

Once installed, open Termux. You'll see a black screen with text — that's the terminal, where you'll type commands.

---

## 2. Download the project onto your phone

Once Termux is open, type the following commands **one at a time**, pressing Enter after each (copy-paste works too):

```bash
pkg update && pkg upgrade -y
```
(This updates Termux's own packages. May take a moment, be patient.)

```bash
pkg install nodejs git -y
```
(This installs Node.js and git — the server won't run without them.)

```bash
git clone https://github.com/Alim4385/termux-mcp.git
```
(This copies the project from GitHub onto your phone.)

```bash
cd termux-mcp
```
(This moves you into the newly created folder.)

---

## 3. Start the server

```bash
npm start
```

You should see something like this on screen:

```
⚠️  BETA — Termux MCP Server
🌐 http://127.0.0.1:3000/mcp
📁 /data/data/com.termux/files/home/claude_workspace
```

If you see this, **the server is running** and this window needs to stay open. **Don't close this window** — the server will stop. Next, we'll open a second window so you can type more commands while the server keeps running.

---

## 4. Open a second window (session)

Without closing the window running the server, you can open a **new session** in Termux:

1. **Swipe from the left edge of the screen to the right** — this opens a hidden sidebar menu (drawer)
2. In the menu that opens, you'll see **"NEW SESSION"** — tap it
3. You now have a brand new, empty terminal window — the server is still running in the background in the first window

(If the drawer doesn't open, there may be a small arrow/hamburger icon in the top-left corner of the screen — tapping that opens the same menu.)

To switch between sessions, use the same swipe gesture — every session you've opened will show up in the list, tap one to switch to it.

---

## 5. Open a tunnel to the internet (cloudflared)

Now, in the **new second window** you just opened, type:

```bash
pkg install cloudflared -y
```

```bash
cloudflared tunnel --url http://127.0.0.1:3000
```

Wait a few seconds, and you'll see a line on screen like this:

```
https://random-words-here.trycloudflare.com
```

**Copy this link.** This is your phone's temporary address on the internet.

⚠️ **Don't close this window either** — closing it stops the tunnel and kills the link.

**Add `/mcp` to the end** of the link you copied. So:

```
https://random-words-here.trycloudflare.com/mcp
```

This is the full link you'll give to an AI. Paste it somewhere (a notes app, for example) so you don't lose it — this link **changes** every time you restart `cloudflared`.

---

## 6. Connecting an AI — Claude

**Requirement:** a claude.ai account (a free account works too, but you can only add 1 custom connector; Pro/Max have no limit).

1. Go to [claude.ai](https://claude.ai) (browser or mobile app)
2. From the left sidebar menu, go to **Settings → Connectors** (on mobile: tap your profile icon → **Settings** → **Connectors**)
3. Tap the **"+"** button
4. Select **"Add custom connector"**
5. In the dialog that opens:
   - **Name**: anything you like, e.g. `My Phone`
   - **URL**: paste the link you got in step 5 (ending in `/mcp`)
6. Tap **"Add"**

Once connected, you'll need to enable it per conversation:
1. In the chat screen, tap the **"+"** button in the lower-left corner
2. Select **"Connectors"**
3. Toggle on the connector you created

Now write something to Claude, like: *"Check what files are on my phone"* — Claude will connect to your server and run a bash command.

---

## 7. Connecting an AI — ChatGPT

**Requirement:** a ChatGPT Plus, Pro, Team, Enterprise, or Edu account. **This feature is not available on the Free plan.**

1. Go to [chatgpt.com](https://chatgpt.com)
2. Tap your profile icon (bottom-left/right corner) → **Settings**
3. Go to **Connectors** (or **Apps & Connectors**)
4. Scroll to **Advanced** → turn on **Developer mode** — you'll get a warning that "custom connectors run third-party code" — accept it
5. You'll now see an **"Add custom connector"** (or **"Create connector"**) button — tap it
6. Fill in:
   - **Name**: anything you like
   - **URL**: paste the link you got in step 5 (ending in `/mcp`)
   - **Authentication**: select `No authentication` (this server doesn't use a token)
7. Save

**Note:** adding the connector isn't enough — you need to enable it separately **in every new conversation**:
1. In the chat box, tap the **"+"** button
2. Select **"Developer mode"** (or **"More"**)
3. Find your connector in the list and toggle it on

Now write something to ChatGPT — it will connect to your server and run commands. Usually ChatGPT will ask you to confirm before each command — this is normal, you'll need to tap something like "Run".

---

## 8. Security & Capabilities

This server's only tool is `exec` — arbitrary bash execution. What that *actually* means for your device depends on what else is installed or enabled:

**If [Termux:API](https://wiki.termux.com/wiki/Termux:API) is installed** (`pkg install termux-api` + the Termux:API app), any AI connected to this server can, in principle, call commands like:
- `termux-sms-list` / `termux-sms-send` — read/send SMS
- `termux-contact-list` — read contacts
- `termux-camera-photo` — take a photo
- `termux-location` — get GPS location
- `termux-clipboard-get/set`, `termux-notification`, `termux-battery-status`, `termux-microphone-record`, etc.

Only install Termux:API if you're comfortable with a connected AI potentially being able to use these.

**If the device is rooted:** a rooted shell can modify system partitions, access other apps' private data, and change firewall rules. Running this server as root is **not recommended**.

**Regarding ADB:** ADB access isn't directly related to this server, but anyone with ADB access to your phone has close to full control (install/uninstall apps, pull files, mirror the screen). Never share ADB access alongside this server's tunnel link.

**About the tunnel link:** the `cloudflared` link has no password protection. Don't publish it (blur/black it out if you show it in screenshots or on TikTok), and know who has the link for as long as the server is up.

---

## 9. Disclaimer

This is beta software. The author takes **no responsibility** for data loss, device damage, unauthorized access, or any other consequence resulting from running this server, exposing it to a network, or actions taken by any AI or client connected to it. **You are responsible** for how you configure it and who you give access to. See [LICENSE](../LICENSE) for the full legal text. Use at your own risk.

## AI Safety Guide

If you're connecting an AI to this server, give it [`prompts/ai-usage-guide.txt`](../prompts/ai-usage-guide.txt) as part of its system instructions — it asks the AI to confirm with you before destructive actions.

<p align="center"><a href="README.en.md">← Back to About</a></p>
