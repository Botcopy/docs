# Chat Components
Botcopy offers some added utility with **Chat Components**.

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
