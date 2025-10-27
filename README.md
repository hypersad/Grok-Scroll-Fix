# Grok Auto-Scroll Fix for Legacy Chrome/Edge

- When **new messages are dynamically added**
- Works reliably even on **legacy browsers**

---

## Features

| Feature | Description |
| --- | --- |
| Instant scroll on load | Uses `DOMContentLoaded` + fallback interval |
| Real-time scroll on updates | MutationObserver watches DOM changes |
| Lightweight & fast | No external dependencies |
| Works on old Chrome/Edge | Tested on Chrome 85+ and Edge Legacy |
| Safe & non-intrusive | Only scrolls — no data collection |

---

## Installation

1. **Install Tampermonkey**  
  → [Chrome Web Store](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)  
  → [Edge Add-ons](https://microsoftedge.microsoft.com/addons/detail/tampermonkey/iikmkjmpaadaobahmlepeloendndfphd)
  
2. **Create a new script**  
  Open Tampermonkey → **Create a new script** → Replace all content with the code below.
  

---

## Userscript Code (Copy & Paste)

```javascript
// ==UserScript==
// @name         Grok Auto-Scroll to Bottom Fix
// @namespace    http://tampermonkey.net/
// @version      1.1
// @description  Force scroll to bottom on Grok chat pages (fixes legacy Chrome/Edge)
// @author       hypersad
// @match        https://grok.com/*
// @grant        none
// @license      MIT
// ==/UserScript==

(function() {
    'use strict';

    // Scroll to bottom function
    function scrollToBottom() {
        const body = document.body;
        const html = document.documentElement;
        const height = Math.max(
            body.scrollHeight, body.offsetHeight,
            html.clientHeight, html.scrollHeight, html.offsetHeight
        );
        window.scrollTo({ top: height, behavior: 'smooth' });
    }

    // On DOM ready
    document.addEventListener('DOMContentLoaded', () => {
        scrollToBottom();
        // Poll every 200ms for 5 seconds (handles async content)
        const interval = setInterval(scrollToBottom, 200);
        setTimeout(() => clearInterval(interval), 5000);
    });

    // Observe DOM changes (new messages, loading states)
    const observer = new MutationObserver(scrollToBottom);
    observer.observe(document.body, {
        childList: true,
        subtree: true,
        attributes: true,
        characterData: true
    });

    // Optional: Scroll again after 2 seconds as final fallback
    setTimeout(scrollToBottom, 2000);
})();
```

---

## How It Works

| Step | Action |
| --- | --- |
| 1   | Waits for `DOMContentLoaded` |
| 2   | Scrolls immediately |
| 3   | Checks every **200ms** for **5 seconds** |
| 4   | Uses `MutationObserver` to detect new chat bubbles |
| 5   | Final scroll after 2 seconds as backup |

> Uses `behavior: 'smooth'` for better UX (falls back to instant on old browsers).

---

## Troubleshooting

| Issue | Solution |
| --- | --- |
| Not scrolling | Hard refresh: **Ctrl + Shift + R** |
| Only scrolls halfway | Increase interval timeout to `8000` |
| Script not running | Confirm URL matches `@match` patterns |
| Too much CPU | Reduce interval to `500` ms |

---

## Supported URLs

```
https://grok.com/*
```

---

## License

**MIT License** – Free to use, modify, and distribute.

---

## Feedback

Found a bug? Want a version with **"Scroll to Top" button**?  
Open an issue here: [GitHub Gist Link](https://gist.github.com/yourname/xxx) *(replace with real link if hosted)*

**Enjoy seamless Grok conversations — always at the latest message!**  
