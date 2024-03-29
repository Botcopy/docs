# Human Handover

Human Handover uses Botcopy's open endpoints to pause requests to Dialogflow and send them to a third party API of your choice.  This allows you to connect your bot to a live chat tool like Genesys, Service Now, HP SM, etc.

The conversation's message history can also be transmitted, enabling your live chat team to resume the conversation with added context.

## Scope of Work

Botcopy provides an open endpoint for you to connect an ITSM Tool. It is your responsibility to create the proxy between the ITSM tool and Botcopy's endpoint: `https://api.botcopy.com/webhooks/handover/push`

Here are the steps that proxy should follow:

1. Botcopy detects a trigger context (Dialogflow ES) or a session parameter (Dialogflow CX) configured on the Portal's Connect page for each bot
2. Botcopy calls your Live Chat Endpoint Webhook and uses the response to either pause the bot or continue the conversation.

   a. If your webhook returns `{ paused: true, minutesPaused: 10 }`, the bot is paused and any subsequent messages from the end-user will not be forwarded to Dialogflow and will trigger a call to your Live Chat Endpoint Webhook URL

   b. If your webhook returns `{ paused: false }`, the bot is not paused and any subsequent message from the end-user will be forwarded to Dialogflow

3. To send events ("message", "channel_update" or "user_typing") to your end-user make a POST request to `https://api.botcopy.com/webhooks/handover/push` (more details below)

## Setup in the Botcopy Portal

The handover integration for each bot is found on the Connect page in the Portal.

1.  Set a Live Chat Endpoint Webhook URL.

2.  - Dialogflow ES: set any lowercased trigger context you want like `bc-human-handover`.
    - Dialogflow CX: set `bcHumanHandover` as your trigger session parameter.

3.  Generate an `API Key`. Botcopy uses this to authenticate you when you make a POST request to `https://api.botcopy.com/webhooks/handover/push`.
4.  Set an `access token` that you will use to authenticate Botcopy when Botcopy makes POST requests to your Live Chat Endpoint Webhook URL.
5.  Enable the storing of message history.

**Please note:** The `API Key` created to view message history will only display once. If you lose this key, you must generate a new one.

Per request, Botcopy can ensure the message history complies with GDPR by storing conversations in our database and send the chat history on the initial webhook call.

## Webhook Integration

### Step One

**Case:** Bot not paused, but a defined trigger context has been detected

**Direction:** Botcopy -> Client

**Purpose:** Client receives a POST request to their Live Chat Endpoint Webhook URL when Botcopy detects a trigger output context(ES)/session parameter(CX) in a Dialogflow response.

If an additional custom header name & value are set, Botcopy will additionally send this header in the POST request.

```json5
"Authorization:  BOT_ACCESS_TOKEN"
"CustomHeaderName: CUSTOM_HEADER_VALUE"
```

```json5
{
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

If `minutesPaused` is not provided, the default is set to 10 minutes.

When sending each message to the user, `paused` and `minutesPaused` should be a part of the payload that you send to Botcopy's endpoint: `https://api.botcopy.com/webhooks/handover/push`. To keep the conversation going make sure that `paused` is true and `minutesPaused` greater than 0. This way the session time expiry is pushed back each time a message is received from and sent to the end-user and you have a way to resume AI responses on your bot when you receive or send a message to the user.

### Step Two

**Case:** Bot already paused, chat user sends a message

**Direction:** Botcopy -> Client

**Purpose:** Client receives a webhook call when the bot is paused and the user sends a message.

Botcopy sends a request with the following JSON:

```json5
"Authorization: BOT_ACCESS_TOKEN"
"CustomHeaderName: CUSTOM_HEADER_VALUE"
```

```json5
{
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

Send an Authorization header & JSON body to `https://api.botcopy.com/webhooks/handover/push`

```json5
"Content-Type: application/json"
"Authorization: YOUR_API_KEY"
```

```json5
{
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

**Resume Bot requests** to Dialogflow with the following Authorization header & JSON body. If a name is provided, Botcopy displays `NAME has left the conversation. You are now chatting with the bot.`

Otherwise, Botcopy displays `The human has left the conversation. You are now chatting with the bot.`

```json5
"Content-Type: application/json"
"Authorization: YOUR_API_KEY"
```

```json5
{
  eventType: "channel_update",
  userId: "***", // Botcopy`s user id
  botId: "***", // Botcopy`s bot id
  paused: false, // paused?
  unpauseChatEvent: "default" | "none" | "eventName" // event to dialogflow when bot requests are resumed
  agentProfile: {
    // info about the current live agent
    name: "Jane Doe", // name of the agent
    avatarUrl: "", // url to agent image
  },
}
```

`unpauseChatEvent` is optional, when not provided or set to `default`, the message `NAME has left the conversation. You are now chatting with the bot.` is pushed to the conversation when bot requests are resumed.  
If you prefer to not have this message displayed in the conversation when resuming requests, set `unpauseChatEvent` to `none`.  
If you want to dispatch a DialogFlow Event immediately after bot requests are resumed, set `unpauseChatEvent` to the name of your dialogflow event.


**Pause Bot requests** to Dialogflow with this Authorization header & JSON body:

```json5
"Content-Type: application/json"
"Authorization: YOUR_API_KEY"
```

```json5
{
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
"Content-Type: application/json"
"Authorization: YOUR_API_KEY"
```

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

When communicating from Client -> Botcopy, the `API Key` is used.

When communicating from Botcopy -> Client, the `access token` and `custom header/value` is used.

`"Content-Type": "application/json"` is required in the header of the POST request to `https://api.botcopy.com/webhooks/handover/push`
