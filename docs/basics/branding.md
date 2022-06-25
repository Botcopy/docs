# Branding

Botcopy offers the most customizable chat client on the market today. The **Branding** page on your portal is the master page for the entire look and feel of your bot, and has a live preview so you can see your changes in real time. Every hexcode used in the chat window can be changed in the Branding page. This page also has a number of **pre-styled color themes** available.

## Images

The **Images** section allows you to adjust the images and greeter of your bot. You may upload your own image for the greeter and bot's avatar in the chat here, or choose from a selection of Botcopy's greeter and avatar images. GIFs are also accepted.

You may also change the shape of the greeter and the size of the image within the greeter image field.

## Colors

The colors section gives control over every color used in the chat window. Each may be customized to give you full control of the look of the bot's minimized state, messaging, header, components, and more.

The first panel, **Greeter and Prompts**, deals with the colors of the minimized greeter and prompt bubble experience.

The second panel, **Chat Window**, deals with the maximized chat experience and controls colors of the header, body, and input bar of the chat window.

The final panel, **Components**, controls the colors of [Botcopy's components](https://botcopy.github.io/docs/#/basics/components).

## Settings

The settings section deals with Branding configurations not related to colors.

### Greeter Settings

Select an animation to add some flair to your greeter when it first loads on a website. Choose a speed and timing function to further configure the animation.

### Prompts Settings

Choose to display the greeter and prompts or hide them entirely, and also choose to display suggestion chips under the prompt.

### Chat Window Settings

Select the size of the window, or if it should be rendered fullscreen with [Fullscreen Mode](#fullscreen-mode). You may also change the Z-Index of the chat window here.

### Header Settings

Select from our Header types and choose what functionality is available on the header (Maximize, Clear Conversation, etc.)

### Body Settings

Adjust the font type of your bot, show the Bot's avatar image, and hide the "UI by {••}" tag.

### Input Bar Settings

Choose from Input Bar types and whether the microphone should be displayed. A menu can also be displayed on the input bar.

## Fullscreen Mode

A full-screen chat experience that replaces a website or specific web page(s) within a site. The chat window remains the same, with the exception of removing the _minimize_ and _close_ buttons normally seen on the header. This behavior can be set for an entire bot, or for individual pages of your website by giving the page a Bot Prompt configured to Fullscreen mode.

## Custom Chat Window Styling

**AppendChild Guide**

For further customization of the chat window, custom styles can be added to the script and attached once the shadow DOM is ready.

Note: In order to use this feature the Botcopy Snippet must be at the end of your `<body>`, not in `<head>`.

```
//Botcopy Snippet
<script type="text/javascript"
    id="botcopy-embedder-d7lcfheammjct"
    class="botcopy-embedder-d7lcfheammjct"
    data-botId="BOT_ID_HERE"
>
    var s = document.createElement('script');
    s.type = 'text/javascript'; s.async = true;
    s.src = 'https://widget.botcopy.com/js/injection.js';
    document.getElementById('botcopy-embedder-d7lcfheammjct').appendChild(s);

    //Attach new styles when shadow DOM is ready:
    window.addEventListener('load', function() {
    var botStyles = document.createElement('style');
    // add custom styles, dont forget ";"
    botStyles.innerHTML = '.botcopy-class { background-color: rgba(0,0,0,0.0) !important; }
    .botcopy--avatar-image { margin-right: 40px !important; }';
    // make sure everything has loaded, otherwise you'll get an undefined exception
    document.getElementById('botcopy-widget-root').shadowRoot.appendChild(botStyles);
});
</script>
```
