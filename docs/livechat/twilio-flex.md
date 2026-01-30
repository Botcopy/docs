# Twilio Flex Integration

> **Early Access**: This feature is currently in early access. To enable Twilio Flex integration for your organization, please contact [sales@botcopy.com](mailto:sales@botcopy.com).

Botcopy integrates with [Twilio Flex](https://www.twilio.com/flex) to provide seamless live agent handover. When a customer needs human assistance, the conversation is routed to your Twilio Flex contact center where agents can take over and communicate directly with the customer through the Botcopy widget.

## Features

- **Seamless Handover**: Automatically route conversations to Twilio Flex when human assistance is needed
- **Bidirectional Messaging**: Messages flow between the Botcopy widget and Twilio Flex dashboard in real-time
- **Typing Indicators**: Both agents and customers can see when the other party is typing
- **Agent Information**: Display agent name and avatar in the widget when an agent joins
- **Conversation History**: Message history is preserved and visible to agents for context

## Prerequisites

Before setting up the integration, you'll need:

1. A [Twilio account](https://www.twilio.com/try-twilio) with Flex enabled
2. Access to your Twilio Console
3. A Botcopy account with portal access

## Setup

### Step 1: Create Twilio API Credentials

1. Log in to your [Twilio Console](https://console.twilio.com/)
2. Navigate to **Account** > **API keys & tokens**
3. Click **Create API Key**
4. Select **Standard** key type
5. Give your key a friendly name (e.g., "Botcopy Integration")
6. Click **Create API Key**
7. **Important**: Save the **SID** and **Secret** - the secret is only shown once

You'll also need your **Account SID**, found on the Twilio Console dashboard.

### Step 2: Get TaskRouter Workspace Details

1. In Twilio Console, navigate to **TaskRouter** > **Workspaces**
2. Select or create a workspace for Botcopy conversations
3. Note the **Workspace SID** (starts with `WS`)
4. Navigate to **Workflows** within your workspace
5. Note the **Workflow SID** (starts with `WW`) for routing Botcopy tasks

### Step 3: Get Conversations Service Details

1. In Twilio Console, navigate to **Conversations** > **Services**
2. Select or create a Conversations service for Botcopy
3. Note the **Service SID** (starts with `IS`)
4. Note the **Service Name** for reference

### Step 4: Configure Botcopy Portal

1. Log in to the [Botcopy Portal](https://portal.botcopy.com)
2. Navigate to **Account Settings**
3. Expand the **Twilio Flex Settings** accordion

#### API Credentials

Enter your Twilio credentials:

| Field | Description | Format |
|-------|-------------|--------|
| Account SID | Your Twilio account identifier | `ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |
| API Key SID | The SID of your API key | `SKxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |
| API Key Secret | The secret for your API key | (hidden after save) |

Click **Save Credentials** to store your API credentials securely.

#### Workspace

Add your TaskRouter workspace (one workspace per organization):

| Field | Description | Format |
|-------|-------------|--------|
| Workspace Name | A friendly name for reference | e.g., "Support Team" |
| Workspace SID | Your TaskRouter workspace identifier | `WSxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |
| Default Workflow SID | The workflow for routing tasks | `WWxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |

Click **Add Workspace** to save.

#### Conversations Service

Configure your Twilio Conversations service:

| Field | Description | Format |
|-------|-------------|--------|
| Service Name | A friendly name for reference | e.g., "Botcopy Conversations" |
| Conversations Service SID | Your Conversations service identifier | `ISxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |

Click **Add Service** to save.

### Step 5: Configure Twilio Webhooks

Configure Twilio to send events to Botcopy by setting up webhooks in your Twilio Console.

#### Conversations Webhook

1. Navigate to **Conversations** > **Services** in Twilio Console
2. Select your Conversations service
3. Go to **Webhooks**
4. Set the **Post-Event URL** to:
   ```
   https://api.botcopy.com/twilio/webhook/conversations
   ```
5. Enable the following events:
   - `onMessageAdded`
   - `onConversationStateUpdated`

#### TaskRouter Webhook

1. Navigate to **TaskRouter** > **Workspaces**
2. Select your workspace
3. Go to **Settings**
4. Set the **Event Callback URL** to:
   ```
   https://api.botcopy.com/twilio/webhook/taskrouter
   ```
5. Enable the following events:
   - `reservation.accepted`
   - `task.completed`
   - `task.wrapup`
   - `task.canceled`

### Step 6: Configure Bot Integration

Each bot inherits your organization's Twilio Flex workspace configuration.

1. In the Botcopy Portal, navigate to your bot's **Integrations** page
2. Under **Twilio Flex**, you will see:
   - **Workspace**: Displays your organization's configured workspace (read-only)
3. Optionally, override the **Workflow SID** if this bot should use a different workflow than the workspace default
4. Click **Save**

### Step 7: Configure Handover Trigger

Configure when the bot should hand over to Twilio Flex:

1. Navigate to your bot's **Connect** page
2. Under **Handover Integration**, configure the trigger:
   - **Dialogflow ES**: Set a trigger context (e.g., `bc-human-handover`)
   - **Dialogflow CX**: Set `bcHumanHandover` as a session parameter

When this trigger is detected in a Dialogflow response, the conversation will be routed to Twilio Flex.

## How It Works

### Conversation Flow

1. **User requests human assistance**: The bot detects the handover trigger context/parameter
2. **Task created**: Botcopy creates a Twilio Flex task and routes it to your workflow
3. **Agent accepts**: An agent accepts the task in Twilio Flex
4. **Live chat begins**: The widget displays "You are now chatting with [Agent Name]"
5. **Bidirectional messaging**: Messages flow between widget and Flex dashboard
6. **Conversation ends**: Agent completes the task, bot resumes control

### Typing Indicators

The integration supports real-time typing indicators in both directions:

- **Agent typing**: When an agent types in Twilio Flex, the customer sees a typing indicator in the widget
- **Customer typing**: When a customer types in the widget, the agent sees a typing indicator in Twilio Flex

> **Technical Note**: Twilio typing indicators require a direct SDK connection. The widget automatically establishes this connection when a Twilio Flex handover begins.

### Agent Information

When an agent accepts a conversation:

- The agent's name is displayed in the widget snackbar
- The agent's avatar (if configured in Twilio Flex) is shown next to their messages

## Troubleshooting

### Messages not appearing in widget

1. Verify your Conversations webhook URL is correct
2. Check that `onMessageAdded` event is enabled
3. Ensure your API credentials are valid

### Agent join/leave not detected

1. Verify your TaskRouter webhook URL is correct
2. Check that `reservation.accepted` and `task.completed` events are enabled

### Typing indicators not working

1. Ensure the widget has successfully connected to the Twilio SDK
2. Check browser console for any connection errors
3. Verify the Conversations service SID is correctly configured

### Tasks not being created

1. Verify your Workspace SID and Workflow SID are correct
2. Check that your API credentials have the necessary permissions
3. Ensure the workflow has available workers

### Conversations Service issues

1. Verify your Conversations Service SID is correctly configured in Account Settings
2. Ensure the service exists and is active in your Twilio Console
3. Check that webhooks are configured on the correct Conversations service

## Security

- API credentials are stored securely and encrypted at rest
- API Key Secrets are never displayed after initial save
- All communication between Botcopy and Twilio uses HTTPS
- Webhook signatures are validated to ensure requests originate from Twilio

## Limitations

- Typing indicators require the widget to maintain an active connection to Twilio's SDK
- File attachments sent through Twilio Flex are not currently supported in the widget
- Rich message formats (cards, carousels) are not supported during live chat - only text messages
