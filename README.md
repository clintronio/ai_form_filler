# Voice Form Assistant

A Tampermonkey userscript that adds a voice recording widget to any web form. Speak notes and Claude AI automatically extracts and suggests values for the visible form fields.

![Version](https://img.shields.io/badge/version-1.3.0-blue)
![Platform](https://img.shields.io/badge/platform-Tampermonkey-brightgreen)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

## Demo

> Open any web form → click the green mic FAB → speak → Claude maps your words to the form fields → click Apply.

---

## How it works

1. A microphone button sits fixed in the bottom-right corner of every page
2. Click it to open the assistant panel and hit **Start Recording**
3. Speak naturally — names, emails, company details, statuses, whatever is relevant
4. When you stop, the live transcript is sent to Claude along with every visible field on the page (name, label, and valid options for dropdowns)
5. Claude returns a list of suggested field values — apply them individually or all at once with a single click

The script reads fields directly from the live DOM so it works on any form without configuration — standard inputs, selects, and textareas are all supported.

---

## Prerequisites

- [Tampermonkey](https://www.tampermonkey.net/) installed in Chrome, Edge, or Firefox
- An [Anthropic API key](https://console.anthropic.com/)

---

## Installation

**1. Install Tampermonkey**

Download from [tampermonkey.net](https://www.tampermonkey.net/) and add it to your browser.

**2. Add your API key**

Open `voice-form-assistant.user.js` and replace the placeholder near the top of the file:

```js
const ANTHROPIC_API_KEY = 'your-api-key-here';
```

**3. Install the script**

Option A — drag the `.user.js` file onto the Tampermonkey dashboard.

Option B — open the Tampermonkey dashboard → **Create new script** → paste the contents → save.

**4. Reload the page**

Hard refresh with `Ctrl+Shift+R` (Windows/Linux) or `Cmd+Shift+R` (Mac). The green mic button will appear in the bottom-right corner.

---

## Verifying it's running

Open DevTools (`F12`) → **Console** and look for:

```
[VA] userscript loaded, href: https://...
[VA] bootstrap(), body ready: true
[VA] injecting widget
[VA] widget injected OK
```

If none of these appear, confirm the script is **enabled** in the Tampermonkey dashboard.

---

## Configuration

Both options are at the top of the script:

| Constant | Default | Description |
|---|---|---|
| `ANTHROPIC_API_KEY` | — | Your Anthropic API key |
| `ANTHROPIC_MODEL` | `claude-sonnet-4-6` | Claude model to use |

---

## Limiting to specific sites

By default the script runs on all `https://` and `http://` pages. To restrict it to specific domains, replace the `@match` lines in the script header:

```js
// @match  https://crm.zoho.com/*
// @match  https://app.hubspot.com/*
// @match  https://your-internal-tool.com/*
```

Or use Tampermonkey's built-in **Includes/Excludes** settings per-script without editing the file.

---

## Why Tampermonkey bypasses CORS

The Anthropic API blocks direct `fetch()` calls from browser pages due to CORS headers — the same reason `curl` works but a plain page `fetch()` does not. Tampermonkey's `GM_xmlhttpRequest` runs in the **browser extension process**, which is not subject to CORS restrictions, so no proxy server or backend is needed.

---

## How fields are read and written

The script reads fields by scanning `input[name]`, `select[name]`, and `textarea[name]` elements on the live page. Human-readable labels are resolved from associated `<label for="...">` elements or `aria-label` / `placeholder` attributes.

Picklist options are read directly from `<select>` elements so Claude only ever suggests values that are valid for the specific form — no hardcoding required.

Values are written back by setting `.value` and dispatching `input` and `change` events so the host page's own reactivity (React, Vue, Angular, plain JS) picks up the changes correctly.

---

## Troubleshooting

**Widget doesn't appear**
- Check DevTools console for `[VA]` messages to confirm the script is executing
- Confirm Tampermonkey is enabled for the current domain
- Try a hard refresh (`Ctrl+Shift+R`)

**"No speech detected" after recording**
- Grant the browser microphone permission for the site (address bar → Site settings)
- Chrome and Edge support the Web Speech API natively; Firefox requires enabling `media.webspeech.recognition.enable` in `about:config`

**API errors in the status bar**
- Verify your API key is correct and has credits at [console.anthropic.com](https://console.anthropic.com/)
- Check DevTools console for `[VA] Claude error:` for the full error detail

**Suggestions appear but fields don't fill**
- Some frameworks (React, Angular) require a value change event to register — the script dispatches both `input` and `change` but some custom components intercept differently
- Try clicking Apply immediately after suggestions appear before the page re-renders

---

## Project structure

```
voice-form-assistant.user.js   — the userscript (single file, no dependencies)
README.md
```

---

## License

MIT
