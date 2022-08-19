# Human Handover

Human Handover uses open endpoints for Botcopy to pause requests to Dialogflow and send them to a third party API of your choice. You may also send the message history.

The chat transcript with the bot can be sent so your livechat team can pick up the conversation with additional context.

## Scope of Work

Botcopy provides an open endpoint for you to connect an ITSM Tool like Genesys, Service Now, HP SM, etc. It is your responsibility to create the proxy between the ITSM tool and Botcopy's endpoint: `https://api.botcopy.com/webhooks/handover/push`

Here are the steps that proxy should follow:

1. You receive a webhook call when Botcopy detects a specific context in dialogflow response, and when Botcopy bot is paused and user writes a message.
2. You can interact with the chat user with a `message`, `channel_update`, and `agent_typing`.
3. You will handle the socket towards the chosen ITSM tool.

## Setup in the Botcopy Portal

The handover integration is set for each individual Botcopy bot on its Connect page in the Portal. You may set the webhook URL, set contexts which will trigger the handover, and enable the storing of message history. This is also where you generate an access token to view your message history, and set an access token so the requests can authenticate with your webhook.

**Please note:** Access tokens created to view message history may only be saved once. If you lose this token, you must generate a new one.

Per request, Botcopy can ensure the message history complies with GDPR by storing conversations in our database and send the chat history on the initial webhook call.

## Webhook Integration

### Step One

**Case:** Bot not paused, but a defined trigger context has been detected

**Direction:** Botcopy -> Client

**Purpose:** You receive a webhook call when Botcopy detects a specific context in a Dialogflow response. You define a webhook URL pointing to your middleware, an access token to authenticate with your middleware, and a Dialogflow context name to trigger the webhook. When the context is detected, we send a `POST` request to your webhook with the following JSON body:

```json5
{
  accessToken: "***", // User defined access token created on the Connect page in the Botcopy Portal
  userId: "***",
  botId: "***",
  session: "projects/test***/agent/sessions/***-***-***-***-***",
  queryInput: {
    text: {
      text: "handover",
      languageCode: "en",
    },
  },
  queryParams: {
    contexts: [
      {
        name: "botcopy-timezone",
        lifespanCount: 10,
        parameters: {
          tz: "Europe/Vienna",
        },
      },
    ],
  },
  dialogflowResponse: {
    responseId: "***-***-***-***-***-***",
    queryResult: {
      fulfillmentMessages: [
        {
          platform: "PLATFORM_UNSPECIFIED",
          text: {
            text: ["human handover text"],
          },
          message: "text",
        },
      ],
      outputContexts: [
        {
          name: "handover-test",
          lifespanCount: 1,
          parameters: {},
        },
        {
          name: "botcopy-timezone",
          lifespanCount: 9,
          parameters: {
            tz: "Europe/Vienna",
          },
        },
      ],
      queryText: "handover",
      speechRecognitionConfidence: 0,
      action: "",
      parameters: {},
      allRequiredParamsPresent: true,
      fulfillmentText: "human handover text",
      webhookSource: "",
      webhookPayload: {},
      intent: {
        inputContextNames: [],
        events: [],
        trainingPhrases: [],
        outputContexts: [],
        parameters: [],
        messages: [],
        defaultResponsePlatforms: [],
        followupIntentInfo: [],
        name: "projects/test***/agent/intents/***-***-***-***-***",
        displayName: "Human Handover Context",
        priority: 0,
        isFallback: false,
        webhookState: "WEBHOOK_STATE_UNSPECIFIED",
        action: "",
        resetContexts: false,
        rootFollowupIntentName: "",
        parentFollowupIntentName: "",
        mlDisabled: false,
      },
      intentDetectionConfidence: 1,
      diagnosticInfo: null,
      languageCode: "en",
    },
    webhookStatus: null,
  },
}
```

You must respond with a JSON body `{ paused: true, minutesPaused: number}` within five seconds, otherwise the bot will not pause the conversation and ignore the response after the timeout. if `{paused: false}` is provided, the bot will not pause the conversation.

