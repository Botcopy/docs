# Session Management

Botcopy's session management system controls how long user conversations remain active and how chat history is handled when sessions expire. Understanding session behavior helps you provide a seamless experience for your users.

## Default Session Expiration

Botcopy follows the [Dialogflow default session expiration settings](https://cloud.google.com/dialogflow/cx/docs/concept/session ":target=_blank"). For Dialogflow ES, the default session timeout is **20 minutes** and cannot be changed due to Dialogflow limitations. For Dialogflow CX, the default session timeout is **30 minutes** and can be customized in your Dialogflow CX console. This means that if a user doesn't interact with your bot for the configured duration, their session will expire.

When a session expires:
- The conversation context is lost
- Any stored parameters or variables are cleared
- The next user message will start a new session

## Customizing Session Expiration

If you want to modify the session timeout duration, you can do so through your bot's **Session Management** page in the Botcopy Portal. This allows you to set a custom time limit that better fits your use case.

To access Session Management:
1. Navigate to your bot in the [Botcopy Portal](https://portal.botcopy.com/ ":target=_blank")
2. Go to the **Session Management** page
3. Adjust the session timeout to your preferred duration

## Session Expired Messages

When a session expires, users will see a default session expired message. You can customize this message to match your bot's tone and branding through your bot's **Localization** page.

To customize the session expired message:
1. Go to your bot's **Localization** page in the [Botcopy Portal](https://portal.botcopy.com/ ":target=_blank")
2. Find the session expired message setting
3. Update the text to your preferred message

## Chat History Behavior

When a session expires, the chat history behavior depends on your Session Management settings:

### Default Behavior
By default, when a session expires:
- Previous messages remain visible in the chat window
- Users can see their conversation history
- When the user sends a new message, a fresh session is initialized
- The bot starts with a clean context while preserving visual continuity

### Hide Chat History
If you enable the **"Hide chat history toggle"** on the Session Management page:
- Previous messages are cleared from the chat window when the session expires
- Users start with a completely clean chat interface
- No conversation history is visible from the expired session

This setting is useful when:
- Handling sensitive information that shouldn't persist visually
- You want to provide a completely fresh start for each session
- Compliance requirements dictate clearing visible conversation data

## Best Practices

- **Set appropriate timeouts**: Consider your users' typical interaction patterns when setting session timeouts
- **Clear communication**: Use custom session expired messages to explain what happened and how users can continue
- **Sensitive data**: Enable chat history clearing for bots handling personal or sensitive information