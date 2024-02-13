# Botcopy Response Payload Format for Dialogflow CX

Botcopy responses are designed to be flexible in their role and this is reflected in the payload structure. Components in the payload have an action field that dictates their behavior in the Botcopy chat when the component is selected by users. It allows multiple response types with different actions to be grouped together.

Examples:

- Suggestions can be used to send messages or links
- Card Buttons can send messages or links
- Carousels can contain more than one type of action
  - Link cards act as browse carousel cards
  - Message cards act as regular selection key carousel cards

[Actions in Detail](#actions)
[Response Payload Details](#response-payload-structure-details)

- [Text Responses](#text-responses)
- [Suggestions](#suggestions)
- [Basic Cards](#basic-cards)
- [Carousels](#carousels)
- [Lists](#lists)
- [Forms](#forms)

## Response Payload Structure Details

Each custom payload should contain a `botcopy` array with your desired rich responses.

Default CX text responses will render alongside your payload if they are provided.

Custom Rich Responses will be rendered in the chat in their arrayed order.

To add a custom payload to a CX response, select the 'Custom Payload' fulfillment option.

## Text Responses

An array of text responses.

textToSpeech, displayText, and ssml fields handle different aspects of the message.

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

**text**

- displayText - the message displayed in the chat.
- ssml (optional) - SSML format text for the phrase spoken by a bot.
- textToSpeech (optional) - the phrase spoken by the bot. Takes priority over SSML. If both are provided in the payload, textToSpeech is used for the audio file.

---

## Suggestions

Suggestions prompt users with a clickable button after a series of responses. A suggestion can either continue the conversation with a **message** action, or send the user to an external link with a **link** action.

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

**suggestion**

- title: the label displayed in the suggestion chip to the end user
- action: defines the behavior of the suggestion. Must be exclusively a single action
  - [message](#message)
  - [link](#link)

---

## Basic Cards

Basic cards have a title, subtitle, and body containing information. Image media includes, but is not limited to images, gifs and [videos](/responses/videos).

Cards can also use a [button](#button) action, which is displayed in this example.

```
{
  "botcopy": [
    {
      "card": {
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
        },
        "body": "Card body",
        "image": {
          "alt": "Image of a cat",
          "url": "https://homepages.cae.wisc.edu/~ece533/images/cat.png"
        },
        "subtitle": "Card subtitle",
        "title": "Card title"
      }
    }
  ]
}
```

**card**

- title: title for the card
- image (optional): object containing media info
  - url: link to media
  - alt (optional): alt text for accessibility
- body (optional): body text for the card
- subtitle (optional): subtitle for the card
- action
  - [message](#message)
  - [link](#link)
  - [button](#button)

---

## Carousels

Carousels are a collection of cards. Each carousel item can have an action of either [message](#message), [link](#link), or [button](#button).

```
{
  "botcopy": [
    {
      "carousel": [
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
          "subtitle": "Subtitle One",
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
      ]
    }
  ]
}
```

**carousel**

- title: title for the card
- image (optional): object containing media info
  - url: link to media
  - alt: alt text for accessibility
- body (optional): body text for the card
- subtitle (optional): subtitle for the card
- action
  - [message](#message)
  - [link](#link)
  - [button](#button)

---

## Lists

Lists are another way to visualize an assortment of data. Lists do not use a button action.

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

**list**

- title: list title shown in header
- items: array of list items
  - image (optional)
    - url: link to media
    - alt: alt text for accessibility
  - title: item title
  - description (optional): description of list item
  - action
    - [message](#message)
    - [link](#link)

---

## Forms

Forms contain fields for user input. These fields come in a variety of fieldTypes and associated attributes. You can also add a title and subtitle to your forms.

Dialogflow ES: User inputs are set as a parameter on `botcopy-form-context`

Dialogflow CX: User inputs are set as session parameters

Forms exclusively use a [message](#message) action, which is the training phrase or event used to continue the conversation when the form is filled.

Fields have a max length of 256 characters. At least one field is required for the component to render.

```
{
	"botcopy": [{
		"form": {
			"fields": [{
					"label": "Email Address",
					"placeholder": "matt@botcopy.com",
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
				},
				{
					"groupLabel": "Select the amount below:",
					"fieldType": "checkbox",
					"parameter": "amount",
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
			],
			"style": "message",
			"title": "Contact Information",
			"subtitle": "Thank you for your business!",
			"force": true,
			"action": {
				"message": {
					"command": "Pricing",
					"type": "training"
				}
			}
		}
	}]
}
```

**Form Attributes**

- title | string: title of the form
- subtitle | string: subtitle of the form
- style | string: style of the form component
  - message, wall
- fieldType | string: Type of the field (< input type=""> element)
  - text, checkbox, radio, select
- force (optional) | boolean: defaults to false.
  - If true, the wall style does not have a cancel button
  - Does not apply to forms with style: "message"
- fields: array of objects
  - error | string: text shown if a required field is not filled
  - helperText | string: used to give context about a field's input, such as how the input will be used or its current error state.
  - label | string: text input label
  - placeholder | string: input placeholder
  - parameter | string: name of dialogflow parameter to assign input to
  - expose (optional) | boolean: add the parameter name and field value to the `botcopy-form-submitted` window event
  - multiline (optional) | boolean: transforms text field into a textarea element. Unless rows is set, the height of the text field dynamically matches its content
  - rows (optional) | number: Minimum number of rows to display when multiline option is set to true
  - required (optional) | boolean: field required to submit form
  - pattern (optional)\* | string: regex patterns
  - type (optional) | string: type of input to be collected
    - Supported types: checkbox, date, datetime-local, email, month, week, number, password, tel, text, time, color, url
    - Unsupported types: file, image, built-in calendar, radio, button, hidden, range, reset, search, submit

\*Note on regex patterns: The following characters are reserved characters and can not be used in JSON and must be properly escaped to be used in strings.

- Backspace to be replaced with \b
- Form feed to be replaced with \f
- Newline to be replaced with \n
- Carriage return to be replaced with \r
- Tab to be replaced with \t
- Double quote to be replaced with \"
- Backslash to be replaced with \\

### Checkboxes

Checkboxes contain an array of checkbox fields. You can define each checkbox field and expose values in the bc-form-submitted window event.

Checkboxes object attributes

- groupLabel: name of dialogflow parameter to assign input to
- fieldType: checkbox
- parameter: name of dialogflow parameter checkbox parameters are assigned to
- expose: exposes the user's input in the bc-form-submitted window event

Checkbox field attributes

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

Radio object attributes

- expose: exposes the user's input in the bc-form-submitted window event
- fieldType: radio
- groupLabel: name of dialogflow parameter to assign input to
- parameter: name of dialogflow parameter radio parameters are assigned to
- required (optional): radio button is required to submit the form

Radio button attributes

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

Select contains menu items within a dropdown menu. You can define each option and expose values in the bc-form-submitted window event.

Select object attributes

- fieldType: select
- groupLabel: name of dialogflow parameter to assign input to
- parameter: name of dialogflow parameter select parameter is assigned to
- expose: exposes the user's input in the bc-form-submitted window event

Select field attributes

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

Card can also use a [button](#button) action. Each button has its own action.

Only one action should be assigned to each payload element. In case of conflicting actions, Botcopy priorities message > link > button.

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

---