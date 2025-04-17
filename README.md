# Discord Giveaway Bot

A Python-based Discord bot that enables server administrators to create, manage, and customize giveaway events with flexible winner selection mechanisms and advanced interaction features.

## Features

- Create and manage giveaways in Discord servers
- Use both prefix commands (!) and Discord slash commands (/)
- Customizable giveaway duration and number of winners
- Interactive button-based entry system
- Preview mode for testing giveaway configurations
- Automatic winner selection and announcements
- Giveaway management (early end, reroll winners)
- Database tracking of giveaway entries and winners
- Built with discord.py and Flask

## Setup Instructions

### Prerequisites

1. A Discord account
2. A Discord bot (create one at [Discord Developer Portal](https://discord.com/developers/applications))
3. Python 3.8+ installed

### Environment Setup

1. Clone this repository
2. Set up the following environment variables:
   - `DISCORD_TOKEN` - Your Discord bot token (required)
   - `APPLICATION_ID` - Your Discord application ID (required for slash commands)
   - `SUPPORT_SERVER_LINK` - Your support server invite URL (optional)
   - `LOG_WEBHOOK_URL` - Discord webhook URL for logging server join/leave events (optional)

### Inviting the Bot to Your Server

1. Go to [Discord Developer Portal](https://discord.com/developers/applications)
2. Select your application
3. Go to OAuth2 > URL Generator
4. Select the following scopes:
   - `bot`
   - `applications.commands`
5. Select the following permissions:
   - `Send Messages`
   - `Embed Links`
   - `Read Message History`
   - `Add Reactions`
   - `Use External Emojis`
   - `Use Application Commands`
6. Copy and visit the generated URL to invite the bot to your server

### Running the Bot

To run the Discord bot:

```bash
python main.py
```

## Slash Commands Setup

For detailed instructions on setting up slash commands, please refer to [SLASH_COMMANDS_SETUP.md](SLASH_COMMANDS_SETUP.md)

## Project Structure

The project structure is organized as follows:

- `main.py` - Main entry point for running the Discord bot and web server
- `bot.py` - Core Discord bot implementation
- `cogs/giveaway.py` - Giveaway commands and functionality
- `utils/` - Utility functions for time parsing and giveaway management
- `models.py` - Database models for storing giveaway data

## Commands

### Prefix Commands
- `!gstart <time> <winners> <prize>` - Start a new giveaway
  - Example: `!gstart 24h 1 Discord Nitro`
- `!gend <message_id>` - End a giveaway early
- `!greroll <message_id> [winners_count]` - Reroll a giveaway to select new winners
- `!glist` - List all active giveaways in the server
- `!gpreview <time> <winners> <prize>` - Preview how a giveaway will look without creating it
- `!ginvite` - Get an invite link for the bot
- `!gsupport` - Join the support server for help
- `!gabout` - Display information about the bot
- `!ghelp` - Display help information for giveaway commands

### Slash Commands
- `/gcreate` - Start a giveaway using an easy-to-use form
- `/gstart <time> <winners> <prize>` - Start a new giveaway
- `/gend <message_id>` - End a giveaway early
- `/greroll <message_id> [winners_count]` - Reroll a giveaway to select new winners
- `/glist` - List all active giveaways in the server
- `/gpreview <time> <winners> <prize>` - Preview how a giveaway will look without creating it
- `/ginvite` - Get an invite link for the bot
- `/gsupport` - Join the support server for help
- `/gabout` - Display information about the bot
- `/ghelp` - Display help information for giveaway commands

## Time Format

When specifying time for giveaways, use the following format:
- `s` for seconds (e.g., `30s`)
- `m` for minutes (e.g., `5m`)
- `h` for hours (e.g., `2h`)
- `d` for days (e.g., `1d`)

You can combine them like: `1d12h30m` (1 day, 12 hours, and 30 minutes)

## Preview Mode

The giveaway preview mode lets server administrators test different giveaway configurations without actually creating a giveaway. This is useful for:

- Testing how different prizes, durations, and winner counts will look
- Training new moderators on how to use the giveaway system
- Planning future giveaways without committing to them

Preview messages automatically delete after 60 seconds to avoid confusion with real giveaways.

## Support Server

The bot provides easy access to a support server where users can:

- Get help with bot commands and setup
- Report bugs or request features
- Receive announcements about updates
- Connect with other bot users

Use the `!gsupport` command or `/gsupport` slash command to receive an invite link to the support server. To configure your own support server link, set the `SUPPORT_SERVER_LINK` environment variable with your Discord invite URL.

## Webhook Logging

The bot includes a webhook logging system that sends notifications to a designated Discord channel whenever it joins or leaves a server. This feature provides valuable insights for bot owners and administrators.

### Setting Up Webhook Logging

1. Create a webhook in your Discord server:
   - Go to Server Settings > Integrations > Webhooks
   - Click "New Webhook"
   - Customize the name and avatar if desired
   - Select the channel where you want the logs to appear
   - Copy the webhook URL

2. Add the webhook URL to your environment variables:
   - Set `LOG_WEBHOOK_URL` to the copied webhook URL

For detailed instructions and troubleshooting, please refer to [WEBHOOK_SETUP.md](WEBHOOK_SETUP.md)

### What Gets Logged

- **Server Joins**: When the bot is added to a new server, a detailed webhook message is sent including:
  - Server name and ID
  - Owner information
  - Member count
  - Server creation date
  - Server icon (if available)
  - Updated server count

- **Server Leaves**: When the bot is removed from a server, similar information is logged to help track bot usage and retention.

## Dependencies

- discord.py - Discord API library
- python-dotenv - Environment variable management
- Flask - Web framework (for keep-alive server)
- Flask-SQLAlchemy - Database ORM
- Gunicorn - WSGI HTTP Server
- aiohttp - Async HTTP client for webhook requests

## Troubleshooting

- **Slash commands not working?** Make sure you've set up your `APPLICATION_ID` correctly (see [SLASH_COMMANDS_SETUP.md](SLASH_COMMANDS_SETUP.md))
- **Bot not responding?** Check that your `DISCORD_TOKEN` is correct and the bot has proper permissions
- **Commands being rejected?** Ensure the bot has appropriate permissions in the channel
- **Giveaway button not working?** The bot might have restarted - giveaways created before a restart will need to be ended and recreated
- **Webhook logging not working?** Verify that your `LOG_WEBHOOK_URL` is correct and the webhook hasn't been deleted from the Discord channel
- **Port conflicts when running locally?** The bot will automatically try alternative ports if the default port (5000) is already in use

## Contributing

Contributions are welcome! Feel free to open issues or submit pull requests.
