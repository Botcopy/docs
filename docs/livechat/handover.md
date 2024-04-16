# Live Chat

Botcopy provides an endpoint to pause requests to Dialogflow and send user messages to a third party API of your choice.  This allows you to connect your bot to a live chat tool like Genesys, Service Now, HP SM, etc.

The conversation's message history can also be transmitted, enabling your live chat team to resume the conversation with added context.

## Scope of Work

Botcopy provides this open endpoint for you to connect an ITSM Tool. It is your responsibility to create the proxy between the ITSM tool and Botcopy's endpoint: `https://api.botcopy.com/webhooks/handover/push`

Here are the steps that proxy should follow:

1. Botcopy detects a trigger context (Dialogflow ES) or a session parameter (Dialogflow CX) configured on the <a href="https://portal.botcopy.com" target="_blank">Portal's</a> Integrations page for each bot

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
"Content-Type": "application/json",
"Authorization":  "BOT_ACCESS_TOKEN",
"CustomHeaderName: CUSTOM_HEADER_VALUE"
```

```json5
{
  "accessToken": string, // specified on the portal's Integrations page, to verify that this webhook call originates from Botcopy
  "userId": string, // identifies the Customer
  "botId": string, // identifies the bot invoking livechat
  "sessionId": string, // identfies a session for a unique user of the bot
  "dialogflowResponse": {}, // unchanged response from Dialogflow detectIntent as described by Google Documentation
  // https://cloud.google.com/dialogflow/es/docs/reference/rest/v2/projects.agent.environments.users.sessions/detectIntent
  // https://cloud.google.com/dialogflow/cx/docs/reference/rest/v3/DetectIntentResponse
  "messageHistory": Array of MessageHistory Items, 
  "session": string, // dialogflow session id
  "queryInput": {
    "text": {
      "text": string, // the customer message sent to Live Chat
        "languageCode": string, // the code for the customer language (e.g., 'en' for English)
      }
    },
  "queryParams": {
    "contexts": []
  }
}
```

Client must respond with a JSON body `{ paused: true, minutesPaused: number}` within 5 seconds, otherwise the bot will not pause the conversation and ignore the response after the timeout. if `{paused: false}` is provided, the bot will not pause the conversation.

If `minutesPaused` is not provided, the default is set to 10 minutes.

When sending each message to the user, `paused` and `minutesPaused` should be a part of the payload that you send to Botcopy's endpoint: `https://api.botcopy.com/webhooks/handover/push`. To keep the conversation going make sure that `paused` is true and `minutesPaused` greater than 0. This way the session time expiry is pushed back each time a message is received from and sent to the end-user and you have a way to resume AI responses on your bot when you receive or send a message to the user.

Botcopy will send the [messageHistory](livechat/handover?id=message-history) array to the Live Chat Endpoint Webhook URL when the conversation is paused. This array contains the conversation history between the user and the bot.

### Step Two

**Case:** Bot already paused, chat user sends a message

**Direction:** Botcopy -> Client

**Purpose:** Client receives a webhook call when the bot is paused and the user sends a message.

Botcopy sends a request with the following JSON:

```json5
"Content-Type": "application/json",
"Authorization": "BOT_ACCESS_TOKEN",
"CustomHeaderName": "CUSTOM_HEADER_VALUE"
```

```json5
{
    "accessToken": string, // specified on the portal Integrations page, to verify that this webhook call originates from Botcopy
    "userId": string, // identifies the Customer
    "botId": string, // identifies the bot invoking livechat
    "sessionId": string, // identfies a session for a unique user of the bot
    "session": string, // dialogflow session id
    "queryInput": {
        "text": {
            "text": string, // the customer message sent to Live Chat
            "languageCode": string, // the code for the customer language (e.g., 'en' for English)
        }
    },
    "queryParams": {
        "contexts": [],
        "payload": {},
        "webhookHeaders": {}
    }
}
```

If Botcopy receives `{ paused: false }` or a response after five seconds, we forward the user message in the request to Dialogflow.

If you wish to end the conversation with the user in any of these 2 cases return the following instead:
`{ paused: false, minutesPaused: 0 }`

## API Integration

Botcopy offers an endpoint that you can use to communicate with the user while in a Live Chat conversation.

**Case:** Sending a message from your integration to a user.

**Direction:** Client -> Botcopy

**Purpose:** Client can interact with a Botcopy user.

The endpoint can handle three different event types:

1. message => send a message to a user
2. channel_update => update channel data like paused, avatar, agent name, etc.
3. user_typing => show the typing indicator

### Sending a message

This payload is used to send messages from the Live Agent to the Customer

Send an Authorization header & JSON body to `https://api.botcopy.com/webhooks/handover/push`

```json5
"Content-Type": "application/json",
"Authorization": "YOUR_API_KEY"
```

```json5
{
 "eventType": "message",
 "userId": string, // identifies the Customer
 "botId": string, // identifies the bot invoking livechat
 "paused": true,
 "minutesPaused": number, // maximum idle time for the Live Chat conversation
 "text": string, // the Live Agent message
}
```

### Updating the channel

#### Pause Bot requests

Setting `paused` to `true` is used to visually indicate to the Customer that they are now in a communication with a Live Agent.

Displays `You are now chatting with an Agent.`


```json5
"Content-Type": "application/json",
"Authorization": "YOUR_API_KEY"
```

```json5
{
 "eventType": "channel_update",
 "userId": string, // identifies the Customer
 "botId": string, // identifies the bot invoking livechat
 "paused": true,
 "minutesPaused": number, // maximum idle time for the Live Chat conversation
 "agentProfile": {
   "name": string, // customize the name of the agent in the snackbar
   "avatarUrl": string, // url of agent image
 }
}
```


