# Window Methods

The global Botcopy object has a number of **methods** which can be used to control the bot's behavior through an app or website. The Botcopy object is available once the chat window loads.

## Send Event

Sends an event name to trigger a specific intent.

### Dialogflow ES

The event name is required. However you may also provide a context as well. The event name must correspond with an event assigned to an intent in Dialogflow ES.

```js
/**
 * @param name - required string. The event name to send
 * @param context - optional object. A context to send
 */
Botcopy.sendEvent("event-name", {
  name: "context-name",
  lifespanCount: 2,
  parameters: { email: "hello@botcopy.com" },
});
```

Button example:

`<button onclick="Botcopy.sendEvent("event-name")">Send event</button>`

### Dialogflow CX

The event name is required, and you may provide an object matching IQueryParameters as outlined in the [Dialogflow CX SDK Client Reference here](https://googleapis.dev/nodejs/dialogflow-cx/latest/google.cloud.dialogflow.cx.v3beta1.IQueryParameters.html).

```js
/**
 * @param name - required string. The event name to send
 * @param queryParameters - optional object. Query parameters to send
 */
Botcopy.sendCXEvent("event-name", {
  parameters: { email: "hello@botcopy.com" },
  webhookHeaders: { jwt: "eyJhbGci09i..." },
});
```

Button example:

`<button onclick="Botcopy.sendCXEvent("event-name")">Send event</button>`

## Send Silent Event

A silent event does not remove the previous message's suggestion chips. The optional context or parameters have the same syntax as [Send Event](window/methods?id=send-event).

### Dialogflow ES

```js
/**
 * @param name - required string. The event name to send
 * @param context - optional object. A context to send
 */
Botcopy.sendEventSilent("event-name", {
  name: "context-name",
  lifespanCount: 2,
  parameters: { email: "hello@botcopy.com" },
});
```

Button example:
`<button onclick="Botcopy.sendEventSilent("event-name")">Silently send event and open chat window</button>`

### Dialogflow CX

```js
/**
 * @param name - required string. The event name to send
 * @param queryParameters - optional object. Query parameters to send
 */
Botcopy.sendCXEventSilent("event-name", {
  parameters: { email: "hello@botcopy.com" },
  webhookHeaders: { jwt: "eyJhbGci09i..." },
});
```

## Send Text

Sends a training phrase to trigger a specific intent.

```js
/**
 * @param phrase - required string. The training phrase to send
 * @param context - optional object. A context to send
 */
Botcopy.sendText("training-phrase", {
  name: "context-name",
  lifespanCount: 2,
  parameters: { email: "hello@botcopy.com" },
});
```

## Show Feedback

Shows the feedback box.

```js
Botcopy.showFeedback();
```

Button example:

`<button onclick="Botcopy.showFeedback()">Show Feedback</button>`

## Hide Feedback

Hides the feedback box.

```js
Botcopy.hideFeedback();
```

Button example:

`<button onclick="hideFeedback()">Hide Feedback</button>`

## Enable Focus Trap

Enables Focus Traps

```js
Botcopy.enableFocusTrap();
```

Button example:

`<button onclick="Botcopy.enableFocusTrap()">Enable Focus Traps</button>`

## Disable Focus Trap

Disables Focus Traps

```js
Botcopy.disableFocusTrap();
```

Button example:

`<button onclick="Botcopy.disableFocusTrap()">Disable Focus Traps</button>`

## Clear History

Clears the chat message history.

```js
Botcopy.clearHistory();
```

Button example:

`<button onclick="Botcopy.clearHistory()">Clear History</button>`

## Select Language

Changes the bot's language. Provided language must be supported by your agent.

```js
Botcopy.selectLanguage("code");
```

Button example:

`<button onclick="Botcopy.selectLanguage('es')">Select Español</button>`

## Show Languages

Show supported languages.

```js
Botcopy.showLanguages(boolean);
```

Button example:

`<button onclick="Botcopy.showLanguages(true)">Show Languages</button>`

## Open Prompt

Shows the prompt bubble when minimized.

```js
Botcopy.openPrompt();
```

Button example:

`<button onclick="Botcopy.openPrompt()">Opens Prompt</button>`

## Close Prompt

Hides the prompt bubble when minimized.

```js
Botcopy.closePrompt();
```

Button example:

`<button onclick="Botcopy.closePrompt()">Closes Prompt</button>`

## Close Webview

Closes the webview iFrame.

```js
Botcopy.closeWebview();
```

Button example:

`<button onclick="Botcopy.closeWebview()">Close Webview</button>`

## Open Window

Opens the chat window.

```js
Botcopy.openWindow();
```

Button example:

`<button onClick="Botcopy.openWindow()">Opens Chat Window</button>`

## Close Window

Closes the chat window.

```js
Botcopy.closeWindow();
```

Button example:

`<button onClick="Botcopy.closeWindow()">Closes Chat Window</button>`
