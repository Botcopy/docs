# Authorizing an End-User
You may have a use case where you want to authorize an end-user to take actions within a bot. This guide will outline how you can leverage Botcopy's window events and methods to set up an authorization flow.

Organizations in all categories and sectors benefit from this feature. It allows you to:
- Pass data from your front-end website to a Dialogflow agent with custom parameters
- Pass headers and payloads to a webhook to authenticate users and act on their behalf within your backend (i.e., handle refunds, change user information, file claims)
- Collect information like user timezone and geolocation

## Instructions
The data exchange is similar between platforms. However, the specific methods used differ between Dialogflow ES and Dialogflow CX.

### Dialogflow ES
The chat window will look for an output context named `bc-auth-required` on an incoming intent response. If it exists, the chat window will fire a `bc-auth-required` window event. You should define this context before an intent where authorization would be required (fetching user data, changing user data, etc.), and typically you would want the `lifespan` of that context to equal `1`.

Once this event has been recognized with your implementation, you can use the global Botcopy object method `Botcopy.setESParameters` to add data to the next request sent to the Dialogflow API. As an example this request can be [Botcopy.sendEventSilent()](window/methods?id=send-silent-event) or any next input from the user. In terms of authorizing the user with an application, you may want to set a property on `webhookHeaders` with an authorization code like a JWT.

The global object method takes an object which follows the QueryParameters structure [documented here](https://cloud.google.com/dialogflow/es/docs/reference/rest/v2/QueryParameters).

```js
Botcopy.setESParameters({
  webhookHeaders: {jwt: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."}
})
```

In fulfillment or your webhook, you can then pull the incoming data and authorize your user.

### Dialogflow CX
The chat window looks for a parameter named `bcAuthRequired` set equal to `true` on a page or intent in the Dialogflow CX console. If it exists, the chat window will fire a `bc-auth-required` window event. If the parameter exists, Botcopy will set the parameter equal to `null` in the response so the event no longer fires.

Once the event has been recognized with your implementation, you can use the global Botcopy object method `Botcopy.setCXParameters` to add data to the next request sent to the Dialogflow API. As an example this request can be [Botcopy.sendEventSilent()](window/methods?id=send-silent-event) or any next input from the user. In this example, we will also use `webhookHeaders`.

The global object method takes an object which follows the QueryParameters structure [documented here](https://googleapis.dev/nodejs/dialogflow-cx/latest/google.cloud.dialogflow.cx.v3beta1.IQueryParameters.html).

Parameters sent with the method are set as CX session parameters, which may be referenced like this: `$session.params.**parameter-name**`. 

```js
Botcopy.setCXParameters({
  webhookHeaders: { jwt: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." },
  parameters: {
    profile: { name: "Lisa", age: 32 } 
    }
});
```

In your webhook, you can pull the incoming `webhookHeaders` from the request and authorize your user.
