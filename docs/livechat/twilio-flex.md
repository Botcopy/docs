# Twilio Flex Live Chat

The Twilio Flex integration hands a conversation off from your Botcopy-powered bot to a human agent in your Twilio Flex contact center. When a trigger fires during a Dialogflow conversation, the bot pauses and the end user is routed to a Flex agent. When the agent ends the conversation, the widget resumes the Dialogflow session automatically.

This guide covers connecting Twilio Flex to a bot. To translate messages between users and agents automatically, see [Twilio Flex Translation](livechat/twilio-flex-translation.md) after you finish the setup below.

## Before You Begin

You will need:

1. A Twilio Flex account with TaskRouter and Conversations configured.
2. Admin access to your [Botcopy Portal](https://portal.botcopy.com).
3. The following identifiers from your Twilio Console (see the next section for where each lives).

### Twilio identifiers to gather

| Value | Where to find it in Twilio |
| --- | --- |
| Account SID (`AC...`) | Console home / Account Info |
| API Key SID (`SK...`) and Secret | Console → Account → API keys & tokens (create a new key) |
| Workspace SID (`WS...`) | TaskRouter → Workspaces |
| Workflow SID (`WW...`) | TaskRouter → Workspace → Workflows |
| Conversations Service SID (`IS...`) | Conversations → Services |

> The API Key Secret is shown by Twilio only once when you create the key. Copy it before leaving the page. Botcopy stores it encrypted and never displays it again after you save.

## Step 1: Create Twilio Flex Credentials in the Portal

Credentials are managed at the organization level, so they can be reused across bots.

1. In the [Portal](https://portal.botcopy.com), open **Account Settings** and find the **Twilio Flex** section.
2. Add a new credential set and fill in:
   - **Name** (a label such as "Production")
   - **Account SID**
   - **API Key SID** and **API Key Secret**
   - **Workspace SID**
   - **Workflow SID**
   - **Conversations Service SID**
3. Save. Botcopy generates a unique webhook token for this credential and shows you two webhook URLs to configure in Twilio (next step).

## Step 2: Configure the Webhooks in Twilio

Botcopy receives Twilio events at two URLs. Both contain the webhook token generated for your credential, so copy them from the Portal exactly as shown.

### Conversations post-event webhook

```
https://api.botcopy.com/twilio/webhook/conversations/{webhookToken}
```

Configure in **Twilio Console → Conversations → your Service → Webhooks → Post-Event URL**:

- Method: HTTP POST
- Events:
  - `onMessageAdded`
  - `onConversationStateUpdated`

### TaskRouter event callback

```
https://api.botcopy.com/twilio/webhook/taskrouter/{webhookToken}
```

Configure in **Twilio Console → TaskRouter → your Workspace → Settings → Event Callback URL**:

- Events:
  - `reservation.accepted`
  - `task.completed`
  - `task.wrapup`
  - `task.canceled`

## Step 3: Assign the Credential to a Bot

1. In the Portal, open the bot and go to the **Integrations** page.
2. In the **Twilio Flex** section, select the credential set you created in Step 1 from the dropdown.
3. Save.

## Step 4: Configure the Dialogflow Trigger

Set the trigger on the intent, page, or route in your Dialogflow agent where the handover should occur. Botcopy looks for `bcTwilioFlexLivechat`:

- **Dialogflow CX:** set a session parameter `bcTwilioFlexLivechat` with a value of `true`.
- **Dialogflow ES:** set an output context named `bcTwilioFlexLivechat`.

When Botcopy detects this trigger in a Dialogflow response, it creates a Twilio Conversation, routes the end user to an agent through your TaskRouter workflow, and pauses the bot.

For routing to specific departments or queues, configure your TaskRouter workflow in Twilio. See [Botcopy Live Chat](livechat/botcopylc.md) for general handover concepts.

## What Happens During a Handover

- When the conversation is created, the agent receives a notice that a customer connected from the Botcopy widget, along with the conversation context.
- When an agent accepts the task, the widget shows that the user is now chatting with an agent (including the agent name and avatar when available).
- Typing indicators are exchanged in both directions in real time.
- When the agent completes, wraps up, or cancels the task (or the conversation is closed), the bot resumes and the Dialogflow session continues.

While an agent is connected the bot stays paused. The pause is extended automatically as the conversation continues, so it will not time out mid-chat.

## Security Notes

- Each credential has a unique webhook token that Botcopy validates on every inbound Twilio event, and the Account SID is cross-checked against the credential.
- The API Key Secret is write-only: stored encrypted and never returned after you save it.
- A credential cannot be deleted while a bot still references it. Remove it from the bot's Integrations page first.

## Next Steps

- Enable automatic translation between users and agents: [Twilio Flex Translation](livechat/twilio-flex-translation.md).
- Review general live chat behavior and departments: [Botcopy Live Chat](livechat/botcopylc.md).
