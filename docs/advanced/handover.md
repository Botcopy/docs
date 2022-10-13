# Human Handover

Human Handover uses open endpoints for Botcopy to pause requests to Dialogflow and send them to a third party API of your choice. You may also send the message history.

The chat transcript with the bot can be sent so your livechat team can pick up the conversation with additional context.

## Scope of Work

Botcopy provides an open endpoint for you to connect an ITSM Tool like Genesys, Service Now, HP SM, etc. It is your responsibility to create the proxy between the ITSM tool and Botcopy's endpoint: `https://api.botcopy.com/webhooks/handover/push`

Here are the steps that proxy should follow:

1. Botcopy detects a trigger contexts (DialogFlow ES) or a session parameter (DialogFlow CX) configured on the Portal Connect page for each bot
2. Botcopy calls your Live Chat Endpoint Webhook and use its response to either pause the bot or keep the bot running.
   a. If your webhook returns `{ paused: true, minutesPaused: 10 }`, the bot is paused and any subsequent messages from the end-user will not be forwarded to Dialogflow and will trigger a call to your Live Chat Endpoint Webhook
   b. If your webhook returns `{ paused: false }`, the bot is not paused and any subsequent message from the end-user will be forwarded to Dialogflow
3. To send events ("message", "channel_update" or "user_typing") to your end-user make a POST request to  `https://api.botcopy.com/webhooks/handover/push` (more details below)

## Setup in the Botcopy Portal

The handover integration is set for each Botcopy bot on its Connect page in the Portal.
You may set your Live Chat Endpoint Webhook URL
If you're using Dialogflow ES set any lowercased trigger context you want like `bc-human-handover`
If you're using Dialoflow CX set `bcHumanHandover` as your trigger session parameter
Enable the storing of message history.
Generate your `organization access token`, that's used by Botcopy to authenticate you when you make a POST request to `https://api.botcopy.com/webhooks/handover/push`
Set a `bot access token` that you will use to authenticate Botcopy when Botcopy is making POST requests to your Live Chat Endpoint Webhook.

**Please note:** The `organization access token` created to view message history will only display once. If you lose this token, you must generate a new one.

Per request, Botcopy can ensure the message history complies with GDPR by storing conversations in our database and send the chat history on the initial webhook call.

## Webhook Integration

### Step One

**Case:** Bot not paused, but a defined trigger context has been detected

**Direction:** Botcopy -> Client

**Purpose:** Client receives a POST request to their Live Chat Endpoint Webhook when Botcopy detects a trigger output context(ES)/session parameter(CX) in a Dialogflow response.

```json5
{
  accessToken: "***", // bot access token
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

Client must respond with a JSON body `{ paused: true, minutesPaused: number}` within 5 seconds, otherwise the bot will not pause the conversation and ignore the response after the timeout. if `{paused: false}` is provided, the bot will not pause the conversation.

If the JSON body is received, the conversation is paused and messages from the chat user are sent to your middleware instead of Dialogflow.

If `minutesPaused` is not provided, the default is set to ten minutes.

When sending each message to the user, `paused` and `minutesPaused` should be a part of the payload that you send to Botcopy's endpoint: `https://api.botcopy.com/webhooks/handover/push`. To keep the conversation going make sure that `paused` is true and `minutesPaused` greater than 0. This way the session time expiry is pushed back each time a message is received from and sent to the end-user and you have a way to resume AI responses on your bot when you receive or send a message to the user.


### Step Two

**Case:** Bot already paused, chat user sends a message

**Direction:** Botcopy -> Client

**Purpose:** Client receives a webhook call when the bot is paused and the user sends a message.

Botcopy sends a request with the following JSON:

```json5
{
  accessToken: "***", // bot access token
  userId: "***",
  botId: "***",
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

Client must respond JSON body that includes `{ paused: true, minutesPaused: number }` within 5 seconds to continue pausing the bot. `minutesPaused` defaults to 10 minutes.

If Botcopy receives `{ paused: false }` or a response after five seconds, we forward the user message in the request to Dialogflow.

If you wish to end the conversation with the user in any of these 2 cases return the following instead:
`{ paused: false, minutesPaused: 0 }`

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
  accessToken: "***", // organization access token
  eventType: "message",
  userId: "***", // Botcopy`s user id
  botId: "***", // Botcopy`s bot id
  text: "Hi, how may I assist?", // text you want to send to user
  paused: true, // paused?
  minutesPaused: 5, // minutes to pause the bot, default 10
  agentProfile: {
    // info about the current live agent
    name: "Jane Doe", // name of the agent
    avatarUrl: "", // url to agent image
  },
}
```

### Updating the channel

**Resume Bot requests** to Dialogflow with this JSON body. If a name is provided, Botcopy displays `NAME has left the conversation. You are now chatting with the bot.`

Otherwise, Botcopy displays `The human has left the conversation. You are now chatting with the bot.`

```json5
{
  accessToken: "***", // organization access token
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

**Pause Bot requests** to Dialogflow with this JSON body:

```json5
{
  accessToken: "***", // organization access token
  eventType: "channel_update",
  userId: "***", // Botcopy`s user id
  botId: "***", // Botcopy`s bot id
  paused: true, // paused?
  minutesPaused: 5, // minutes to pause the bot, default 10
  agentProfile: {
    // info about the current live agent
    name: "Jane Doe", // name of the agent
    avatarUrl: "", // url to agent image
  },
}
```

### Show typing indicator

The typing indicator disappears after 10 seconds automatically, so you'll need to trigger the endpoint again if your agent is still typing after 9 seconds.

```json5
{
  accessToken: "***", // organization access token
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

When communicating from Client -> Botcopy, the `organization access token` is used.

When communicating from Botcopy -> Client, the `bot access token` is used.

`"Content-Type": "application/json"` is required in the header of the POST request to `https://api.botcopy.com/webhooks/handover/push`

