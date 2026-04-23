## General

Botcopy conforms with and supports WCAG 2.2 AA [criteria](https://www.w3.org/TR/WCAG22/ ":target=_blank"), thus helping you meet ADA compliance.

Download our <a href="_assets/VPAT2.5WCAG_BotCopyApril2026.pdf" target="_blank" rel="noopener noreferrer">Botcopy Accessibility Conformance Report</a> for a comprehensive list of the accessibility criteria we support and summary descriptions.

!> **Note:** WCAG 2.1 AA conformance is required for US state and local government websites and mobile apps under the US Department of Justice's April 2024 [final rule](https://www.ada.gov/resources/2024-03-08-web-rule/ ":target=_blank") implementing ADA Title II. Per the DOJ's April 20, 2026 Interim Final Rule, compliance deadlines were extended by one year: April 26, 2027 for public entities serving populations of 50,000 or more, and April 26, 2028 for smaller public entities and special district governments. WCAG 2.1 AA is also legally mandated in several US states, including California, New York, Illinois, and Massachusetts. Failure to comply may result in legal action.

Audited by [Zenyth Group](https://www.zenythgroup.com/index ":target=_blank").

## Focus Trap

Focus traps are disabled by default to allow you to [test and implement](/wcag/focus-trap?id=testing) your bot with your existing libraries.

To enable the focus trap you can either:

- Toggle on `Focus Trap` in the Botcopy Portal -> Branding -> Settings -> Chat Window General Settings
- Add and set `data-enableFocusTrap` to `true` on your Botcopy [embed snippet](/basics/connect?id=embed-snippet).

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

To help support this testing, we recommend you use the window methods of [`enableFocusTrap`](/window/methods?id=enable-focus-trap) and [`disableFocusTrap`](/window/methods?id=disable-focus-trap).

In tandem, the window events of [`bc-focus-trap-on`](/window/events?id=bc-focus-trap-on) and [`bc-focus-trap-off`](/window/events?id=bc-focus-trap-off) will fire when these methods are invoked.
