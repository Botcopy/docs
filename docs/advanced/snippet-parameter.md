# Snippet Parameter
Your bot's embeddable snippet can take in a custom **snippet parameter** that will be sent to Dialogflow. Add the attribute **data-snippetRef** equal to the custom value in your snippet, like this:

```js
<script
  type="text/javascript"
  id="botcopy-embedder-d7lcfheammjct"
  class="botcopy-embedder-d7lcfheammjct"
  data-botId="yourBotID"
  data-snippetRef="yourCustomValue" // enter the custom value here
>
    var s = document.createElement('script');
    s.type = 'text/javascript'; s.async = true;
    s.src = 'https://widget.botcopy.com/js/injection.js';
    document.getElementById('botcopy-embedder-d7lcfheammjct').appendChild(s);
</script>
```

## Use Case
Imagine you own a real estate company with five different vacation rentals. Each vacation rental has a different website, address, contact information, number of units, etc. However, you want to use a single Dialogflow agent to serve all five rentals.

You could use the custom snippet parameter to provide a property ID on each website the bot is posted on. At the beginning of your conversation flow, you could use the ID to pull property-specific information at the beginning of your conversation. This way, you aren't required to create a new Dialogflow agent for each location.


## Snippet Parameter in Practice
### Dialogflow ES
The value of your **snippet parameter** is passed to a context named `botcopy-ref-context` as a parameter named `botcopySnippetRefValue`. The context has a lifespan of two intents, so it is best to pull the custom value at the beginning of a conversation.

Here's an example function that would be used in the fulfillment inline editor to pull the custom value.

```js
function snippetTest(agent) {
  // 'botcopy-ref-context' has the passed values
  // it has a lifespan of 2 intents
  const context = agent.context.get('botcopy-ref-context')

  // We're destructuring the value of botcopySnippetRefValue off the context's parameters
  const { botcopySnippetRefValue } = context.parameters

  // at this point, you may send the value to an external database with an HTTP call
  // you may also save it to another context for later use
  // in this example, we'll just return the passed value as a simple response in the chat
  agent.add(`Snippet value = ${botcopySnippetRefValue}`)
}
```

### Dialogflow CX
To reference the custom value in Dialogflow CX, use `$session.params.botcopySnippetValue`.
