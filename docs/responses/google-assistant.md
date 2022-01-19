# Google Assistant
Dialogflow ES comes with rich response types out-of-the-box with **Google Assistant**. This section provides a quick overview of each response type with a visual aid.

We've also included an example of using each response type in the **Fulfillment** inline editor or a webhook. Each example will be using the `actions-on-google` package by declaring `agent.conv()` before the response. Each response type must be imported.

Google Assistant has certain design constraints when using their response types in Dialogflow ES. If you would like to design a conversation outside of these constraints, please view our **Custom Payloads** section *todo: link to custom payloads here*.

Please note that Google Assistant response types are *only* supported for Dialogflow ES. To create rich responses for Dialogflow CX agents, please view our **Custom Payloads** section *todo: link to custom payloads here*.

*todo: add visual aid for each response type*

## Simple Responses
**Simple Responses** are the default text message response. They must be included in a message group with cards, carousels, lists, and suggestions.

Note: If your agent is set up with default responses, you can toggle on "Use Response from the DEFAULT tab as the first response.

**Simple Response Fulfillment**
```js
function simpleResponse(agent) {
    agent.requestSource = "ACTIONS_ON_GOOGLE";
    let conv = agent.conv();
    conv.ask('Hello World!');
    agent.add(conv);
  }
```

## Suggestion Chips
**Suggestion Chips** are suggested options for your users to choose from. Add them strategically to help your customers navigate your chat more easily.

**Suggestion Chip Fulfillment**
```js
const { Suggestions } = require('actions-on-google')

function suggestions(agent) {
    agent.requestSource = "ACTIONS_ON_GOOGLE";
   	let conv = agent.conv();
   	conv.ask('This is a simple response from fulfillment.');

    // adds a single suggestion chip
  	conv.ask(new Suggestions('Menu'));

    // adds multiple suggestion chips
   	conv.ask(new Suggestions(['Support', 'Pricing']));
   	agent.add(conv);
    }
```

## Linkout Suggestions
**Link-out Suggestions** are visually similar to suggestion chips but provide a hyperlink to an external website instead of driving the chat conversation.

**Linkout Suggestion Fulfillment**
```js
const { LinkOutSuggestion } = require('actions-on-google')

function linkOut(agent) {
  	agent.requestSource = agent.ACTIONS_ON_GOOGLE;
  	const conv = agent.conv();
  	conv.ask('Simple responses are required before a rich response');
  	conv.ask(
  		new LinkOutSuggestion({
			name: 'Title of Link Out',
			url: 'https://botcopy.com'
		})
	);
	agent.add(conv);
}
```

## Basic Cards
**Basic Cards** can be used for images, gifs, videos and files by placing the URL in the image URL field. Cards typically contain a title, text, and a button.

Basic cards may contain a URL to an external website. The default behavior for these URLs is to open the link in an iFrame within the chat window. This behavior can be changed by manipulating the URL when creating the basic card:
- To open the link in a new tab, add ?nt=t to the end of your url: https://botcopy.com?nt=t
- To open the link in the same tab, add ?nt=f to the end of your url: https://botcopy.com?nt=f

Images are rendered in a `16:9` ratio.

Videos are supported by adding a video URL as an image.

Botcopy supports Dropbox URLs to media as well. **IMPORTANT:** Please adjust the Dropbox image URL to the following for it to work:

```
Original URL
https://**www**.dropbox.com/s/49cj0a442kxig1d/FRont%20page%20ad%20%2887060D21-08F8-4195-9F09-340CA2A64C3F%29.mp4?dl=0

New URL
https://**dl**.dropboxusercontent.com/s/49cj0a442kxig1d/FRont%20page%20ad%20%2887060D21-08F8-4195-9F09-340CA2A64C3F%29.mp4?dl=0
```

YouTube videos may also be embedded within the card. **IMPORTANT:** If you wish to embed a YouTube video, please adjust the URL to the following for it to work:

```
Original URL
https://www.youtube.com/**watch?v=**i5kZ8JuC10g

New URL
https://www.youtube.com/**embed/**i5kZ8JuC10g
```

**Basic Card Fulfillment**
```js
const { BasicCard, Button, Image } = require('actions-on-google')

function basicCard(agent) {
  agent.requestSource = agent.ACTIONS_ON_GOOGLE;
  const conv = agent.conv();
  conv.ask('Here is a basic card through fulfillment:');
  conv.ask(
    new BasicCard({
      buttons: [
        new Button({ title: 'Botcopy Home', url: 'https://botcopy.com' }),
        new Button({ title: 'Botcopy Pricing', url: 'https://botcopy.com/pricing' })
      ],
      text: 'Welcome to Botcopy! Please select an option below or ask a question in the chat!',
      image: new Image({
        url:
          'https://cdn-images-1.medium.com/max/1200/1*s-JvYKXGEnYPdyIgT0w7gw.png',
        alt: 'Bot banner'
      }),
      subtitle: '{--}',
      title: 'Keystroke Support'
    })
  );
  agent.add(conv);
}
```

## Carousel Cards
**Carousel Cards** display a series of cards with images. Selecting one will drive the conversation forward with a training phrase defined as the *option key* when setting up the response.

Carousels do not support video files.

## Browse Carousel Cards
**Browse Carousel Cards** link to an external web URL when selected.

## Lists
**Lists** display a header and a number of list items to display to the user. Each list item may have an image and drives the conversation forward when selected. Botcopy's **Custom Payloads** allow for linkout functionality with lists *todo: add link to custom payloads here*.
