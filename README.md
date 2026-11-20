<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=28&pause=1000&color=00C9A7&center=true&vCenter=true&width=600&lines=Telegram+PC+Bots;Monitor+Your+PC+Remotely;Written+in+Python+%F0%9F%90%8D" alt="Typing SVG" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Language-Python%203.10%2B-3776AB?style=for-the-badge" alt="Python" />
  <img src="https://img.shields.io/badge/Platform-Windows-blue?style=for-the-badge" alt="Platform" />
  <img src="https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge" alt="Status" />
</p>

# 🤖 Telegram PC Bots

## Overview

This repository brings together two lightweight **Python** programs (referred to as "bots" because they communicate through the **Telegram** messaging app) designed for anyone who wants to monitor or check on their PC when away from the keyboard.

**You do not need to be an experienced programmer to use them.** This guide walks you through everything step by step, explaining technical concepts in plain language. By following the steps in order, you can have both bots up and running in about 10–15 minutes.

| Bot | Folder | What It Does, in Plain English |
|---|---|---|
| 📡 **WiFi Monitor Bot** | `./wifi-monitor-bot` | Sends you a Telegram notification whenever a new device (phone, laptop, smart TV, etc.) connects to your home Wi-Fi network |
| 📸 **PC Screenshot Bot** | `./screenshot-bot` | Lets you send a command from your phone and immediately receive a picture of what is currently on your PC screen |

Both bots are independent of each other—you can run just one or both at the same time.

---

## Why Use a Telegram Bot?

Telegram provides a free **Bot API** that allows software scripts to send and receive messages automatically. It is a practical choice here because:

- **No extra apps required**: you use Telegram, which you likely already have on your phone.
- **Works from anywhere**: as long as both your PC and your phone have internet access, you can receive alerts or request screenshots remotely.
- **Completely free**: no subscriptions, paid tiers, or third-party cloud dashboards required.

---

## Detailed Explanation of Both Bots


<img width="781" height="887" alt="chat_censurata" src="https://github.com/user-attachments/assets/4e9a9f7f-2ac2-4671-a5e7-56ea1e9fa0c7" />

### 1. 📡 WiFi Monitor Bot (`./wifi-monitor-bot`)

**WiFi Monitor Bot** runs in the background on a Windows PC and watches your local network. Its main purpose is to **alert you on Telegram whenever a device that has not been seen before joins your Wi-Fi network**.

It is helpful if you want a simple way to notice when a guest, a neighbor with your Wi-Fi password, or an unrecognized device connects to your network without needing special router firmware or paid monitoring tools.

#### How It Works Under the Hood

The script relies on **Nmap**, a widely used open-source network scanning tool, to perform an **ARP scan**. In simple terms, it sends discovery probes across your local IP range (for example, from `192.168.1.1` to `192.168.1.254`) and checks which devices respond.

Every network-connected device (smartphone, computer, printer, smart TV, etc.) has a **MAC address**, which is a hardware identifier that generally stays constant—unlike an IP address, which your router may reassign over time. The bot uses the MAC address to tell known devices apart from new ones.

Each cycle follows three steps:

1. **Scan**: every 60 seconds (configurable), the script asks Nmap to scan the local network and report active hosts.
2. **Compare**: for each detected device, the script checks whether its MAC address already exists in `known_devices.json`, which acts as a persistent log of previously seen devices.
3. **Notify**: if a MAC address is not in `known_devices.json`, the bot saves it there (so it will not repeatedly alert you for the same device) and sends a Telegram message containing details such as IP address, MAC address, hostname (if available), vendor/manufacturer (e.g., Apple, Samsung, TP-Link), and a quick link to your router's admin page.

Because `known_devices.json` stays saved on disk across restarts, the **very first run** will notify you about all devices currently connected to your network. From that point onward, you will only get alerts when a previously unseen device appears.

---

<img width="858" height="452" alt="11111" src="https://github.com/user-attachments/assets/e10803bb-4efa-4f49-b208-60a4303da2b7" />



### 2. 📸 PC Screenshot Bot (`./screenshot-bot`)

**PC Screenshot Bot** runs on your PC and lets you view your screen remotely by sending a command from Telegram.

It is convenient when you want a quick visual check—such as seeing if a file download finished, checking if a script is still running, or verifying what is on screen—without setting up heavy remote desktop software just for a quick glance.

#### How It Works Under the Hood

Unlike a notification-only script, this bot needs to **receive commands** sent by you. It uses **long polling**: at short intervals (such as every 2 seconds), the script checks the Telegram Bot API to see if new messages have arrived.

