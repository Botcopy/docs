# Connect
The Connect page of the dashboard handles linking your agent to Dialogflow, auxiliary settings for your bot, and integrations.

## Agent Details
The **Agent Details** card allows you to connect your bot to a specific Dialogflow agent, change the Dialogflow environment, change the bot's display name seen in the chat window's header, and lets you set a dashboard label on the bot's card on the Dashboard.

You may also deactivate the bot, unlink the bot, and delete the bot.

## Embed Snippet
The **Embed Snippet** card gives you two options for embedding the bot to your website. You may choose between a general script and a Google Tag Manager script. For more information on embedding your snippet, see the embed section *add hyperlink to embed section here*.

Sometimes you might encounter an undefined error if you're using a service like Atlassian or Confluence. In that case, add `data-requireJSOff="true"` as an attribute to the snippet.

## Utilities and Third Party Integrations
The connect page also handles added features available to your bot.

### Dashbot Analytics Integration
Botcopy has a native integration with Dashbot for analytics. When setting up your Dashbot instance, select `Botcopy` from the list of platforms. Enter the instance's Dashbot API Key and your bots messages will be routed to Dashbot's live dashboard.

*add image here from old docs*

We'll send rich response types to the live transcript and report intents that are not handled.

### Janis Livechat Integration
Botcopy integrates with [Janis](https://janis.ai/) for livechat handling. In order to connect to this service, follow these steps:

1) Create or sign into your Janis account
2) Select **Add Bot** on your **Account** page
3) Configure your bot and select **Botcopy** from **Bot Builder / Framework**
4) Select the Dialogflow agent you wish to connect to
5) Once setup is complete, save your **Janis API Key** and **Janis Client Key** and enter these values into the two input fields on the Botcopy Portal's Connect page

One benefit of Janis is the Slack app. Any notifications or requests for livechat will be piped into a Slack workspace through the Janis app. Add the **Janis** Slack app to a channel and follow Janis' onboarding to connect your bot.

By default, Janis will notify you when the fallback intent is triggered. You may also choose to be notified by setting an output context named `botcopy_human_takeover` with a lifespan of 1 on an intent in Dialogflow ES.

### Language Selection
By default, your bot detects a user’s preferred browser language and sends it with a Dialogflow query. If your Dialogflow agent supports the user’s browser language, responses will appear in that language. Additionally, the chat window will be localized to that language.

However, if you enable **Language Selection**, your end-users will see that your bot has other languages available. A dropdown menu in the chat window makes it easy for end-users to select their preferred language, and they are prompted when initiating the chat that other languages are available.

## Google Text-to-Speech (TTS)
To enable TTS, you must first enable the TTS API in your Google Cloud Platform console for the desired project. Once the API is enabled, create a **Cloud Text-to-Speech API Key**, and paste this into the Connect page on the Botcopy Portal.