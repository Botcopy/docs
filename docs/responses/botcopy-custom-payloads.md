# Botcopy Response Payload Guide

## Overview

The following outlines the structure and best practices for Botcopy responses, detailing how to use each response type effectively and the optimal formats for different bot interactions. Botcopy’s flexible payload structure supports combining multiple actions within a single response, enhancing the overall user experience.

Examples:
- Suggestions: Provide quick options to send messages or external links.
- Card Buttons: Enable users to trigger messages or open links.
- Carousels: Support mixed actions for versatile interactions:
  - Link Cards: Act as browse carousel cards for external navigation.
  - Message Cards: Serve as selection key carousel cards for in-chat navigation.

[Explore Actions in Detail](#actions)

**Response Types:**
- [Text Responses](#text-responses)
- [Suggestions](#suggestions)
- [Basic Cards](#basic-cards)
- [Carousels](#carousels)
- [Lists](#lists)
- [Forms](#forms)

## Botcopy Payload Implementation

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

The displayText, textToSpeech, and ssml fields handle different output formats for the message.

**Usage:**
- Ideal for straightforward messages or confirmations.

**Best For:**
- Simple instructions, greetings, or responses that don’t require interactivity.

**Why Use This:**
- The simplest way to deliver static information without the need for buttons, links, or user input.

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

#### Properties

| **Name**         | **Type**   | **Description**                                                                                          |
|-------------------|------------|----------------------------------------------------------------------------------------------------------|
| `displayText`     | `string`   | The message displayed in the chat.                                                                       |
| `ssml`            | `string`   | *(Optional)* SSML-formatted text for the phrase spoken by a bot.                                         |
| `textToSpeech`    | `string`   | *(Optional)* The phrase spoken by the bot. Takes priority over `ssml`. If both are provided, `textToSpeech` is used for the audio file.  |



## Suggestions

Suggestions provide users with clickable buttons to guide them through the conversation or direct them to external resources. Each suggestion performs a specific action, either continuing the dialogue with a [message](#message) action or opening an external link with a [link](#link) action.

**Usage:**
- Offer quick, clickable options after a response to streamline user interaction.

**Best For:**
- Guiding users toward common follow-ups or linking to external resources.

**Why Use This:**
- Simplifies Navigation: Reduces the need for typing by offering pre-set choices.
- Enhances Efficiency: Helps users quickly access relevant information or actions.
- Improves Satisfaction: Streamlined interactions lead to a smoother user experience.

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
          "title": "Message Suggestion",
          "ariaLabel": "Learn about pricing options"
        },
        {
          "action": {
            "link": {
              "target": "_blank",
              "url": "https://botcopy.com"
            }
          },
          "title": "Link Suggestion",
          "ariaLabel": "Visit Botcopy website"
        }
      ]
    }
  ]
}
```

#### Properties

| **Name**    | **Type**           | **Description**                                                                                  |
|--------------|--------------------|--------------------------------------------------------------------------------------------------|
| `title`      | `string`           | The text displayed on the suggestion button.                                                     |
| `action`     | `object`           | Defines what happens when the suggestion is clicked. Options include:                           |
|              |                    | - **[message](#message)**: Continues the conversation with a predefined command.                |
|              |                    | - **[link](#link)**: Opens an external URL in a new window or tab.                              |
| `ariaLabel`  | `string`           | *(Optional)* Custom accessibility label for screen readers. Overrides the default aria-label (which defaults to the title text). |


## Basic Cards

Basic cards present structured information using a title, subtitle, body, and optional media like images, GIFs, and [videos](/responses/videos). They can also include [button](#button) actions for user interaction.

**Usage:**
- Display detailed information with a visual layout, and provide interactive buttons for next steps.

**Best For:**
- Highlighting key information and offering follow-up actions with buttons.

**Why Use This:**
- Visual Structure: Organizes content in a clear, visually appealing format.
- User Engagement: Buttons guide users to take specific actions.
- Versatility: Ideal for displaying information, media, and actionable next steps.

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

#### Properties

| **Name**       | **Type**   | **Description**                                                                                  |
|-----------------|------------|--------------------------------------------------------------------------------------------------|
| `title`         | `string`   | Main heading for the card.                                                                       |
| `subtitle`      | `string`   | Subheading to provide additional context. *(Optional)*                                           |
| `body`          | `string`   | Detailed text or description. *(Optional)*                                                       |
| `image`         | `object`   | Contains media information. *(Optional)*                                                         |
| ├─ `url`        | `string`   | Link to the media.                                                                               |
| └─ `alt`        | `string`   | Alternative text for accessibility. *(Optional)*                                                 |
| `action`        | `object`   | Defines the behavior of the card. Options include:                                               |
|                 |            | - **[message](#message)**                                                                        |
|                 |            | - **[link](#link)**                                                                              |
|                 |            | - **[button](#button)**                                                                          |


## Carousels

Carousels present multiple cards in a scrollable format, each with a title, subtitle, body, and optional media. Each card can include an action such as [message](#message), [link](#link), or [button](#button).

**Usage:**
- Display a series of cards that users can swipe or scroll through to explore multiple options.

**Best For:**
- Showcasing products, services, or options with interactive actions on each card.

**Why Use This:**
- Interactive Display: Allows users to explore multiple options without overwhelming them.
- Flexibility: Each card can lead to different actions (e.g., links, messages, or buttons).
- Engagement: Visually appealing and easy to navigate, enhancing user experience.

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

#### Properties

| **Name**        | **Type**   | **Description**                                                                               |
|-----------------|------------|-----------------------------------------------------------------------------------------------|
| `title`         | `string`   | Main heading for each card.                                                                   |
| `subtitle`      | `string`   | Subheading for additional context. *(Optional)*                                               |
| `body`          | `string`   | Description or detailed text for the card. *(Optional)*                                       |
| `image`         | `object`   | Contains image or media information. *(Optional)*                                             |
| ├─ `url`        | `string`   | Link to the image or media.                                                                   |
| └─ `alt`        | `string`   | Alternative text for accessibility. *(Optional)*                                              |
| `action`        | `object`   | Defines the behavior of the card. Options include:                                            |
|                 |            | - **[message](#message)**                                                                     |
|                 |            | - **[link](#link)**                                                                           |
|                 |            | - **[button](#button)**                                                                       |


## Lists

Lists display multiple items in a structured format. Each list item can include a title, optional description, optional image, and an action such as a message or link. Unlike other components, lists do not support button actions.

**Usage:**
- Present multiple options or categories in a clear, organized list.

**Best For:**
- Offering concise lists of items or choices that users can interact with.

**Why Use This:**
- Compact Display: Efficiently presents multiple options in a clear, structured format.
- Easy Navigation: Helps users quickly find and select relevant items.
- Versatile Actions: Supports linking to external pages or continuing the conversation.

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

#### Properties

| **Name**       | **Type**      | **Description**                                                                                      |
|----------------|---------------|------------------------------------------------------------------------------------------------------|
| `title`        | `string`      | The title displayed at the top of the list.                                                         |
| `items`        | `array`       | An array containing individual list items, each with the following properties:                      |
| ├─ `title`     | `string`      | The main title of the list item.                                                                    |
| ├─ `body`      | `string`      | Additional description or details about the item. *(Optional)*                                      |
| ├─ `image`     | `object`      | Contains image or media details for the item. *(Optional)*                                          |
| │ ├─ `url`     | `string`      | The URL link to the image or media file.                                                            |
| │ └─ `alt`     | `string`      | Alternative text for the image, improving accessibility. *(Optional)*                               |
| └─ `action`    | `object`      | Specifies the action triggered when the user interacts with the item. Options include:              |
|                |               | - **[message](#message)**                 |
|                |               | - **[link](#link)**                                |



## Forms


Forms are used to collect structured input from users through various properties. They can include a **title** and **subtitle** for context and are configured differently depending on whether you are using Dialogflow CX or ES.

- **Dialogflow CX**: User inputs are set as session parameters.
- **Dialogflow ES**: User inputs are set as parameters on `botcopy-form-context`.

Forms exclusively use a [message](#message) action to continue the conversation once the form is submitted.

**Field Limitations**:  
- Each field has a maximum length of **256 characters**.  
- At least **one field** is required for the form to render.


**Usage**

- **Collect structured input** such as emails, preferences, or feedback.

**Best For**

- Capturing user information in an organized manner.

**Why Use This:**
Forms streamline data collection, ensure input accuracy, and improve the user experience by guiding users through structured inputs.

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

#### Parameters


#### Properties

| **Name**  | **Type**    | **Description**                                                                                   |
|----------------|-------------|---------------------------------------------------------------------------------------------------|
| `title`       | *string*    | The title of the form.                                                                            |
| `subtitle`    | *string*    | A subtitle providing context for the form.                                                        |
| `style`       | *string*    | Display style of the form component. Properties: `message`, `wall`.                                  |
| `force`       | *boolean*   | *(Optional)* Defaults to `false`. If `true`, the wall style removes the cancel button.            |


#### Field-Level Properties

| **Name**   | **Type**     | **Description**                                                                                       |
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

### How to Read Data from Form Submissions

Once a form is submitted, the data will be returned in the parameters of your intent response. 

For example, if a user has this set to their payload;

```json
{
  "botcopy": [
    {
      "form": {
        "force": true,
        "action": {
          "message": {
            "type": "training",
            "command": "Pricing"
          }
        },
        "fields": [
          {
            "parameter": "email",
            "expose": true,
            "error": "This field is required.",
            "pattern": "^[^\\s@]+@[^\\s@]+\\.[^\\s@]+$",
            "required": true,
            "label": "Email Address",
            "fieldType": "text",
            "placeholder": "matt@botcopy.com",
            "type": "email"
          },
          {
            "error": "This field is required.",
            "fieldType": "text",
            "label": "Phone Number",
            "type": "tel",
            "required": false,
            "placeholder": "(214) 555-1423",
            "parameter": "phoneNumber",
            "expose": false
          },
          {
            "select": [
              {
                "selectLabel": "Web Development",
                "parameter": "web_development"
              },
              {
                "selectLabel": "Mobile App Development",
                "parameter": "mobile_development"
              },
              {
                "parameter": "ai_integration",
                "selectLabel": "AI Integration"
              },
              {
                "parameter": "consulting",
                "selectLabel": "Consulting Services"
              },
              {
                "selectLabel": "E-commerce Solutions",
                "parameter": "ecommerce"
              }
            ],
            "parameter": "services",
            "required": false,
            "groupLabel": "Services Interested In",
            "expose": true,
            "fieldType": "select"
          },
          {
            "parameter": "communication_preferences",
            "required": false,
            "fieldType": "checkbox",
            "checkboxes": [
              {
                "parameter": "comm_email",
                "checkboxLabel": "Email Updates",
                "checked": false
              },
              {
                "checkboxLabel": "SMS Notifications",
                "checked": false,
                "parameter": "comm_sms"
              },
              {
                "checkboxLabel": "Phone Calls",
                "parameter": "comm_phone",
                "checked": false
              },
              {
                "checkboxLabel": "Newsletter",
                "parameter": "comm_newsletter",
                "checked": false
              }
            ],
            "expose": true,
            "label": "Preferred Communication Methods"
          },
          {
            "required": true,
            "expose": false,
            "error": "You must accept the terms and conditions.",
            "fieldType": "checkbox",
            "label": "I agree to the Terms and Conditions",
            "parameter": "terms"
          }
        ],
        "style": "message",
        "subtitle": "Thank you for your business!",
        "title": "Contact Information"
      }
    }
  ]
}
```

Once this form is submitted, I can find this data in the response. 

**Example User Selections:**
- Email: `matt@botcopy.com`
- Phone Number: `604-123-4567` 
- Services: Selected "Consulting Services" (maps to `consulting`)
- Communication Preferences: Selected all checkboxes (Email Updates, SMS Notifications, Phone Calls, Newsletter)
- Terms: Accepted the terms and conditions

**Resulting Response Parameters:**

```json
{
    "intentResponse": {
        "responseId": "36273a13-f860-4fcf-917f-731fd139ae1a",
        "queryResult": {
            ...
            "parameters": {
                "bcBotId": "6791867a4d92eba9b1d99574",
                "email": "matt@botcopy.com",
                "bcTime": "7:13:32 PM",
                "bcLivechatAgent": false,
                "communication_preferences": {
                    "comm_phone": true,
                    "comm_sms": true,
                    "comm_newsletter": true,
                    "comm_email": true
                },
                "phoneNumber": "604-123-4567`",
                "formVariant": "basicForm",
                "bcTimeZone": "America/Vancouver",
                "services": "consulting",
                "terms": {},
                "bcCurrentUrl": {
                    "origin": "https://widget.botcopy.org:3020",
                    "path": "/embed.html",
                    "url": "https://widget.botcopy.org:3020/embed.html?botid=67d219de2dbf1377c8e2b0fe&envurl=widget.botcopy.org:3020"
                }
            },
            ...
        },
        ...
    }
}
```

### Checkboxes

Checkboxes contain an array of checkbox fields. You can define each checkbox field and expose values in the bc-form-submitted window event.

#### Properties

| **Name**       | **Type**      | **Description**                                                                       |
|----------------|---------------|---------------------------------------------------------------------------------------|
| `groupLabel`   | `string`      | The name of the Dialogflow parameter to assign input to.                             |
| `fieldType`    | `string`      | Specifies the field type; in this case, it is a `checkbox`.                          |
| `parameter`    | `string`      | The name of the Dialogflow parameter the checkbox input is assigned to.              |
| `expose`       | `boolean`     | Exposes the user's input in the `bc-form-submitted` window event.                    |


#### Field Properties

| **Name**        | **Type**     | **Description**                                                               |
|-----------------|--------------|-------------------------------------------------------------------------------|
| `parameter`     | `string`     | The name of the Dialogflow parameter to assign the input value to.           |
| `checkboxLabel` | `string`     | The label displayed next to the checkbox.                                    |
| `checked`       | `boolean`    | Sets the checkbox to 'Checked' by default when the form loads. *(Optional)* |


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

When submitted, the values in the response will look like this;
```json
{
    "amount": {
        "1": true,
        "2": false
    }
}
```

### Radio

Radio contains an array of radio buttons. You can define each radio button and expose values in the bc-form-submitted window event.

#### Properties

| **Name**       | **Type**     | **Description**                                                                                     |
|----------------|--------------|-----------------------------------------------------------------------------------------------------|
| `expose`      | `boolean`    | Exposes the user's input in the `bc-form-submitted` window event.                                   |
| `fieldType`   | `string`     | Specifies the field type; in this case, it is a `radio` button.                                     |
| `groupLabel`  | `string`     | The name of the Dialogflow parameter to assign the input to.                                         |
| `parameter`   | `string`     | The name of the Dialogflow parameter the radio button input is assigned to.                         |
| `required`    | `boolean`    | Specifies if selecting a radio button is mandatory to submit the form. *(Optional)*                 |


#### Field Properties

| **Name**        | **Type**     | **Description**                                                                                                 |
|-----------------|--------------|-----------------------------------------------------------------------------------------------------------------|
| `parameter`     | `string`     | The name of the Dialogflow parameter to assign the input value to.                                             |
| `radioLabel`    | `string`     | The label displayed next to the radio button.                                                                  |
| `selected`      | `boolean`    | Sets the radio button to be selected by default. If no default is selected, `required` should be set to `true`. *(Optional)* |

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

When submitted, the values in the response will look like this;
```json
{
    "letter": "A"
}
```


### Select

Select contains menu items within a dropdown menu. You can define each option and expose values in the [bc-form-submitted](#/window/events/bc-form-submitted) window event.

#### Properties

| **Name**       | **Type**     | **Description**                                                                                      |
|----------------|--------------|------------------------------------------------------------------------------------------------------|
| `fieldType`   | `string`     | Specifies the field type; in this case, it is a `select` dropdown.                                  |
| `groupLabel`  | `string`     | The name of the Dialogflow parameter to assign the selected input to.                                |
| `parameter`   | `string`     | The name of the Dialogflow parameter the selected value is assigned to.                              |
| `expose`      | `boolean`    | Exposes the user's input in the `bc-form-submitted` window event.                                     |


#### Field Properties

| **Name**        | **Type**   | **Description**                                                      |
|-----------------|------------|----------------------------------------------------------------------|
| `parameter`     | `string`   | The name of the Dialogflow parameter to assign the selected value to.|
| `selectLabel`   | `string`   | The label displayed for the menu item in the dropdown.               |


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

When submitted, the values in the response will look like this (assuming "Apple" was selected);
```json
{
    "fruits": "Apple"
}
```

**Autocomplete**

The [autocomplete](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete) attribute lets web developers specify what if any permission the user agent has to provide automated assistance in filling out form field values, as well as guidance to the browser as to the type of information expected in the field.

#### Properties
| **Name**         | **Type**    | **Description**                                                                                  |
|------------------|-------------|--------------------------------------------------------------------------------------------------|
| `autocomplete`   | `string`    | Defines the autocomplete behavior. Defaults to `'on'`. *(Optional)*                             |
| `type`           | `string`    | Specifies the type of the input field (e.g., `text`, `email`, `number`). *(Optional)*           |
| `name`           | `string`    | The name attribute for the input field. *(Optional)*                                            |
| `id`             | `string`    | The unique identifier for the input field. *(Optional)*                                         |


## Actions

[message](#message) and [link](#link) are used for Suggestions, Cards, Carousel Items, and List Items.

Cards and Carousels can also use a [button](#button) action. Each button has its own action.

Only one action should be assigned to each payload element. In case of conflicting actions, Botcopy priorities message > link > button.

#### Types of Actions
- [Message](#message): Continues the conversation within the chat.
- [Link](#link): Opens a URL in a new tab or webview.
- [Button](#button): Used within cards, combining multiple actions.

#### Choosing the Right Action
- Message Actions are ideal for in-chat responses.
- Link Actions direct users outside the chat.
- Button Actions provide flexibility within cards & carousels.
--- 

### message

Triggers a predefined command or training phrase when the user selects the option, sending a message to the bot along with optional parameters.

**Usage**: 
- This action helps guide the conversation by invoking specific bot responses or intents.

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


#### Properties
| **Name**         | **Type**           | **Description**                                                                                                     |
|------------------|--------------------|---------------------------------------------------------------------------------------------------------------------|
| `type`           | `string`           | Specifies the type of action: either `'event'` for an event name or `'training'` for a training phrase.            |
| `command`        | `string`           | *(Optional)* The event name or training phrase sent to the bot. Defaults to the `title` if not provided.           |
| `parameters`     | `object`           | An object containing parameters sent to the bot. Each key represents a parameter name, and the value is its value. *(Optional)*  |


--- 

### link

Opens a destination URL in a new tab or a Botcopy webview when the user selects the option.

**Usage**: 
- This action is useful for directing users to external resources or detailed information outside the chat interface.

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


#### Properties
| **Name**     | **Type**   | **Description**                                                                                           |
|--------------|------------|-----------------------------------------------------------------------------------------------------------|
| `url`        | `string`   | The destination URL that the link points to.                                                             |
| `target`     | `string`   | Specifies where to open the link: a new tab or a webview. Defaults to a Botcopy webview if not provided. *(Optional)* |

--- 
### button
An array of buttons, each with its own action. Buttons are exclusively used within Cards & Carousels to offer interactive options.

**Usage**:
- Use buttons to guide users toward next steps, such as opening external links or continuing the conversation with specific commands.

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

#### Properties

| **Name** | **Type** | **Description**                                                       |
|----------|----------|-----------------------------------------------------------------------|
| `title`  | `string` | The text displayed on the button.                                     |
| `action` | `object` | Defines the behavior of the button. Must be a single action, such as: |
|          |          | - **[message](#message)**                                             |
|          |          | - **[link](#link)**                                                   |

## AgentOne: Bot Card

The Bot Card lets you add a card to your chat that contains a button for switching the conversation to a different Botcopy bot.

The bot’s name and avatar are filled in automatically, so you don’t have to set them yourself.

**Usage:**

Display a card in the chat with a customizable action button.

Automatically show the target bot’s name and avatar pulled from the widget.

Trigger an instant bot switch when the button is clicked.

**Best For:**

Guiding users to a different Botcopy bot for specific topics or services.

Promoting another bot in your organization without manual setup for name/avatar.

**Why Use This:**

Customizable Call-to-Action: Choose your own button text to match the context (e.g., “Talk to Sales,” “Switch to Support”).

Automatic Branding: No need to upload images or type the bot name — the widget handles it.

Built-In Validation: Ensures only valid bot IDs render the card, avoiding broken experiences.

#### Dialogflow Payload (What you configure)

```json
{
  "botcopy": [
    {
      "botCard": {
        "buttonText": "Change to Sales Bot",
        "botId": "bot_sales_01"
      }
    }
  ]
}
```

#### Dialogflow Payload With Multiple Bot Cards

```json
{
  "botcopy": [
    {
      "botCard": {
        "buttonText": "Change to Sales Bot",
        "botId": "bot_sales_01"
      },
      "botCard": {
        "buttonText": "Change to Support Bot",
        "botId": "bot_support_02"
      }
    }
  ]
}
```

#### Properties

| **Name**     | **Type** | **Description**                   |
| ------------ | -------- | --------------------------------- |
| `buttonText` | `string` | The text displayed for the button |
| `botId`      | `string` | Reference to a bot                |

#### Important Notes

- **Organization Scope**: Bot card can only switch to bots from the same organization as the requesting bot.
