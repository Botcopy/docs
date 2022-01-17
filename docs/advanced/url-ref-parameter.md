# URL Ref Parameters
When a bot is loaded on a page, it will look for a user-specified **URL parameter**. If it exists, it will take the value of the parameter and send it to Dialogflow for future use.

By default, the window will look for a url parameter named `ref`. The parameter name can be changed on your Bot Prompts page.

## Use Case
Imagine you have a marketing campaign which will send emails to a list of customers. Each email corresponds to a customer in your database. In the email to a user, there is a button with a hyperlink to a marketing page on your website where a bot lives. The hyperlink may contain a **URL parameter** equal to that specific customer's ID in your database.

When the user clicks on the button in the email and is redirected to your landing page, that ID will be sent to Dialogflow. You could use that ID to pull customer information to your welcome intent and provide a personalized welcome message with the user's first name.

## URL Parameter in Practice
### Dialogflow ES

The value of the **URL parameter** is passed to a context named `botcopy-ref-context` as a parameter named `botcopyRefValue`. The context has a lifespan of two intents, so it is best to pull the custom value at the beginning of a conversation.

Here's an example function that would be used in the fulfillment inline editor to pull the custom value.

```js
function urlTest(agent) {
  // 'botcopy-ref-context' has the passed values
  // it has a lifespan of 2 intents
  const context = agent.context.get('botcopy-ref-context')

  // We're destructuring the value of botcopySnippetRefValue off the context's parameters
  const { botcopyRefValue } = context.parameters

  // at this point, you may send the value to an external database with an HTTP call
  // you may also save it to another context for later use
  // in this example, we'll just return the passed value as a simple response in the chat
  agent.add(`URL value = ${botcopyRefValue}`)
}
```

### Dialogflow CX
To reference the custom value in Dialogflow CX, use `$session.params.botcopyRefValue`.