#### Resume Bot requests

Set `paused` to `false` to resume bot requests.

If a name is provided, Botcopy displays `NAME has left the conversation. You are now chatting with the bot.` 

Otherwise, Botcopy displays `The human has left the conversation. You are now chatting with the bot.` to the end user.

```json5
"Content-Type": "application/json",
"Authorization": "YOUR_API_KEY"
```

```json5
{
 "eventType": "channel_update",
 "userId": string, // identifies the Customer
 "botId": string, // identifies the bot invoking livechat
 "paused": false,
 "unpauseChatEvent": "default" | "none" | "eventName", // a Dialogflow event to fire when ending the Live Chat conversation
}
```

`unpauseChatEvent` is optional, when not provided or set to `default`, the message `NAME has left the conversation. You are now chatting with the bot.` is pushed to the conversation when bot requests are resumed.  
If you prefer to not have this message displayed in the conversation when resuming requests, set `unpauseChatEvent` to `none`.  
If you want to dispatch a Dialogflow Event immediately after bot requests are resumed, set `unpauseChatEvent` to the name of your dialogflow event.




### Show typing indicator

A typing indicator is used to let the user know your live agents are typing. The typing indicator disappears after 10 seconds automatically, so you'll need to trigger the endpoint again if your agent is still typing after 9 seconds.

```json5
"Content-Type": "application/json",
"Authorization": "YOUR_API_KEY"
```

```json5
{
 "eventType": "user_typing",
 "userId": string, // identifies the Customer
 "botId": string, // identifies the bot invoking livechat
 "paused": true,
 "minutesPaused": number, // maximum idle time for the Live Chat conversation
}
```

## Message History

When a Live Chat conversation starts in [step one](livechat/handover?id=step-one), Botcopy sends the history of the conversation for that user in order to provide context to the Live Agent. This data is sent to your custom endpoint as an array of objects at `messageHistory` property of the payload.

Below is how we represent these different types of messages.

### Dialogflow

#### Message sent to Dialogflow

```json5
{
    "_id": string, // id of the messageHistory item
    "botPlatform": "dialogflowES" | "dialogFlowCX", // the platform used by the bot
    "direction": "in", // in means a message sent by the customer to the bot, out means a message returned by the bot to the customer
    "payload": {
      "queryParameters": {
        "queryParameters": object, // query parameters sent to dialogflow detectIntent
        "query": string, // the customer message to the bot
        "lang": string, // the code for the customer language (e.g., 'en' for English)
        "sessionId": string, // dialogflow session id
    },
    "createdAt": string, // The creation date and time of the object in ISO 8601 format.
}
```

#### Event sent to Dialogflow

```json5
{
      "_id": string, // id of the messageHistory item
      "botPlatform": "dialogflowES" | "dialogFlowCX", // the platform used by the bot
      "direction": "in", // in means a message sent by the customer to the bot, out means a message returned by the bot to the customer
      "payload": {
        "queryParameters": object, // query parameters sent to dialogflow detectIntent
        "event": {
          "name": string, // event name
          "data": object
        },
        "lang": string, // the code for the customer language (e.g., 'en' for English)
        "sessionId": string, // dialogflow session id
      },
      "createdAt": string, // The creation date and time of the object in ISO 8601 format.
    },
```

#### Response from Dialogflow

```json5
{
  "_id": string, // id of the messageHistory item
  "botPlatform": "dialogflowES" | "dialogFlowCX", // the platform used by the bot
  "direction": "out", // in means a message sent by the customer to the bot, out means a message returned by the bot to the customer
  "payload": // unchanged response from Dialogflow detectIntent as described by Google Documentation
  // https://cloud.google.com/dialogflow/es/docs/reference/rest/v2/projects.agent.environments.users.sessions/detectIntent
  // https://cloud.google.com/dialogflow/cx/docs/reference/rest/v3/DetectIntentResponse
  "createdAt": string, // The creation date and time of the object in ISO 8601 format.
},
```

### Live Chat

#### Message from a Live Agent to a Customer

```json5
{
  "_id": string, // id of the messageHistory item
  "botPlatform": "dialogflowES" | "dialogFlowCX", // the platform used by the bot
  "direction": "out", // in means a message sent by the customer to the bot, out means a message returned by the bot to the customer
  "payload": {
    "text": string, // message sent by the live agent to the customer
  },
  "createdAt": string, // The creation date and time of the object in ISO 8601 format.
},
```

#### Message from a Customer to a Live Agent

```json5
{
  "_id": string, // id of the messageHistory item
  "botPlatform": "dialogflowES" | "dialogFlowCX", // the platform used by the bot
  "direction": "out", // in means a message sent by the customer to the bot, out means a message returned by the bot to the customer
  "payload": {
    "queryParameters": {
      "contexts": [],
      "payload": {},
      "webhookHeaders": {}
    },
    "query": string, // message sent by the customer to the live agent
    "lang": string, // the code for the customer language (e.g., 'en' for English)
    "sessionId": string, // dialogflow session id
  },
  "createdAt": string, // The creation date and time of the object in ISO 8601 format.
},
```

## Remarks

When communicating from Client -> Botcopy, the `API Key` is used.

When communicating from Botcopy -> Client, the `access token` and `custom header/value` is used.

`"Content-Type": "application/json"` is required in the header of the POST request to `https://api.botcopy.com/webhooks/handover/push`
