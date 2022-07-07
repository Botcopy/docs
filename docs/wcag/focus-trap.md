## General

Botcopy's chat window is fully compliant with WCAG 2.1 [standards](https://www.w3.org/TR/WCAG21/).

<!-- Conformance Report available [here]().

Audited by [Zenyth Group](https://www.zenythgroup.com/index). -->

## Focus Trap

Focus traps are disabled by default to allow you to [test and implement](https://botcopy.github.io/docs/#/wcag/focus-trap?id=testing) your bot with your existing libraries.

To enable the focus trap you can either:

- Toggle on `Focus Trap` in the Botcopy Portal -> Branding -> Settings -> Chat Window General Settings
- Add and set `data-enableFocusTrap` to `true` on your Botcopy [embed snippet](https://botcopy.github.io/docs/#/basics/connect?id=embed-snippet).

```js
<script
  type="text/javascript"
  id="botcopy-embedder-d7lcfheammjct"
  class="botcopy-embedder-d7lcfheammjct"
  data-botId="yourBotID"
  data-enableFocusTrap="true" // set to true to enable focus trap
>
  var s = document.createElement('script'); s.type = 'text/javascript'; s.async
  = true; s.src = 'https://widget.botcopy.com/js/injection.js';
  document.getElementById('botcopy-embedder-d7lcfheammjct').appendChild(s);
</script>
```

## Testing

Test as you integrate your bot's focus trap with your existing libraries. Some libraries have built-in focus functionality, so it will require some tinkering to ensure there are no endless loops.

To help support this testing, we recommend you use the window methods of [`enableFocusTrap`](https://botcopy.github.io/docs/#/window/methods?id=enable-focus-trap) and [`disableFocusTrap`](https://botcopy.github.io/docs/#/window/methods?id=disable-focus-trap).

In tandem, the window events of [`bc-focus-trap-on`](https://botcopy.github.io/docs/#/window/events?id=bc-focus-trap-on) and [`bc-focus-trap-off`](https://botcopy.github.io/docs/#/window/events?id=bc-focus-trap-off) will fire when these methods are invoked.
