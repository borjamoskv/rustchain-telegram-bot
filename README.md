# RustChain Telegram Bot (@RustChainBot)
*Built for Bounty Issue #2869*

A robust, rate-limited Telegram bot that lets users check their RustChain wallet and miner status in real-time.

## Features
- **Rate-Limiting (O(1) memory):** Enforces 1 request / 5 seconds per user.
- **Commands:**
  - `/balance <wallet>` - Check RTC balance.
  - `/miners` - List active miners (queries `bottube.ai/api/beacon/directory`).
  - `/epoch` - Current epoch info and network Hashrate.
  - `/price` - Show RTC reference rate ($0.10).
  - `/help` - Show commands.

## Deployment Instructions

### 1. Railway / Fly.io (Docker / Node Setup)
If deploying to modern serverless or container hosts:
1. Fork or clone this repository.
2. In your Railway/Fly dashboard, set the Environment Variable: `BOT_TOKEN=<your_telegram_token_from_botfather>`
3. Set the deploy Start Command to: `node src/bot.js`
4. Deploy!

### 2. Systemd (Linux VPS)
If deploying on a bare-metal Linux node running Ubuntu/Debian:

**1. Create a `.env` file:**
In the project root, create a file named `.env` and add:
```
BOT_TOKEN=your_token_here
```

**2. Install dependencies:**
```bash
npm install
```

**3. Create the systemd service:**
```bash
sudo nano /etc/systemd/system/rustchain-bot.service
```

Add the following configuration (replace `/path/to/bot` with your actual path):
```ini
[Unit]
Description=RustChain Telegram Bot
After=network.target

[Service]
ExecStart=/usr/bin/node /path/to/bot/src/bot.js
WorkingDirectory=/path/to/bot
Restart=always
User=root
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
```

**4. Start the service:**
```bash
sudo systemctl daemon-reload
sudo systemctl enable rustchain-bot
sudo systemctl start rustchain-bot
```

### Dependencies
- `telegraf`
- `dotenv`
