# Bot Prompts
A Bot Prompt is the small bubble that initially appears next to the bot greeter icon, "prompting" the end-user to engage with the bot by clicking on it and opening up the chat window. Bot Prompts function best when they are different for each page and are a call to action that targets the end user's likely intentions within the context of the page they are on.

For example, a bot prompt within a Pricing page might have a call to action such as "Click me to find the best pricing plan to meet your needs." This would be a very different initial message than, say, the Bot Prompt message for your About Us page, which might say "Have more questions about us? I can answer them or set up a meeting." 

*todo: add image of prompt and greeter*

Botcopy offers different types of Bot Prompts as well as ways to configure each Bot Prompt to suit the needs of your use case.

## Intent Prompts
**Intent Prompts** are the default prompt type. They query Dialogflow with a **Trigger**, which is the Training Phrase or Event Name of an intent you've created within your Dialogflow agent.

The trigger will only fire on the specified **Page URL**, creating a contextualized call to action to your users.

## Preset Prompts
**Preset Prompts** differ from Intent Prompts in that they do not send an initial query to Dialogflow. Instead, within Botcopy you will provide a **Message** and optional **Suggestions** and **Output Contexts**. Dialogflow will only be queried when the end-user sends a message or selects a suggestion through the bot. 

Similar to Intent Prompts, for each Preset Prompt the message is rendered on the provided **Page URL**. Each suggestion you add to a Preset Prompt has a **Label** and **Trigger**. The trigger is linked to a Training Phrase or Event Name within your Dialogflow agent.

**IMPORTANT**: Preset Prompts are a must for high-traffic websites because, unlike with Intent Prompts, Preset prompts don't query Dialogflow – and thus do not incur a Google Cloud Dialogflow charge – until your end-user chooses to engage. 

## Default Bot Prompt
The first Bot Prompt that you set up in your Botcopy Bot Prompts page within the Botcopy Portal will automatically serve as your **Default Prompt**, which simply means that this prompt will be used for any Page URL within your site that currently lacks a contextual Bot Prompt. 

However, if you have mulitple Bot Prompts set up, you can easily reassign which one will serve as the **Default Prompt.** At the bottom of the prompt's card, select the button **Change Default Prompt**. You may then assign one of your other prompts to be the **Default Prompt.** 

Additionally, you may not want the Default Prompt to display on Page URLs within your site that currently lack a contextual Bot Prompt. At the top of the Bot Prompts page, you can set the **Prompt Settings** toggle to **Greeter** so that pages without a prompt will only show a greeter icon. 

## Bot Prompt Settings
Each Bot Prompt has a number of settings that may also be configured.

- Botcopy's **CUI Mode** enables a full-screen experience for that prompt only. This especially comes in handy for mobile experiences, or any situation where you want to make a conversational UX mandatory, or the default UX for a given page or project. 
- Botcopy's **Devices** allow you to choose which Bot Prompts show on mobile devices and desktop devices. You may assign any prompt to only show on Mobile, or only show on desktop; or both, or neither. 

The **Devices** configuration options can also be applied to **Suggestion Chips** within **Preset Prompts**. (This comes in handy for Mobile, when sometimes suggestion chips under a Bot Prompt will obscure Mobile web content, but look fine on Desktop.)  
