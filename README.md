<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=28&pause=1000&color=00C9A7&center=true&vCenter=true&width=600&lines=Telegram+PC+Bots;Monitor+your+PC+remotely;Written+in+Python+%F0%9F%90%8D" alt="Typing SVG" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Author-Gabriele%20Tech-brightgreen?style=for-the-badge" alt="Author" />
  <img src="https://img.shields.io/badge/Platform-Windows-blue?style=for-the-badge" alt="Platform" />
  <img src="https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge" alt="Status" />
</p>

# 🤖 Telegram PC Bots

## What's in this repository

This project collects two small Python programs (we call them "bots", because they communicate through the Telegram app) designed for anyone who wants to keep an eye on, or control, their own PC even when they're not sitting in front of it.

**You don't need to be an expert programmer to use them**: this page walks you through everything step by step, explaining the technical concepts in simple terms. If you follow the instructions in order, you'll have your bot up and running in 10-15 minutes.

| Bot | What it does, in simple terms |
|---|---|
| 📡 [`wifi-monitor-bot`](./wifi-monitor-bot) | Sends you a Telegram message every time a new device (a phone, a PC, anything) connects to your home WiFi network |
| 📸 [`screenshot-bot`](./screenshot-bot) | Send a command from your phone and instantly get a photo of whatever is currently showing on your PC's screen |

Each bot has its own folder with a dedicated **README.md** file, explaining in detail how it works and how to configure it — click the links in the table above to open them.

## Why a "Telegram bot" and not another app?

Telegram provides a free system (called an "API") that lets a program send and receive messages as if it were a person. This is convenient because:
- You don't need to install any extra app: you use Telegram, which you probably already have on your phone
- You can receive notifications anywhere, as long as you have internet access
- It's completely free, both for you and for whoever writes the program

## What you need before starting

- A Windows PC that stays on (the bots need to run on a computer that remains powered on to be usable)
- **Python** installed (the programming language the bots are written in) — if you're not sure you have it, open Command Prompt and type `python --version`; if it's not recognized, download it for free from [python.org](https://www.python.org/downloads/)
- A Telegram account, even the one you already use normally

## Getting started

1. Choose which bot interests you most (you can install both, they're completely independent of each other)
2. Open the corresponding folder and follow its README.md, which contains all the detailed instructions
3. In both cases you'll need to create a "Telegram bot" through the official assistant **@BotFather** — the process is identical and explained in each README

## ⚠️ A note on security

Each bot needs a "token", a kind of secret password that Telegram gives you when you create the bot. This token must be entered directly inside each bot's Python file (precise instructions are in each README).

**Very important**: once you've entered your real token, that file becomes "personal" and should never be re-shared or re-published online as-is — otherwise anyone could control your bot in your place. The files in this repository, as you find them here, contain only empty placeholders instead of the token, specifically so they're safe to publish.

## 📄 License

This project is distributed under the **MIT License**, one of the most permissive and widely used open source licenses. In simple terms, this means:
- Anyone can freely use, copy, modify, and redistribute this code, even for commercial purposes
- The only condition is to keep the original license text (the [`LICENSE`](./LICENSE) file) in any copies or derivative versions
- The code is provided "as is", with no warranties: the author is not responsible for any issues arising from its use

For the full legal text, see the [`LICENSE`](./LICENSE) file in this repository.
