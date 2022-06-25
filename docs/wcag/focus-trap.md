## General

Botcopy's chat window is fully compliant with WCAG 2.1 [standards](https://www.w3.org/TR/WCAG21/).

Conformance Report available [here]().

Audited by [Zenyth Group](https://www.zenythgroup.com/index).

## Focus Trap

Focus traps are disabled by default to allow you to implement your bot with your existing libraries.

To enable the focus trap:

1. Toggle on `Enforce Focus` in the Botcopy Portal -> Branding
2. Add and set `enforce-focus` to `true` on your Botcopy [embed snippet](http://localhost:3000/#/basics/connect?id=embed-snippet).

```js
<script
  type="text/javascript"
  id="botcopy-embedder-d7lcfheammjct"
  class="botcopy-embedder-d7lcfheammjct"
  data-botId="yourBotID"
  enforce-focus="true" // set to true to enable focus trap
>
  var s = document.createElement('script'); s.type = 'text/javascript'; s.async
  = true; s.src = 'https://widget.botcopy.com/js/injection.js';
  document.getElementById('botcopy-embedder-d7lcfheammjct').appendChild(s);
</script>
```
