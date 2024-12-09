# Botcopy Response Payload Format for Dialogflow CX

## Overview

This document outlines the structure and best practices for Botcopy responses. It explains how to effectively use each response type and highlights the optimal formats for different bot interactions. Botcopy’s flexible payload structure allows combining various actions within a single response to enhance user experience.

Examples:

- Suggestions can be used to send messages or links
- Card Buttons can send messages or links
- Carousels can contain more than one type of action
  - Link cards act as browse carousel cards
  - Message cards act as regular selection key carousel cards

[Explore Actions in Detail](#actions)

**Response Types:**
- [Text Responses](#text-responses)
- [Suggestions](#suggestions)
- [Basic Cards](#basic-cards)
- [Carousels](#carousels)
- [Lists](#lists)
- [Forms](#forms)

## Botcopy Response Payload Format

### How to Use the Payload

Botcopy responses use an array-based format with an `action` field to determine the chat’s behavior when users interact with each component. This structure enables you to include multiple response types within a single payload, making interactions versatile and dynamic.

**Key Points:**
- Each custom payload should include a `botcopy` array with the desired rich responses.
- Default Dialogflow CX text responses will display alongside the custom payload if  provided.
- Rich responses are displayed in the order they appear in the array, ensuring a consistent user experience.

**Why This Matters:**  
This flexible approach allows you to design more precise and engaging interactions. For instance:
- Carousels can offer a mix of internal navigation (message actions) and external redirection (link actions).
- Cards and Suggestions can direct users to the next step efficiently with buttons or links.
By combining different response types, you can create a seamless and intuitive chatbot experience.

## Text Responses

An array of text responses used to convey static information.

The displayText, textToSpeech, and ssml fields handle different output formats for the message:

- displayText: The text displayed to the user.
- textToSpeech: The spoken version for voice interfaces.
- ssml: Rich text for speech synthesis using SSML (Speech Synthesis Markup Language).

**Usage:**
- Ideal for straightforward messages or confirmations.

**Best For:**
- Simple instructions, greetings, or responses that don’t require interactivity.

#### Example Payload
```
{
  "botcopy": [
    {
      "text": [
        {
          "displayText": "Hello.",
          "ssml": "<speak>Hi!</speak>",
          "textToSpeech": "Hi, how are you doing?"
        }
      ]
    }
  ]
}
```

#### Why Use This:
The simplest way to deliver static information without the need for buttons, links, or user input.

#### Properties

- displayText - the message displayed in the chat.
- ssml (optional) - SSML format text for the phrase spoken by a bot.
- textToSpeech (optional) - the phrase spoken by the bot. Takes priority over SSML. If both are provided in the payload, textToSpeech is used for the audio file.


## Suggestions

Suggestions provide users with clickable buttons to guide them through the conversation or direct them to external resources. Each suggestion performs a specific action, either continuing the dialogue with a [message](#message) action or opening an external link with a [link](#link) action.

**Usage:**
- Offer quick, clickable options after a response to streamline user interaction.

**Best For:**
- Guiding users toward common follow-ups or linking to external resources.

#### Example Payload
```
{
  "botcopy": [
    {
      "suggestions": [
        {
          "action": {
            "message": {
              "command": "Pricing",
              "type": "training"
            }
          },
          "title": "Message Suggestion"
        },
        {
          "action": {
            "link": {
              "target": "_blank",
              "url": "https://botcopy.com"
            }
          },
          "title": "Link Suggestion"
        }
      ]
    }
  ]
}
```
#### Why Use This:
- Simplifies Navigation: Reduces the need for typing by offering pre-set choices.
- Enhances Efficiency: Helps users quickly access relevant information or actions.
- Improves Satisfaction: Streamlined interactions lead to a smoother user experience.

#### Properties

- title: The text displayed on the suggestion button.
- action: Defines what happens when the suggestion is clicked. Options include:
  - [message](#message): Continues the conversation with a predefined command.
  - [link](#link): Opens an external URL in a new window or tab.


## Basic Cards

Basic cards present structured information using a title, subtitle, body, and optional media like images, GIFs, and [videos](/responses/videos). They can also include [button](#button) actions for user interaction.

**Usage:**
Display detailed information with a visual layout, and provide interactive buttons for next steps.

**Best For:**
Highlighting key information and offering follow-up actions with buttons.

#### Example Payload
```
{
  "botcopy": [
    {
      "card": {
        "title": "Card Title",
        "subtitle": "Card Subtitle",
        "body": "Card Body",
        "image": {
          "url": "https://example.com/image.png",
          "alt": "Image Description"
        },
        "action": {
          "buttons": [
            {
              "action": {
                "link": {
                  "url": "https://botcopy.com"
                }
              },
              "title": "Visit Website"
            }
          ]
        }
      }
    }
  ]
}

```

#### Why Use This:
- Visual Structure: Organizes content in a clear, visually appealing format.
- User Engagement: Buttons guide users to take specific actions.
- Versatility: Ideal for displaying information, media, and actionable next steps.

#### Properties

- title: Main heading for the card.
- subtitle (optional): Subheading to provide additional context.
- body (optional): Detailed text or description.
- image (optional): object containing media info.
  - url: link to media or media.
  - alt (optional): alternative text for accessibility.
- action
  - [message](#message)
  - [link](#link)
  - [button](#button)

## Carousels

Carousels present multiple cards in a scrollable format, each with a title, subtitle, body, and optional media. Each card can include an action such as [message](#message), [link](#link), or [button](#button).

**Usage:**
Display a series of cards that users can swipe or scroll through to explore multiple options.

**Best For:**
Showcasing products, services, or options with interactive actions on each card.

#### Example Payload
```
{
  "botcopy": [
    {
      "carousel": [
        {
          "title": "Item One",
          "subtitle": "Subtitle One",
          "body": "Description One",
          "image": {
            "alt": "Image of a cat",
            "url": "https://homepages.cae.wisc.edu/~ece533/images/cat.png"
          },
          "action": {
            "link": {
              "target": "_blank",
              "url": "https://botcopy.com"
            }
          }
        },
        {
          "title": "Item Two",
          "subtitle": "Subtitle Two",
          "body": "Description Two",
          "image": {
            "alt": "Image of a cat",
            "url": "https://homepages.cae.wisc.edu/~ece533/images/cat.png"
          },
          "action": {
            "message": {
              "command": "Pricing",
              "type": "training"
            }
          }
        }
      ]
    }
  ]
}

```

#### Why Use This:
- Interactive Display: Allows users to explore multiple options without overwhelming them.
- Flexibility: Each card can lead to different actions (e.g., links, messages, or buttons).
- Engagement: Visually appealing and easy to navigate, enhancing user experience.

#### Properties

- title: Main heading for each card.
- subtitle (optional): Subheading for additional context.
- body (optional): Description or detailed text for the card.
- image (optional):
  - url: link to image or media.
  - alt: alternative text for accessibility.
- action
  - [message](#message)
  - [link](#link)
  - [button](#button)

## Lists

Lists display multiple items in a structured format. Each list item can include a title, optional description, optional image, and an action such as a message or link. Unlike other components, lists do not support button actions.

**Usage:**
Present multiple options or categories in a clear, organized list.

**Best For:**
Offering concise lists of items or choices that users can interact with.

#### Example Payload
```
{
  "botcopy": [
    {
      "list": {
        "items": [
          {
            "action": {
              "link": {
                "target": "_blank",
                "url": "https://botcopy.com"
              }
            },
            "body": "Description One",
            "image": {
              "alt": "Image of a cat",
              "url": "https://homepages.cae.wisc.edu/~ece533/images/cat.png"
            },
            "title": "Item One"
          },
          {
            "action": {
                "message": {
                  "command": "Pricing",
                  "type": "training"
                }
            },
            "body": "Description Two",
            "image": {
              "alt": "Image of a cat",
              "url": "https://homepages.cae.wisc.edu/~ece533/images/cat.png"
            },
            "title": "Item Two"
          }
        ],
        "title": "List Title"
      }
    }
  ]
}
```
#### Why Use This:
- Compact Display: Efficiently presents multiple options in a clear, structured format.
- Easy Navigation: Helps users quickly find and select relevant items.
- Versatile Actions: Supports linking to external pages or continuing the conversation.

#### Properties

- title: The title displayed for the list.
- items: An array of list items, each with the following properties:
  - title: item title
  - body (optional): A description of the item.
  - image (optional)
    - url: link to media
    - alt: alt text for accessibility
  - action
    - [message](#message)
    - [link](#link)


## Forms


Forms are used to collect structured input from users through various properties. They can include a **title** and **subtitle** for context and are configured differently depending on whether you are using Dialogflow CX or ES.

- **Dialogflow CX**: User inputs are set as session parameters.
- **Dialogflow ES**: User inputs are set as parameters on `botcopy-form-context`.

Forms exclusively use a [message](#message) action to continue the conversation once the form is submitted.

**Field Limitations**:  
- Each field has a maximum length of **256 characters**.  
- At least **one field** is required for the form to render.


#### Usage

- **Collect structured input** such as emails, preferences, or feedback.

#### Best For

- Capturing user information in an organized manner.

#### Example Payload
```json
{
  "botcopy": [
    {
      "form": {
        "title": "Contact Information",
        "subtitle": "Thank you for your business!",
        "style": "message",
        "force": true,
        "fields": [
          {
            "label": "Email Address",
            "placeholder": "hello@botcopy.com",
            "parameter": "email",
            "type": "email",
            "required": true,
            "error": "This field is required.",
            "helperText": "Format: email@domain.com",
            "expose": true
          },
          {
            "fieldType": "select",
            "parameter": "fruits",
            "groupLabel": "Select a fruit below:",
            "expose": true,
            "select": [
              { "selectLabel": "Apple", "parameter": "Apple" },
              { "selectLabel": "Banana", "parameter": "Banana" },
              { "selectLabel": "Orange", "parameter": "Orange" }
            ]
          },
          {
            "fieldType": "checkbox",
            "parameter": "amount",
            "groupLabel": "Select the amount below:",
            "expose": true,
            "checkboxes": [
              { "parameter": "1", "checkboxLabel": "1", "checked": true },
              { "parameter": "2", "checkboxLabel": "2" }
            ]
          }
        ],
        "action": {
          "message": {
            "command": "Pricing",
            "type": "training"
          }
        }
      }
    }
  ]
}

```
#### Why Use This:
Forms streamline data collection, ensure input accuracy, and improve the user experience by guiding users through structured inputs.

#### Properties

| **Property**  | **Type**    | **Description**                                                                                   |
|----------------|-------------|---------------------------------------------------------------------------------------------------|
| `title`       | *string*    | The title of the form.                                                                            |
| `subtitle`    | *string*    | A subtitle providing context for the form.                                                        |
| `style`       | *string*    | Display style of the form component. Properties: `message`, `wall`.                                  |
| `force`       | *boolean*   | *(Optional)* Defaults to `false`. If `true`, the wall style removes the cancel button.            |


### Field-Level Properties

| **Property**   | **Type**     | **Description**                                                                                       |
|-----------------|--------------|-------------------------------------------------------------------------------------------------------|
| `label`         | *string*     | The label shown for the input field.                                                                 |
| `placeholder`   | *string*     | Placeholder text displayed in the field.                                                             |
| `parameter`     | *string*     | The Dialogflow parameter to store the input value.                                                   |
| `type`          | *string*     | *(Optional)* Type of input to collect. Supported types: `email`, `text`, `checkbox`, etc.            |
| `fieldType`     | *string*     | *(Optional)* The type of field element. Properties: `text`, `checkbox`, `radio`, `select`.              |
| `required`      | *boolean*    | *(Optional)* Indicates if the field must be filled to submit the form.                               |
| `error`         | *string*     | *(Optional)* Error message shown if a required field is left empty.                                  |
| `helperText`    | *string*     | *(Optional)* Additional instructions or context for the field.                                       |
| `expose`        | *boolean*    | *(Optional)* If `true`, exposes the field value in the `botcopy-form-submitted` event.               |
| `multiline`     | *boolean*    | *(Optional)* Transforms a text field into a textarea.                                                |
| `rows`          | *number*     | *(Optional)* Minimum number of rows to display for multiline fields.                                 |
| `pattern`       | *string*     | *(Optional)* Regex pattern for input validation.                                                     |


### Supported Input Types for `type`

- `checkbox`
- `date`
- `datetime-local`
- `email`
- `month`
- `week`
- `number`
- `password`
- `tel`
- `text`
- `time`
- `color`
- `url`

### Notes on Regex Patterns

Some characters are reserved in JSON and need to be properly escaped:

- **Backspace**: `\b`  
- **Form feed**: `\f`  
- **Newline**: `\n`  
- **Carriage return**: `\r`  
- **Tab**: `\t`  
- **Double quote**: `\"`  
- **Backslash**: `\\`  

### Checkboxes

Checkboxes contain an array of checkbox fields. You can define each checkbox field and expose values in the bc-form-submitted window event.

#### Properties

- groupLabel: name of dialogflow parameter to assign input to
- fieldType: checkbox
- parameter: name of dialogflow parameter checkbox parameters are assigned to
- expose: exposes the user's input in the bc-form-submitted window event

#### Field Properties

- parameter: name of dialogflow parameter value to assign input to
- checkboxLabel: Label of the checkbox
- checked (optional): Sets checkbox to 'Checked' on load

```
	{
		"fieldType": "checkbox",
		"parameter": "amount",
		"groupLabel": "Select the amount below:",
		"expose": true,
		"checkboxes": [{
				"parameter": "1",
				"checkboxLabel": "1",
				"checked": true
			},
			{
				"parameter": "2",
				"checkboxLabel": "2"
			}
		]
	}
```

### Radio

Radio contains an array of radio buttons. You can define each radio button and expose values in the bc-form-submitted window event.

#### Properties

- expose: exposes the user's input in the bc-form-submitted window event
- fieldType: radio
- groupLabel: name of dialogflow parameter to assign input to
- parameter: name of dialogflow parameter radio parameters are assigned to
- required (optional): radio button is required to submit the form

#### Field Properties

- parameter: name of dialogflow parameter value to assign input to
- radioLabel: Label of the radio
- selected (optional): select a default. Radio buttons should have the most commonly used option selected by default. If no default is selected, `required` should be set to true.

```
	{
		"fieldType": "radio",
		"parameter": "letter",
		"groupLabel": "Select a letter below:",
		"expose": true,
        "required": true,
		"radio": [{
				"parameter": "A",
				"radioLabel": "A",
                "selected": true,
			},
			{
				"parameter": "B",
				"radioLabel": "B"
			}
		]
	}
```


### Select

Select contains menu items within a dropdown menu. You can define each option and expose values in the [bc-form-submitted](#/window/events/bc-form-submitted) window event.

#### Properties

- fieldType: select
- groupLabel: name of dialogflow parameter to assign input to
- parameter: name of dialogflow parameter select parameter is assigned to
- expose: exposes the user's input in the bc-form-submitted window event

#### Field Properties

- parameter: name of dialogflow parameter value to assign input to
- selectLabel: Label of the menu item

```
 {
 	"fieldType": "select",
 	"parameter": "fruits",
 	"groupLabel": "Select a fruit below:",
 	"expose": true,
 	"select": [{
 			"selectLabel": "Apple",
 			"parameter": "Apple"
 		},
 		{
 			"parameter": "Banana",
 			"selectLabel": "Banana"
 		},
 		{
 			"selectLabel": "Orange",
 			"parameter": "Orange"
 		}
 	]
 }
```

**Autocomplete**

The [autocomplete](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete) attribute lets web developers specify what if any permission the user agent has to provide automated assistance in filling out form field values, as well as guidance to the browser as to the type of information expected in the field.

- autocomplete (optional): defines what should populate for autocomplete. Defaults to 'on'
- type (optional): type of the input field
- name (optional): name of the field
- id (optional): id of the field


## Actions

[message](#message) and [link](#link) are used for Suggestions, Cards, Carousel Items, and List Items.

Cards and Carousels can also use a [button](#button) action. Each button has its own action.

Only one action should be assigned to each payload element. In case of conflicting actions, Botcopy priorities message > link > button.

#### Types of Actions
- Message: Continues the conversation within the chat.
- Link: Opens a URL in a new tab or webview.
- Button: Used within cards, combining multiple actions.
#### Choosing the Right Action
- Message Actions are ideal for in-chat responses.
- Link Actions direct users outside the chat.
- Button Actions provide flexibility within cards.

### message

```
{
  "action": {
    "message": {
      "command": "Pricing",
      "type": "training",
      "parameters": {
        "param1": "value1",
        "param2": "value2"
      }
    }
  }
}
```

Sends a message to the bot on selection.

- command (optional): the event name or training phrase sent to the bot. Defaults to title if not provided
- type: either 'event' or 'training'. This corresponds to an event name or training phrase for an intent.
- parameters (optional): an object containing parameters to be sent to the bot. The object key is the parameter name and the value is the parameter value.

### link

```
{
  "action": {
    "link": {
      "target": "_blank",
      "url": "https://botcopy.com"
    }
  }
}
```

Sends the user to a destination url within a new tab, or a Botcopy webview.

- url: the destination url
- target (optional): new tab or webview. Defaults to a Botcopy webview in the chat if not defined.

### button

```
{
  "action": {
    "buttons": [
      {
        "action": {
          "link": {
            "target": "_blank",
            "url": "https://botcopy.com"
          }
        },
        "title": "Linkout Button"
      },
      {
        "action": {
          "message": {
            "command": "Pricing",
            "type": "training"
          }
        },
        "title": "Message Button"
      }
    ]
  }
}
```

An array of buttons, each with an action of their own. Exclusively used for Cards.

- title: the title of the button
- action: defines the behavior of the button. Must be exclusively a single action
  - [message](#message)
  - [link](#link)