When it receives the `/screenshot` command:

1. It captures the current screen image using `pyautogui`.
2. It encodes the capture as a PNG image in memory (without saving extra screenshot files onto your hard drive).
3. It sends the image directly as a photo in your Telegram chat.

#### Access Control

The bot checks incoming messages against a single **authorized Chat ID** (yours). If someone else somehow discovers your bot's username and sends `/screenshot`, the script ignores it because their Chat ID does not match the authorized one.

---

## What You Need Before Starting

- **A PC**:
  - **WiFi Monitor Bot**: developed and tested on **Windows** (may require minor adjustments on other operating systems).
  - **PC Screenshot Bot**: works on **Windows, Linux, or macOS**, provided there is an **active desktop session** (someone must be logged into the graphical desktop; running purely headless or without an active screen session prevents screen capture).
- **Python 3.10 or higher**: open *Command Prompt* (or your terminal) and run `python --version` to check if it is installed. If not, download it from [python.org](https://www.python.org/downloads/).
- **Nmap** (required for **WiFi Monitor Bot**): free and open source. On Windows, the official Nmap installer includes **Npcap**, which is needed for network scans.
- **A Telegram account**: your regular personal account works fine.

---

## Step-by-Step Setup Guide

### Step 1 — Install Python Dependencies

Open a terminal or *Command Prompt* inside the folder of the bot you want to set up (`wifi-monitor-bot` or `screenshot-bot`) and run:

pip install -r requirements.txt

- For **WiFi Monitor Bot**, this installs dependencies such as `python-nmap` (to invoke Nmap from Python) and `requests` (to communicate with the Telegram Bot API).
- For **PC Screenshot Bot**, this installs `pyautogui` (for screen capture), `pillow` (for image handling), and `requests` (for Telegram communication).

### Step 2 — Create Your Telegram Bot with @BotFather

Every Telegram bot needs a **token**—a secret authentication key generated by Telegram when the bot is created.

1. Open Telegram and search for **@BotFather** (Telegram's official bot for creating and managing bots).
2. Send the `/newbot` command.
3. Choose a **display name** for your bot (for example, `Home WiFi Monitor` or `PC Screen Viewer`).
4. Choose a unique **username** ending in `bot` (for example, `ExampleWifiMonitor_bot`).
5. Copy the **TOKEN** provided by @BotFather (it looks similar to `123456789:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw`).

> **Tip**: If you set up both bots, creating a **separate bot** for each one keeps their tokens, logs, and responsibilities cleanly separated.

> **Keep your token secret**: anyone who has your bot token can send messages on behalf of your bot and interact with its API.

### Step 3 — Find Your Telegram Chat ID

Your bot needs a **Chat ID**—the numeric ID of your private chat with the bot—so it knows where to send messages or which chat is authorized.

1. Search for your newly created bot in Telegram and open a chat with it.
2. Send any message to the bot (for example, `hello`).
3. Open your browser and visit the following URL, replacing `<TOKEN>` with your actual bot token:

https://api.telegram.org/bot<TOKEN>/getUpdates

4. In the JSON response shown in the browser, locate the `chat` object and note the number next to `"id"`:

"chat": { "id": 987654321, "first_name": "User", ... }

In this example, the Chat ID is `987654321`. If you see `"result":[]`, send another message to your bot in Telegram and refresh the page.

### Step 4 — Find Your Network Subnet and Router Address (WiFi Monitor Bot)

If you are setting up **WiFi Monitor Bot**, open *Command Prompt* on Windows and run:

ipconfig

Look under your active Wi-Fi adapter section and note two values:

- **IPv4 Address** (e.g., `192.168.1.15`): helps determine your local subnet, which is commonly `192.168.1.0/24`.
- **Default Gateway** (e.g., `192.168.1.1`): usually the address of your router's configuration page.

### Step 5 — Configure the Script

Open the bot's main Python file in any text editor and fill in the configuration variables at the top.

#### For `wifi_monitor_bot.py`:

TELEGRAM_BOT_TOKEN = ""   # token obtained from @BotFather
TELEGRAM_CHAT_ID = ""     # your personal chat_id (as a string)
NETWORK_RANGE = "192.168.1.0/24"   # your local subnet
SCAN_INTERVAL_SECONDS = 60         # scan interval in seconds
NMAP_PATH = r"C:\Program Files (x86)\Nmap\nmap.exe"   # path to nmap.exe
ROUTER_URL = "http://192.168.1.1"  # your router URL

- `TELEGRAM_BOT_TOKEN`: paste the token from Step 2.
- `TELEGRAM_CHAT_ID`: paste the numeric Chat ID from Step 3 inside quotes.
- `NETWORK_RANGE`: set your local subnet from Step 4.
- `NMAP_PATH`: verify the path to `nmap.exe` on your machine.
- `ROUTER_URL`: set your router gateway prefixed with `http://` (or `https://` if applicable).

#### For `screenshot_bot.py`:

TELEGRAM_BOT_TOKEN = ""     # token obtained from @BotFather
AUTHORIZED_CHAT_ID = ""     # only this chat_id is allowed to send commands

- `TELEGRAM_BOT_TOKEN`: paste the token from Step 2.
- `AUTHORIZED_CHAT_ID`: paste your numeric Chat ID from Step 3 inside quotes.

Save the file after editing.

### Step 6 — Run and Test the Bots

Open a terminal or *Command Prompt* in the respective folder and start the script:

python wifi_monitor_bot.py

or:

python screenshot_bot.py

- If `TELEGRAM_BOT_TOKEN` or the Chat ID is missing, the script will warn you and exit.
- For **WiFi Monitor Bot**, running *Command Prompt* as **Administrator** is recommended on Windows because ARP scanning requires elevated privileges. Within the first scan cycle, you should start receiving notifications.
- For **PC Screenshot Bot**, once running, the console may stay quiet while polling. Open your chat with the bot on Telegram and send:

/screenshot

Within a few seconds, you should receive a screenshot of your PC screen in the chat.

---

## Automatic Startup on Windows (Task Scheduler)

If you want either bot to start automatically without launching it manually every time, you can use **Task Scheduler** on Windows:

1. Open **Task Scheduler** from the Start menu.
2. Click **Create Task...** (not *Create Basic Task*).
3. On the **General** tab, enter a name for the task:
   - **WiFi Monitor Bot**: check **Run with highest privileges**, because Nmap ARP scanning on Windows requires administrator privileges.
   - **PC Screenshot Bot**: **do not** check **Run with highest privileges**, as screen capture needs to run within the regular logged-in user's desktop session.
4. On the **Triggers** tab, click **New...** and choose:
   - **At startup** for **WiFi Monitor Bot** (runs when the computer boots).
   - **At log on** for **PC Screenshot Bot** (runs when your user account logs into the desktop).
5. On the **Actions** tab, click **New...**, set **Action** to *Start a program*, enter the path to `python.exe` (you can check it by running `where python` in *Command Prompt*), put the script filename (`wifi_monitor_bot.py` or `screenshot_bot.py`) in **Add arguments**, and set the bot's folder path in **Start in**.
6. On the **Conditions** tab, uncheck *Start the task only if the computer is on AC power* if you do not want power state restrictions.
7. Click **OK** to save.

---

## Troubleshooting

### WiFi Monitor Bot

- **No Telegram messages arrive**: check `wifi_monitor_log.txt` in the bot folder to inspect scan activity and error messages.
- **Script stalls or fails to run Nmap**: verify that `NMAP_PATH` points to the actual `nmap.exe` binary on your system and that Npcap is installed.
- **Works in terminal, not via Task Scheduler**: confirm that **Run with highest privileges** is enabled and that **Start in** points to the script's folder.

### PC Screenshot Bot

- **No reply to `/screenshot`**: check `screenshot_bot_log.txt` in the bot folder for incoming commands or errors.
- **Screenshot comes back blank or black**: ensure the script is running inside an active desktop session (not elevated as a background service and not over a disconnected Remote Desktop session).
- **Bot does not respond at all**: double-check that `AUTHORIZED_CHAT_ID` matches your exact Telegram Chat ID from Step 3.

---

## ⚠️ Security Best Practices

Once you paste your real bot token into a `.py` file, that file contains a sensitive credential. Treat a bot token like a password.

- **Never commit real tokens or Chat IDs to Git**: keep the files pushed to GitHub with empty placeholders (`""`), and keep your real configuration only in your local working copy (or load secrets from environment variables / a git-ignored config file).
- **If a token is ever exposed**: revoke it immediately by messaging **@BotFather** with `/revoke` and generate a new token.
- **Keep bot usernames private when practical**: even though Chat ID verification blocks unauthorized commands, avoiding unnecessary public exposure of your bot username reduces unwanted traffic.

---

## 📄 License

This project is licensed under the **MIT License**:

- You are free to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the software.
- You must include the original copyright and license notice in copies or substantial portions of the software.
- The software is provided *"as is"*, without warranty of any kind, express or implied.

See the [`LICENSE`](./LICENSE) file in this repository for the full license text.
