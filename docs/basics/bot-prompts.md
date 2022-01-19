# Bot Prompts
Bot Prompts are the first messages users see that "prompt" users to engage with your bot. Bot Prompts are a call to action, ideally within the **context** of the current page the bot is on.

For example, a page for Pricing might have a different call to action than a page for Support.

*todo: add image of prompt and greeter*

Botcopy offers different types of prompts and settings to better suit the needs of a variety of use cases.

## Intent Prompts
**Intent Prompts** are the default prompt type. Intent Prompts query Dialogflow with a **Trigger**, which is the Training Phrase or Event Name of an intent you've created on your Dialogflow agent.

The trigger will only fire on the specified **Page URL**, creating a contextualized call to action to your users.

## Preset Prompts
**Preset Prompts** differ in that they do not send a query to Dialogflow. Instead, you provide a **Message** and optional **Suggestions** and **Output Contexts**. Preset Prompts are best for high-traffic websites and can save considerable overhead with Dialogflow.

The message is rendered on the provided **Page URL**, but a query is not sent to Dialogflow until your user engages with the bot by typing a message or selecting a suggestion.

Each suggestion has a **Label** and **Trigger**. The trigger is similarly linked to a Training Phrase or Event Name.

## Default Prompt and Behavior
The first prompt in your Portal is the **Default Prompt**. This is your fallback prompt which will display on any pages not set up with a prompt.

To change your default prompt, select the button at the bottom of the prompt's card. You may then select one of your other existing prompts to replace it.

You may decide you don't want the default prompt to display on pages without prompts. In that case, you can configure it so the Greeter shows without a prompt bubble.

## Prompt Settings
Each prompt has a number of settings that may also be configured.

- **CUI Mode** enables a full screen experience for that prompt only.
- **Devices** allow you to choose which prompts show on which devices. You may choose a prompt to only show on Mobile, for example. This configuration is also available for individual preset prompt suggestions.
