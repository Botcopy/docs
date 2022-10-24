# Google Assistant

**Warning: Google Assistant Conversational Actions will be sunsetted from Dialogflow ES on June 13, 2023. At that time, this Dialogflow Google Assistant integration will be removed. For details, see [Conversational Actions sunset](https://developers.google.com/assistant/ca-sunset).**

Dialogflow ES comes with rich response types out-of-the-box with **Google Assistant**. This section provides a quick overview of each response type with a visual aid.

We've also included an example of using each response type in the **Fulfillment** inline editor or a webhook. Each example will be using the `actions-on-google` package by declaring `agent.conv()` before the response. Each response type must be imported.

Google Assistant has certain design constraints when using their response types in Dialogflow ES. If you would like to design a conversation outside of these constraints, please view our [Custom Payloads](https://botcopy.github.io/docs/#/responses/botcopy-custom-payloads).

Please note that Google Assistant response types are **only** supported for Dialogflow ES.

## Simple Responses

**Simple Responses** are the default text message response. They must be included in a message group with cards, carousels, lists, and suggestions.

Note: If your agent is set up with default responses, you can toggle on "Use Response from the DEFAULT tab as the first response.

**Simple Response Fulfillment**

```js
function simpleResponse(agent) {
  agent.requestSource = "ACTIONS_ON_GOOGLE";
  let conv = agent.conv();
  conv.ask("Hello World!");
  agent.add(conv);
}
```

## Suggestion Chips

**Suggestion Chips** are suggested options for your users to choose from. Add them strategically to help your customers navigate your chat more easily.

**Suggestion Chip Fulfillment**

```js
const { Suggestions } = require("actions-on-google");

function suggestions(agent) {
  agent.requestSource = "ACTIONS_ON_GOOGLE";
  let conv = agent.conv();
  conv.ask("This is a simple response from fulfillment.");

  // adds a single suggestion chip
  conv.ask(new Suggestions("Menu"));

  // adds multiple suggestion chips
  conv.ask(new Suggestions(["Support", "Pricing"]));
  agent.add(conv);
}
```

## Linkout Suggestions

**Link-out Suggestions** are visually similar to suggestion chips but provide a hyperlink to an external website instead of driving the chat conversation.

**Linkout Suggestion Fulfillment**

```js
const { LinkOutSuggestion } = require("actions-on-google");

function linkOut(agent) {
  agent.requestSource = agent.ACTIONS_ON_GOOGLE;
  const conv = agent.conv();
  conv.ask("Simple responses are required before a rich response");
  conv.ask(
    new LinkOutSuggestion({
      name: "Title of Link Out",
      url: "https://botcopy.com",
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

[Videos](https://botcopy.github.io/docs/#/responses/videos) are supported by adding a video URL as an image.

**Fulfillment example**

```js
const { BasicCard, Button, Image } = require("actions-on-google");

function basicCard(agent) {
  agent.requestSource = agent.ACTIONS_ON_GOOGLE;
  const conv = agent.conv();
  conv.ask("Here is a basic card through fulfillment:");
  conv.ask(
    new BasicCard({
      buttons: [
        new Button({ title: "Botcopy Home", url: "https://botcopy.com" }),
        new Button({
          title: "Botcopy Pricing",
          url: "https://botcopy.com/pricing",
        }),
      ],
      text: "Welcome to Botcopy! Please select an option below or ask a question in the chat!",
      image: new Image({
        url: "https://cdn-images-1.medium.com/max/1200/1*s-JvYKXGEnYPdyIgT0w7gw.png",
        alt: "Bot banner",
      }),
      subtitle: "{--}",
      title: "Keystroke Support",
    })
  );
  agent.add(conv);
}
```

## Carousel Cards

**Carousel Cards** display a series of cards with images. Selecting one will drive the conversation forward with a training phrase defined as the _option key_ when setting up the response.

Carousels do not support video files.

Below is an example of how to add a carousel as a response in fulfillment using the actions-on-google package. We recommend using this package over the dialogflow-fulfillment package.

```js
// for this example, you must require the 'actions-on-google' library
// deconstruct the Carousel and Image classes from the library at the top of your document
// example: const { Carousel, Image } = require('actions-on-google')

function fulfillmentCarousel(agent) {
  // set your agent's requestSource to 'ACTIONS_ON_GOOGLE'
  agent.requestSource = agent.ACTIONS_ON_GOOGLE;

  // declare a variable conv (this can be a global variable or within an intent response function)
  let conv = agent.conv();

  // conv.ask your responses

  // this is a simple text response
  conv.ask("This is a carousel example.");

  // this is a carousel response
  conv.ask(
    new Carousel({
      // items in your carousel
      items: {
        // option key name - sent as a response when a user clicks on this card
        SELECTION_KEY_ONE: {
          synonyms: ["synonym 1", "synonym 2", "synonym 3"],
          // title of your carousel item
          title: "Title of First Carousel Item",
          // description of your item
          description: "This is a description of a carousel item.",
          // an image associated with a card. note the new 'Image' class we imported
          image: new Image({
            url: "IMG_URL_AOG.com",
            alt: "Image alternate text",
          }),
        },
        // second carousel item
        SELECTION_KEY_GOOGLE_HOME: {
          synonyms: ["Google Home Assistant", "Assistant on the Google Home"],
          title: "Google Home",
          description:
            "Google Home is a voice-activated speaker powered by " +
            "the Google Assistant.",
          image: new Image({
            url: "IMG_URL_GOOGLE_HOME.com",
            alt: "Google Home",
          }),
        },
        // third carousel item
        SELECTION_KEY_GOOGLE_PIXEL: {
          synonyms: ["Google Pixel XL", "Pixel", "Pixel XL"],
          title: "Google Pixel",
          description: "Pixel. Phone by Google.",
          image: new Image({
            url: "IMG_URL_GOOGLE_PIXEL.com",
            alt: "Google Pixel",
          }),
        },
      },
    })
  );

  // add your conv variable to your agent
  // this sends all your responses to your botcopy widget
  agent.add(conv);
}
```

Below is an example of how to add a carousel as a response in fulfillment using the dialogflow-fulfillment package.

```
// for this example, you must require the 'dialogflow-fulfillment' library
// deconstruct the Payload class from the library at the top of your document
// example: const { Payload } = require('dialogflow-fulfillment')

function fulfillmentPayload(agent) {
  // set your agent's requestSource to 'ACTIONS_ON_GOOGLE'
  agent.requestSource = 'ACTIONS_ON_GOOGLE';

  // use agent.add and the new Payload class to send a raw JSON Carousel to botcopy
  agent.add(
    new Payload(agent.ACTIONS_ON_GOOGLE, {
      expectUserResponse: true,
      richResponse: {
        items: [
          {
            simpleResponse: {
              textToSpeech: 'Choose a item'
            }
          }
        ]
      },
      systemIntent: {
        intent: 'actions.intent.OPTION',
        data: {
          '@type': 'type.googleapis.com/google.actions.v2.OptionValueSpec',
          carouselSelect: {
            items: [
              {
                optionInfo: {
                  key: 'first title'
                },
                description: 'first description',
                image: {
                  url:
                    'https://developers.google.com/actions/images/badges/XPM_BADGING_GoogleAssistant_VER.png',
                  accessibilityText: 'first alt'
                },
                title: 'first title'
              },
              {
                optionInfo: {
                  key: 'second'
                },
                description: 'second description',
                image: {
                  url:
                    'https://lh3.googleusercontent.com/Nu3a6F80WfixUqf_ec_vgXy_c0-0r4VLJRXjVFF_X_CIilEu8B9fT35qyTEj_PEsKw',
                  accessibilityText: 'second alt'
                },
                title: 'second title'
              }
            ]
          }
        }
      }
    })
  );
}
```

## Browse Carousel Cards

**Browse Carousel Cards** link to an external web URL when selected.

Here is an example on adding a Browse Carousel Card in fulfillment with the actions-on-google package

Note: To open links in a new tab versus within the chat window, add ?nt=t at the end of your url: https://botcopy.com?nt=t

```
// deconstruct the BrowseCarousel, BrowseCarouselItem and Image classes from the library at the top of your document

const { BrowseCarousel, BrowseCarouselItem, Image } = require('actions-on-google')

function browseCarousel(agent) {
        agent.requestSource = "ACTIONS_ON_GOOGLE";
	let conv = agent.conv();
	conv.ask('This is a browse carousel example from fulfillment.');
	// Create a browse carousel
	conv.ask(new BrowseCarousel({
		items: [
			new BrowseCarouselItem({
				title: 'Title of item 1',
				url: 'https://botcopy.com',
				description: 'Description of item 1',
				image: new Image({
					url: 'https://www.botcopy.com/wp-content/uploads/2019/01/Screen-Shot-2019-01-27-at-3.37.28-PM.png',
					alt: 'Image alternate text',
				}),
                                footer: 'Item 1 footer', //Supported in next update
			}),
			new BrowseCarouselItem({
				title: 'Title of Item 2',
				url: 'https://botcopy.com',
				description: 'Description of Item 2',
				image: new Image({
					url: 'https://www.botcopy.com/wp-content/uploads/2019/01/Screen-Shot-2019-01-25-at-10.31.54-AM.png',
					alt: 'Image alternate text',
				}),
                                footer: 'Item 2 footer',
			}),
		],
	}));
	agent.add(conv);
}
```

## Lists

**Lists** display a header and a number of list items to display to the user. Each list item may have an image and drives the conversation forward when selected. Botcopy's [Custom Payloads](https://botcopy.github.io/docs/#/responses/botcopy-custom-payloads) allow for linkout functionality with lists.

Below is an example of how to add a List in fulfillment using the actions-on-google package.

```
// this example uses the List class from 'actions-on-google'
// the structure of a list is similar to a carousel
// you'll need to deconstruct the List and Image classes from the actions-on-google package

const { List, Image } = require('actions-on-google')

function list(agent) {
  // set your agent's requestSource to 'ACTIONS_ON_GOOGLE'
  agent.requestSource = agent.ACTIONS_ON_GOOGLE;

  // declare a variable conv (this can be a global variable or within an intent response function)
  let conv = agent.conv();

  // conv.ask your responses

  // this is a simple text response
  conv.ask('This is a list example.');

  // this is a list response
  conv.ask(
    new List({
      // one main difference between a list and a carousel is the title
      // this title is optional. if you choose to include one, it will be rendered as a header over the list
      title: 'List Title',
      items: {
        // the fields within a list mirror those in a carousel
        SELECTION_KEY_ONE: {
          synonyms: ['synonym 1', 'synonym 2', 'synonym 3'],
          title: 'Title of First List Item',
          description: 'This is a description of a list item.',
          // note the image field uses the Image class like carousels
          image: new Image({
            url: 'IMG_URL_AOG.com',
            alt: 'Image alternate text'
          })
        },
        // Add the second item to the list
        SELECTION_KEY_GOOGLE_HOME: {
          synonyms: ['Google Home Assistant', 'Assistant on the Google Home'],
          title: 'Google Home',
          description:
            'Google Home is a voice-activated speaker powered by ' +
            'the Google Assistant.',
          image: new Image({
            url: 'IMG_URL_GOOGLE_HOME.com',
            alt: 'Google Home'
          })
        },
        // Add the third item to the list
        SELECTION_KEY_GOOGLE_PIXEL: {
          synonyms: ['Google Pixel XL', 'Pixel', 'Pixel XL'],
          title: 'Google Pixel',
          description: 'Pixel. Phone by Google.',
          image: new Image({
            url: 'IMG_URL_GOOGLE_PIXEL.com',
            alt: 'Google Pixel'
          })
        }
      }
    })
  );

  // add your conv variable to your agent
  // this sends all your responses to your botcopy widget
  agent.add(conv);
}
```
