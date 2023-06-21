# Window Events

Botcopy Window Events enhance your website functionality by allowing your website to detect and respond to broadcasts from the Botcopy chat window. Our event types offer various triggers your website can detect and respond to, e.g., user actions such as opening the chat window, sending a message, selecting a language option, or authentication.

When you are ready to add window events to your website, you must add an [event listener](https://developer.mozilla.org/en-US/docs/Web/API/EventListener ":target=_blank") (example below) to your website code. This code will detect the desired window event type and trigger the desired response from your website. Depending on the type of event you are listening for, the website could provide relevant content, authenticated information, a personalized message, or other website behaviors to boost engagement significantly.

```js
window.addEventListener("botcopy-events", function (e) {
  switch (e.detail.type) {
    case "bc-initialized":
      // do something
      break;

    case "bc-chip-clicked":
      // do something
      break;
  }
});
```

## bc-initialized

Fires when the chat window has initialized. Use this event to identify when the chat window has fully loaded. All other events and [methods](window/methods?id=window-methods) occur/can occur after initialization. 

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

## bc-focus-trap-on

Fires when the focus trap is enabled.

```js
{
  type: "bc-focus-trap-on",
  payload: {}
}
```

## bc-focus-trap-off

Fires when the focus trap is disabled.

```js
{
  type: "bc-focus-trap-off",
  payload: {}
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

## bc-history-cleared

Fires when chat history is cleared.

```js
{
  type: "bc-history-cleared",
  payload: {}
}
```

## bc-language-selected

Fires when a language is selected.

```js
{
  type: "bc-language-selected",
  payload: {
    currentCode: "string", // selected language code
    currentName: "string", // selected language name
    previousCode: "string", // previous language code
    previousName: "string", // previous language name
  }
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


## bc-sound-played

Fires when a sound is played.

```js
{
  type: "bc-sound-played",
  payload: {
    sound: number
  }
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

## bc-window-style

Fires when the chat window style changes through the setWindowStyle method.

```js
{
  type: "bc-window-style",
   payload: {
    style: "string"
  }
}
```
