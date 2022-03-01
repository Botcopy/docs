# Chat Components
Botcopy offers some added utility with **Chat Components**. Modular control for custom chat window experiences.

## Disable Input Bar

**Dialogflow ES**

To disable the input bar and force users to make a selection (suggestion chips, card buttons etc), add **botcopy-disable-inputbar** (lifespan: 1) as an **output context** to an intent.

**Dialogflow CX**

To disable the input bar and force users to make a selection (suggestion chips, card buttons etc), add a **session parameter** of **bcDisableInputBar** with a value of **true**.

## Feedback
When enabled, the **Feedback** component will appear after responses marked as **End of Conversation** in Dialogflow. You also have a few choices for triggering the box manually:

- Set a context named `botcopy-feedback` on an intent in Dialogflow ES
- Set a parameter named `botcopyFeedback` equal to `true` on an intent in Dialogflow CX
- Use the Feedback method on the global Botcopy window object - `Botcopy.showFeedback()` <div onclick="Botcopy.openWindow(); Botcopy.showFeedback()" style="cursor: pointer">**Show Example**<div>


By default, the content of the Feedback box will be rendered in your user's preferred browser language. You can change the content of the feedback box, but any changes will be static in the given language.

Additionally, you may choose if a user can leave comments or is restricted to a thumbs up / thumbs down selection.

## Privacy Policy
Enabling the **Privacy Policy** component will display a consent screen prior to users engaging with the bot. The Privacy Policy will link to a URL of your choice. As with the Feedback component, you may customize the content of this box, but any custom copy will no longer be automatically translated for your users.

## Profile
Your bot's **Profile** adds supplemental information for users. This information can be viewed by clicking the header or selecting the **Profile** from the dropdown in the header.

**Phone** and **Email** are provided fields with custom logos. You may also provide custom fields for any other information you'd like to display.

## Webview

Can be triggered through your bot or website. 

Use a session parameter (**bcWebviewUrl**) for Dialogflow CX or a context (**openWebview**) for Dialogflow ES to display a webview. 

**DialogflowCX**

Set as a parameter preset in the CX GUI or in [fulfillment](https://cloud.google.com/dialogflow/cx/docs/concept/parameter). 

```
Parameter: bvWebviewUrl
Value: https://botcopy.com
```

**DialogflowES**

```
const webviewContext =  {
name: 'openWebview',
lifespan: 1,
parameters: {
    hideBrowserTab: false, // defaults to false
    webViewTarget: 'self', // defaults to self
    webViewUrl: 'https://website.com/page' // webview url
 }
};
```

Below is an example of how to trigger a webview from an intent on Dialogflow ES through fulfillment and providing a response in case the user closes the webview. Uses the actions-on-google package.

```
// this example uses a Basic Card from 'actions-on-google'
// you'll need to deconstruct the BasicCard and Button classes from the actions-on-google package

const { BasicCard, Button } = require('actions-on-google')

 function showWebview(agent) {
 	// set your agent's requestSource to 'ACTIONS_ON_GOOGLE'
 	agent.requestSource = 'ACTIONS_ON_GOOGLE';
 	// set a context that is passed to Botcopy to trigger your webview
 	const urlContext1 = {
 		name: 'openWebview',
 		lifespan: 1,
 		parameters: {
 			hideBrowserTab: false,
 			webViewTarget: 'self',
 			webViewUrl: 'https://yourwebsite.com/page'
 		}
 	};
 	//If using multiple webviews, set a unique context per intent
 	agent.context.set(urlContext1);
 	// declare a variable conv (this can be a global variable or within an intent response function)
 	const conv = agent.conv();
 	conv.ask("Opening a webview!");
 	conv.ask(new BasicCard({
 		buttons: [new Button({
 			title: 'Button Title',
 			url: 'https://yourwebsite.com/page'
 		})],
 		title: 'title text'
 	}));
 	// add your conv variable to your agent to send your responses to your botcopy widget
 	agent.add(conv);
 }
 ```