If the JSON body is received, the conversation is paused and messages from the chat user are sent to your middleware instead of Dialogflow.

If `minutesPaused` is not given, we default to ten minutes.

You can reference the specific Dialogflow session with the `session` key of the initial request.

### Step Two

**Case:** Bot already paused, chat user sends a message

**Direction:** Botcopy -> Client

**Purpose:** Client receives a webhook call when the bot is paused and the user sends a message.

Botcopy sends a request with the following JSON:

```json5
{
  accessToken: "***",
  userId: "***",
  botId: "***",
  session: "projects/test***/agent/sessions/***-***-***-***-***",
  queryInput: {
    text: {
      text: "help me please",
      languageCode: "en",
    },
  },
  queryParams: {
    contexts: [
      {
        name: "botcopy-timezone",
        lifespanCount: 10,
        parameters: {
          tz: "Europe/Vienna",
        },
      },
    ],
  },
}
```

You must respond with a JSON body of `{paused: true, minutesPaused: number}` within five seconds to continue pausing the bot. `minutesPaused` defaults to ten minutes.

If we receive `{paused: false}` or a response after five seconds, we forward the user message in the request to Dialogflow.

## API Integration

**Case:** Sending a message from your integration to a user.

**Direction:** Client -> Botcopy

**Purpose:** Client can interact with a Botcopy user.

The endpoint can handle three different event types:

1. message => send a message to a user
2. channel_update => update channel data like paused, avatar, agent name, etc.
3. user_typing => show the typing indicator

### Sending a message

Send a JSON body like this:

```json5
{
  accessToken: "***",
  eventType: "message",
  userId: "***", // Botcopy`s user id
  botId: "***", // Botcopy`s bot id
  text: "Hi, how may I assist?", // text you want to send to user
  paused: true, // paused?
  minutesPaused: 5, // how long should we pause the bot for, default 10
  agentProfile: {
    // info about the current live agent
    name: "Jane Doe", // name of the agent
    avatarUrl: "", // url to agent image
  },
}
```

### Updating the channel

**Resume Bot requests to Dialogflow** with this JSON body. If a name is provided, Botcopy displays `NAME has left the conversation. You are now chatting with the bot.`

Otherwise, Botcopy displays `The human has left the conversation. You are now chatting with the bot.`

```json5
{
  accessToken: "***",
  eventType: "channel_update",
  userId: "***", // Botcopy`s user id
  botId: "***", // Botcopy`s bot id
  paused: false, // paused?
  agentProfile: {
    // info about the current live agent
    name: "Jane Doe", // name of the agent
    avatarUrl: "", // url to agent image
  },
}
```

**Pause Bot requests to Dialogflow** with this JSON body:

```json5
{
  accessToken: "***",
  eventType: "channel_update",
  userId: "***", // Botcopy`s user id
  botId: "***", // Botcopy`s bot id
  paused: true, // paused?
  minutesPaused: 5, // how long should we pause the bot for, default 10
  agentProfile: {
    // info about the current live agent
    name: "Jane Doe", // name of the agent
    avatarUrl: "", // url to agent image
  },
}
```

### Show typing indicator

The typing indicator disappears after ten seconds automatically, so you'll need to trigger the endpoint again if your agent is still typing after nine seconds.

```json5
{
  accessToken: "***",
  eventType: "user_typing",
  userId: "***", // Botcopy`s user id
  botId: "***", // Botcopy`s bot id
  agentProfile: {
    // info about the current live agent
    name: "Jane Doe", // name of the agent
    avatarUrl: "", // url to agent image
  },
}
```

## Remarks

As an example, we have interfaced with Genesys and in Genesys Live Agent Chat we have these API’s available that we translate towards Botcopy:

**/ICWS/Web-Chat**

- Start
  - ParticipantID
  - ParticipantName
  - Language
  - Notes
  - ChatHistory
- Exit
  - Participant ID
- Reconnect
  - Participant ID
  - (Chat ID)
- Typing-State
  - Participant ID
- Send-Message
  - Participant ID
  - (ContentType = text/plain or text/html)
