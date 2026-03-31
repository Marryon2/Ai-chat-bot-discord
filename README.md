# Discord AI Private Thread Bot

A Discord bot that creates private AI chat threads for selected users on a server. Each user gets their own private thread, the bot replies automatically, daily usage is limited, and basic safety moderation is applied before requests are sent to the model.

## Features

- Private Discord threads created with `/start-ai`
- Automatic AI replies inside each private thread
- Access restricted to one Discord role or server admins
- Daily message limit per user
- Basic local moderation before requests reach the model
- Admin command to review daily usage
- PowerShell scripts for start, stop, and restart on Windows

## Tech Stack

- Node.js
- `discord.js`
- OpenRouter API
- Local JSON storage for daily usage tracking

## Prerequisites

Before you run the bot, make sure you have:

- Node.js 20 or newer installed
- A Discord application with a bot user
- A Discord server where you can invite the bot
- An OpenRouter API key

## 1. Create and Configure the Discord Bot

1. Open the Discord Developer Portal.
2. Create a new application.
3. Add a bot user to that application.
4. Copy these values for later:
   - `DISCORD_BOT_TOKEN`
   - `DISCORD_APPLICATION_ID`
5. In the bot settings, enable:
   - `Message Content Intent`
6. Invite the bot to your server.

The bot should have these permissions:

- View Channels
- Send Messages
- Read Message History
- Use Application Commands
- Create Private Threads
- Send Messages in Threads
- Manage Threads

## 2. Prepare Your Discord Server

Create or choose:

- One text channel where users will run `/start-ai`
- One role that is allowed to use the bot

Then collect these IDs:

- `DISCORD_GUILD_ID`
- `DISCORD_START_CHANNEL_ID`
- `DISCORD_ALLOWED_ROLE_ID`

To copy Discord IDs:

1. Open Discord settings.
2. Go to `Advanced`.
3. Enable `Developer Mode`.
4. Right-click the server, channel, or role and choose `Copy ID`.

## 3. Get an OpenRouter API Key

1. Create an account at OpenRouter.
2. Generate an API key in the OpenRouter dashboard.
3. Copy it into your environment file as `OPENROUTER_API_KEY`.

The default model in this project is:

```env
OPENROUTER_MODEL=openrouter/free
```

This alias points to a currently available free model, which makes the project easier to run on a fresh machine.

## 4. Clone the Project

```bash
git clone <your-repository-url>
cd <your-project-folder>
```

If you already have the project locally, just open a terminal in the project folder.

## 5. Install Dependencies

```bash
npm install
```

## 6. Create Your `.env` File

Copy the example file:

```powershell
Copy-Item .env.example .env
```

Then fill in your real values inside `.env`.

Example:

```env
DISCORD_BOT_TOKEN=your_new_discord_bot_token
DISCORD_APPLICATION_ID=your_discord_application_id
DISCORD_GUILD_ID=your_server_id
DISCORD_START_CHANNEL_ID=your_start_channel_id
DISCORD_ALLOWED_ROLE_ID=your_allowed_role_id

OPENROUTER_API_KEY=your_openrouter_api_key
OPENROUTER_MODEL=openrouter/free
OPENROUTER_SITE_URL=https://openrouter.ai/

DAILY_MESSAGE_LIMIT=15
THREAD_AUTO_ARCHIVE_MINUTES=1440
MAX_CONTEXT_MESSAGES=12

SYSTEM_PROMPT=You are a helpful AI assistant on a Discord server. Respond clearly, briefly, and safely. Refuse requests involving harmful, illegal, or NSFW content.
```

## 7. Start the Bot

Regular start:

```bash
npm start
```

The bot syncs its slash commands on startup.

## 8. Windows PowerShell Scripts

If you are running the bot on Windows, you can use these helper scripts:

```powershell
.\start-bot.ps1
.\stop-bot.ps1
.\restart-bot.ps1
```

These scripts use a PID file stored in `data/bot.pid`.

## Commands

- `/start-ai` - creates a private AI thread for the user
- `/limit` - shows the user's current daily usage
- `/end-chat` - deletes the current private AI thread
- `/admin-usage` - shows daily usage stats for admins

## Project Structure

- `src/index.js` - main bot logic, Discord events, thread creation, and AI flow
- `src/config.js` - environment loading and configuration validation
- `src/registerCommands.js` - slash command registration
- `src/services/ai.js` - OpenRouter API integration
- `src/services/moderation.js` - local safety filtering
- `src/services/usageStore.js` - daily usage storage in JSON
- `start-bot.ps1` - starts the bot
- `stop-bot.ps1` - stops the bot using the saved PID
- `restart-bot.ps1` - restarts the bot

## How It Works

1. A user runs `/start-ai` in the configured start channel.
2. The bot creates a private thread for that user.
3. The user sends a normal message inside the private thread.
4. The bot checks:
   - whether the user has access
   - whether the daily limit has been reached
   - whether the message passes moderation
5. The bot builds a short message context from the thread.
6. The bot sends that context to OpenRouter.
7. The AI response is posted back into the private thread.

## Safety Notes

- Never commit `.env` to GitHub
- Rotate your Discord bot token immediately if it is ever exposed
- Free OpenRouter models may change, rate-limit, or become temporarily unavailable
- The moderation layer in this project is basic and keyword-based

## Troubleshooting

### The bot logs in but does not answer

Check that:

- the message is sent inside a private AI thread
- the user has the allowed role
- `Message Content Intent` is enabled in the Discord Developer Portal

### The bot cannot reach the model

Check that:

- `OPENROUTER_API_KEY` is valid
- your selected model is available
- your internet connection is working

### Slash commands do not appear

Restart the bot after changing command definitions so it can sync them again.

## License

Add your preferred license here if you plan to publish the project publicly.
