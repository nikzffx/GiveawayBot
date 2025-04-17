# Advanced Configuration System

## Overview

The Giveaway Discord Bot comes with an advanced configuration system that allows you to customize various aspects of the bot's behavior without modifying the source code. This system provides a centralized place to manage settings and makes it easy to adapt the bot to different environments.

## Configuration File

The main configuration file is `config.json` at the root of the project. This file contains structured settings organized into logical sections.

## Environment Variables

The configuration system supports environment variables in two ways:
1. Environment variable templating in the config file using the `${ENV_VAR}` syntax
2. Direct overrides for specific settings

### Secrets and API Keys

For security, the following environment variables should be set:

- `DISCORD_TOKEN` - Required for the bot to connect to Discord
- `APPLICATION_ID` - Required for proper slash command functionality
- `LOG_WEBHOOK_URL` - Optional for logging events to a Discord webhook

## Configuration Sections

### Bot Settings
```json
"bot": {
  "prefix": "!",                                  // Command prefix for traditional commands
  "status_text": "🎉 giveawaybot-entry.onrender.com", // Status text shown in Discord
  "status_type": "custom",                        // Status type: custom, playing, watching, listening
  "intents": ["message_content", "reactions"]     // Discord intents to enable
}
```

### Discord Settings
```json
"discord": {
  "application_id": "${APPLICATION_ID}",          // Application ID from environment variable
  "sync_settings": {                              // Settings for slash command syncing
    "global_sync_cooldown": 300,                  // Seconds between global syncs
    "guild_sync_interval": 15,                    // Seconds between guild syncs
    "max_guilds_to_sync": 10,                     // Maximum guilds to sync at once
    "rate_limit_backoff": {                       // Backoff times when rate limited
      "global_sync": 3600,                        // 1 hour backoff for global sync
      "guild_sync": 900                           // 15 minute backoff for guild sync
    }
  }
}
```

### Giveaway Settings
```json
"giveaways": {
  "default_winners": 1,                           // Default number of winners
  "max_winners": 20,                              // Maximum winners allowed
  "min_duration": 60,                             // Minimum giveaway duration (seconds)
  "max_duration": 2592000,                        // Maximum duration (30 days in seconds)
  "check_interval": 30,                           // How often to check for ended giveaways
  "embed_color": "0x5865F2",                      // Default embed color (Discord blurple)
  "button_style": "2",                            // Button style (2 = primary blue)
  "button_text": "Enter Giveaway 🎉"              // Text shown on the enter button
}
```

### Webhook Settings
```json
"webhooks": {
  "log_events": {                                 // Control which events to log
    "guild_join": true,                           // Log when bot joins a server
    "guild_leave": true,                          // Log when bot leaves a server
    "giveaway_start": true,                       // Log when giveaway starts
    "giveaway_end": true,                         // Log when giveaway ends
    "error_reporting": true                       // Log errors 
  },
  "embed_colors": {                               // Colors for different event types
    "guild_join": "0x57F287",                     // Green
    "guild_leave": "0xED4245",                    // Red
    "giveaway_start": "0x5865F2",                 // Blurple
    "giveaway_end": "0xFEE75C",                   // Yellow
    "error": "0xED4245"                           // Red
  }
}
```

### Database Settings
```json
"database": {
  "type": "memory",                               // Storage type (memory or future: file, sqlite)
  "auto_backup": true,                            // Enable automatic backups
  "backup_interval": 3600,                        // Backup every hour (in seconds)
  "file_path": "data/giveaways.json"              // Where to save backups
}
```

### Web Server Settings
```json
"web": {
  "enabled": true,                                // Enable web server
  "host": "0.0.0.0",                              // Host to bind to
  "primary_port": 5000,                           // Primary port
  "fallback_port": 5001,                          // Fallback port if primary is in use
  "update_stats_interval": 30                     // How often to update stats (seconds)
}
```

### Time Settings
```json
"time": {
  "timezone": "UTC",                              // Timezone for dates
  "date_format": "%B %d, %Y %I:%M%p"              // Date formatting (strftime format)
}
```

### Hosting Settings
```json
"hosting": {
  "environment": "standard",                      // Environment type: standard or render
  "render_optimizations": {                       // Optimizations for Render hosting
    "enabled": false,                             // Enable Render optimizations
    "extended_cooldowns": true,                   // Use longer cooldowns
    "reduced_sync": true                          // Reduce command syncing frequency
  }
}
```

### Permissions Settings
```json
"permissions": {
  "required_for_commands": {                      // Required Discord permissions for commands
    "create": ["manage_guild", "manage_messages"],// Permissions to create giveaways
    "delete": ["manage_guild", "manage_messages"],// Permissions to delete giveaways
    "end": ["manage_guild", "manage_messages"],   // Permissions to end giveaways
    "reroll": ["manage_guild", "manage_messages"] // Permissions to reroll giveaways
  }
}
```

### Advanced Settings
```json
"advanced": {
  "debug_mode": false,                            // Enable debug mode
  "log_level": "INFO",                            // Logging level (DEBUG, INFO, WARNING, ERROR)
  "max_concurrent_tasks": 10,                     // Maximum concurrent tasks
  "command_timeout": 30                           // Command timeout in seconds
}
```

## Using the Config in Code

The configuration system is accessed through the `ConfigManager` class:

```python
from utils.config_manager import ConfigManager

config = ConfigManager()

# Get a value with dot notation, with a default
prefix = config.get('bot.prefix', '!')

# Get specialized settings
intents = config.get_intents()
sync_settings = config.get_sync_settings()
```

## Best Practices

1. Use environment variables for secrets, never store them in the config file
2. Use the config system instead of hardcoding values in the code
3. Create proper defaults so the bot works even without a config file
4. Use specialized config getters for complex objects (like Discord intents)
