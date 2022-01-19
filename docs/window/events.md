# Window Events
The chat window detects and broadcasts user activity with **window events**. You may create an [event listener](https://developer.mozilla.org/en-US/docs/Web/API/EventListener) to utilize these broadcasts.

```js
window.addEventListener('botcopy-events', function (e) {
    switch(e.detail.type) {
      case 'bc-initialized':
        // do something
        break

      case 'bc-chip-clicked':
        // do something
        break
    }
});
```

## bc-initialized
Fires when the chat window has initialized.

```js
{
  type: 'bc-initialized',
  payload: {}
}
```

## bc-agent-entered-chat
Fires when a live agent enters a chat.

```js
{
  type: 'bc-agent-entered-chat',
  payload: {
    avatarURL: "string", // the image url of the agent
    humanName: "string" // the name of the agent
  }
}
```

## bc-agent-left-chat
Fires when a live agent leaves the chat.

```js
{
  type: 'bc-agent-left-chat',
  payload: {
    avatarURL: "string", // the image url of the agent
    humanName: "string" // the name of the agent
  }
}
```

## bc-agent-message-sent
Fires when a live agent sends a message to a user.

```js
{
  type: "bc-agent-message-sent",
  payload: {
    message: "string" // the content of the message
  }
}
```

## bc-auth-required
Fires to notify you when your next intent requires authorizing your user. See [Authorizing Users](advanced/bc-auth.md?id=authorizing-a-user) for more details.

```js
{
  type: "bc-auth-required",
  payload: {}
}
```

## bc-bot-response
Fires when a response is received from a bot.

**Dialogflow ES**
```js
{
  type: "bc-bot-response",
  payload: {
    intentName: "string" // name of the intent
  }
}
```

**Dialogflow CX**
```js
{
  type: "bc-bot-response",
  payload: {
    currentFlow: "string", // id of the current flow
    currentPage: "string", // id of the current page
    intentName: "string", // name of the intent
    previousFlow: "string", // id of the previous flow
    previousPage: "string" // id of the previous page
  }
}
```

## bc-button-clicked
Fires when a button on a card is clicked.

```js
{
  type: "bc-button-clicked",
  payload: {
    input: "string", // selection key
    url: "string" // link url for the card
  }
}
```

## bc-card-clicked
Fires when a carousel item is clicked.

```js
{
  type: "bc-card-clicked",
  payload: {
    input: "string" // selection key
  }
}
```

## bc-chip-clicked
Fires when a suggestion chip is clicked.

```js
{
  type: "bc-chip-clicked",
  payload: {
    input: "string" // the title of the suggestion chip
  }
}
```

## bc-form-submitted
Fires when a custom Botcopy form is submitted. Optionally, some form fields may be exposed in the payload.

```js
{
  type: "bc-form-submitted",
  payload: {} // set the "expose" attribute on a form field to have it appear here
}
```

## bc-link-out-clicked
Fires when a linkout suggestion is clicked.

```js
{
  type: "bc-link-out-clicked",
  payload: {
    url: "string" // the url of the linkout suggestion
  }
}
```

## bc-list-element-clicked
Fires when a list item is clicked.

```js
{
  type: "bc-list-element-clicked",
  payload: {
    input: "string" // selection key of the item
  }
}
```

## bc-history-cleared
Fires when chat history is cleared.

```js
{
  type: "bc-history-cleared",
  payload: {}
}
```

## bc-tts-on
Fires when text-to-speech is enabled.

```js
{
  type: "bc-tts-on",
  payload: {}
}
```

## bc-tts-off
Fires when text-to-speech is disabled.

```js
{
  type: "bc-tts-off",
  payload: {}
}
```

## bc-feedback-open
Fires when the feedback box opens.

```js
{
  type: "bc-feedback-open",
  payload: {}
}
```

## bc-feedback-close
Fires when the feedback box closes.

```js
{
  type: "bc-feedback-close",
  payload: {}
}
```

## bc-webview-open
Fires when a webview iFrame opens in the chat.

```js
{
  type: "bc-webview-open",
  payload: {}
}
```

## bc-webview-close
Fires when a webview iFrame closes within the chat.

```js
{
  type: "bc-webview-close",
  payload: {}
}
```

## bc-window-open
Fires when the chat window opens.

```js
{
  type: "bc-window-open",
  payload: {}
}
```

## bc-window-close
Fires when the chat window closes.

```js
{
  type: "bc-window-close",
  payload: {}
}
```