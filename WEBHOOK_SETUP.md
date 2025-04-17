# Discord Webhook Setup Guide

This guide explains how to set up and configure webhook logging for the bot to track server join and leave events.

## What are Webhooks?

Discord webhooks are a way to send automated messages to Discord channels from external services. The bot uses webhooks to send notifications when it joins or leaves a server, providing you with valuable tracking information.

## Setting Up a Webhook

1. **Create a Webhook in Discord**:
   - Open Discord and go to the server where you want to receive logging notifications
   - Go to Server Settings > Integrations > Webhooks
   - Click "New Webhook"
   - Customize the webhook name (e.g., "Giveaway Bot Logs") and avatar if desired
   - Select the channel where you want log messages to appear (a private admin channel is recommended)
   - Click "Copy Webhook URL" to copy the webhook URL to your clipboard

2. **Configure the Bot**:
   - Add the webhook URL to your environment variables by setting `LOG_WEBHOOK_URL` to the copied URL
   - For Replit: Add the secret by going to "Secrets" in the Replit sidebar and adding a new secret with key `LOG_WEBHOOK_URL` and value as your webhook URL
   - For local development: Add the URL to your `.env` file as `LOG_WEBHOOK_URL=your_webhook_url_here`

## Sample Webhook Messages

### Server Join Notification

When the bot joins a new server, a webhook message will be sent with the following information:

- Server name and ID
- Server owner (username and ID)
- Member count
- Server creation date
- Server icon (if available)
- Updated total server count

### Server Leave Notification

When the bot leaves or is removed from a server, a webhook message will be sent with similar information.

## Testing Webhook Functionality

The bot comes with a testing utility to verify your webhook configuration is working properly:

1. **Using the test script**:
   ```bash
   ./test_webhooks.sh
   ```
   This script will send test notifications to your configured webhook URL to simulate both server join and server leave events.

2. **Manual testing**:
   You can also run the Python test script directly:
   ```bash
   python test_webhook.py
   ```

3. **Testing in development**:
   - The test utility creates mock guild objects to simulate join/leave events
   - You'll see test notifications appear in your configured webhook channel
   - These test messages are clearly marked as test events

## Troubleshooting

If webhook logging isn't working:

1. **Check the webhook URL**: Ensure the `LOG_WEBHOOK_URL` environment variable is set correctly
2. **Verify webhook existence**: Make sure the webhook hasn't been deleted from the Discord channel
3. **Check permissions**: Ensure the webhook has permission to send messages in the chosen channel
4. **Review logs**: Check the bot logs for any webhook-related errors
5. **Run the test script**: Use `./test_webhooks.sh` to verify if your webhook configuration works

## Privacy Considerations

The webhook logging captures basic server information but does not collect personal user data besides the server owner's username and ID, which is necessary for proper server management. This aligns with Discord's Terms of Service and does not violate user privacy.

## Advanced Configuration

For advanced users, you can modify the webhook messages by editing the `utils/webhook_logger.py` file:

- Customize message formatting and content
- Add additional fields to the embeds
- Change the colors of the webhook embeds
- Implement additional webhook notifications for other events

## Security Considerations

Keep your webhook URL private, as anyone with access to it can send messages to your Discord channel. Do not share your `.env` file or expose your environment variables publicly.
