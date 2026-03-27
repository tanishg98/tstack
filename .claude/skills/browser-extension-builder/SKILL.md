---
name: browser-extension-builder
description: >
  Build Chrome and Firefox browser extensions (Manifest V3). Trigger when the user says
  "build a browser extension", "chrome extension", "firefox addon", "extension that does X",
  or wants to add browser-level functionality (injecting into pages, intercepting requests,
  adding a toolbar button, modifying page content, reading clipboard, tab management, etc.).
  Outputs a ready-to-load unpacked extension folder. Covers the full lifecycle:
  design → build → self-test → security check → package.
---

# Browser Extension Builder

You build production-quality Manifest V3 browser extensions. Output is an unpacked extension folder that loads directly in Chrome/Firefox — no build tooling required unless bundling is explicitly needed.

**Before writing any code**, follow the phases below in order.

---

## Phase 0 — Clarify & Choose Architecture

Extensions have four possible components. Use only what's needed — each additional component increases attack surface and review risk.

Ask (or infer from the request) which are required:

| Component | Use when |
|-----------|----------|
| **Popup** (`popup.html`) | Need a UI when the toolbar icon is clicked |
| **Content Script** (`content.js`) | Need to read or modify the current page's DOM |
| **Service Worker** (`background.js`) | Need persistent logic, alarms, or to respond to browser events |
| **Options Page** (`options.html`) | Need user-configurable settings |

**Decision rule:** if the user only needs to inject something into pages, start with content script + popup only. Add a service worker only if you need background processing or event listeners that survive page navigation.

---

## Phase 1 — Scaffold

### Directory structure (adjust based on Phase 0 decision)

```
extension/
├── manifest.json          ← the contract with the browser
├── popup/
│   ├── popup.html
│   ├── popup.css
│   └── popup.js
├── content/
│   └── content.js         ← runs in page context
├── background/
│   └── service-worker.js  ← MV3 service worker (not a persistent page)
├── options/
│   ├── options.html
│   └── options.js
├── icons/
│   ├── icon16.png
│   ├── icon48.png
│   └── icon128.png
└── lib/                   ← third-party scripts if needed (no CDN in extensions)
```

**Important**: Extensions cannot load resources from external URLs (CDN) in scripts. All JS libraries must be vendored into `lib/`. CSS fonts from Google Fonts ARE allowed in HTML files.

### Manifest V3 template

```json
{
  "manifest_version": 3,
  "name": "[Extension Name]",
  "version": "1.0.0",
  "description": "[One sentence description]",
  "icons": {
    "16": "icons/icon16.png",
    "48": "icons/icon48.png",
    "128": "icons/icon128.png"
  },
  "action": {
    "default_popup": "popup/popup.html",
    "default_icon": {
      "16": "icons/icon16.png",
      "48": "icons/icon48.png"
    },
    "default_title": "[Tooltip on hover]"
  },
  "background": {
    "service_worker": "background/service-worker.js",
    "type": "module"
  },
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content/content.js"],
      "run_at": "document_idle"
    }
  ],
  "options_ui": {
    "page": "options/options.html",
    "open_in_tab": false
  },
  "permissions": [],
  "host_permissions": []
}
```

**Permissions — minimal by default.** Add only what's needed:

| Permission | When to add |
|-----------|-------------|
| `storage` | Saving user settings or state |
| `activeTab` | Reading/modifying the currently active tab (only while popup is open) |
| `tabs` | Accessing tab URLs/titles programmatically |
| `scripting` | Injecting scripts programmatically (usually better than content_scripts for on-demand injection) |
| `notifications` | Showing browser notifications |
| `alarms` | Scheduled background tasks |
| `bookmarks` | Reading/writing bookmarks |
| `history` | Accessing browsing history |
| `clipboardWrite` | Writing to clipboard from content script |

Host permissions (`"host_permissions": ["https://*.example.com/*"]`) are separate from permissions. Always scope to the minimum required domain, never `<all_urls>` unless genuinely needed.

---

## Phase 2 — Build

### Key patterns

**Message passing between components** (the only safe way to communicate):
```javascript
// From popup or content script → service worker
chrome.runtime.sendMessage({ action: 'doSomething', data: payload })
  .then(response => console.log(response));

// In service worker — listen
chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  if (message.action === 'doSomething') {
    // handle
    sendResponse({ success: true });
  }
  return true; // keep channel open for async response
});

// From service worker → specific tab's content script
chrome.tabs.sendMessage(tabId, { action: 'updateDOM', data: payload });
```

**Storage** (always use `chrome.storage`, never `localStorage` in extensions):
```javascript
// Save
await chrome.storage.sync.set({ key: value }); // syncs across devices
await chrome.storage.local.set({ key: value }); // local only, larger quota

// Read
const { key } = await chrome.storage.sync.get('key');

// Listen for changes
chrome.storage.onChanged.addListener((changes, area) => {
  if (area === 'sync' && changes.key) {
    const newValue = changes.key.newValue;
  }
});
```

**Content script best practices:**
```javascript
// Always guard against running multiple times (page reload, SPA navigation)
if (window.__myExtensionLoaded) return;
window.__myExtensionLoaded = true;

// Use a unique prefix for all DOM IDs and class names you inject
// Bad: id="container" — may conflict with page
// Good: id="myext-container"

// Inject UI via shadow DOM to prevent style leakage
const host = document.createElement('div');
host.id = 'myext-host';
const shadow = host.attachShadow({ mode: 'closed' });
document.body.appendChild(host);
```

**Service worker limitations** (MV3 is not a persistent background page):
```javascript
// Service workers CAN be terminated between events. Don't store state in variables.
// Always read state from chrome.storage, not in-memory variables.

// Correct: read from storage each time
chrome.runtime.onMessage.addListener(async (msg) => {
  const { settings } = await chrome.storage.local.get('settings');
  // use settings
});

// Wrong: assume this persists
let settings = {}; // will be lost when service worker terminates
```

**Popup HTML** — link to CSS/JS as files, use CSP-safe patterns:
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="color-scheme" content="light dark">
  <link rel="stylesheet" href="popup.css">
  <style>
    :root { --width: 320px; --bg: #ffffff; }
    body { width: var(--width); min-height: 200px; margin: 0; }
  </style>
</head>
<body>
  <!-- No inline event handlers (onclick="...") — CSP blocks them in MV3 -->
  <button id="action-btn">Do thing</button>
  <script src="popup.js"></script> <!-- defer not needed — at body end -->
</body>
</html>
```

### SVG icon generation (when no icons provided)
Generate 3 SVG icons (16, 48, 128px) as placeholder PNGs using a simple colored square + letter. Note that real app store submission requires proper icons — these are for local development only.

---

## Phase 3 — Security Checklist

Run this before declaring the extension done:

**Content Security Policy:**
- [ ] No `eval()` or `new Function()` anywhere in the extension
- [ ] No inline event handlers (`onclick=""`, `onload=""`) in HTML files
- [ ] All external scripts are vendored into `lib/` — no CDN script tags
- [ ] If you must allow `unsafe-inline` CSS, document why; never allow it for scripts

**Permissions audit:**
- [ ] Every permission in `manifest.json` is actually used in the code
- [ ] `host_permissions` is scoped to the minimum required domain(s)
- [ ] `activeTab` is preferred over `tabs` + `<all_urls>` when possible
- [ ] No `webRequest` or `declarativeNetRequest` unless the core feature requires request interception

**Data handling:**
- [ ] No user data sent to external servers without explicit user consent
- [ ] `chrome.storage.sync` (not localStorage) for all settings
- [ ] Content scripts don't expose the extension's internal API to page JS
- [ ] If injecting content into pages: injected elements use shadow DOM to prevent style conflicts and XSS surface

**Firefox compatibility** (if targeting both):
- [ ] Replace `chrome.*` with `browser.*` OR use the [webextension-polyfill](https://github.com/mozilla/webextension-polyfill) (vendor it into `lib/`)
- [ ] Check `manifest.json` for any Chrome-only keys — wrap in `"browser_specific_settings"` for Firefox

---

## Phase 4 — Self-Test Checklist

Before handing off, verify manually or by instruction:

- [ ] Extension loads without errors in Chrome: `chrome://extensions` → "Load unpacked" → select folder
- [ ] No errors in the service worker console (`Inspect views: service worker` link on the extension card)
- [ ] Popup opens, renders correctly, and all interactions work
- [ ] Content script activates on the correct pages (check `matches` in manifest)
- [ ] Storage read/write works (verify in DevTools → Application → Extension Storage)
- [ ] `chrome.runtime.lastError` is never set after API calls (check console)
- [ ] Extension works in incognito if `"incognito": "spanning"` is set
- [ ] If options page: settings persist across popup close/open

---

## Phase 5 — Deliver

Output to `outputs/[extension-name]/`:

```
outputs/[extension-name]/
├── manifest.json
├── popup/
├── content/
├── background/
├── icons/
└── README-install.md    ← one-page install + test instructions
```

`README-install.md` must include:
1. How to load unpacked in Chrome
2. How to load in Firefox (if targeting both)
3. What permissions are requested and why (for user trust)
4. Basic usage instructions

Add comment block at top of `manifest.json`:
```json
{
  "_comment": "Built with Browser Extension Builder | [Extension Name] | [Date]",
  "manifest_version": 3,
  ...
}
```

---

## Common Pitfalls

| Pitfall | Fix |
|---------|-----|
| Service worker state lost between events | Store everything in `chrome.storage`, not variables |
| Popup JS can't access `document` of current tab | Use content script + message passing |
| External fonts/scripts blocked by CSP | Vendor all JS; fonts in CSS/HTML are fine |
| `onclick=""` in popup HTML blocked | Use `addEventListener` in JS file instead |
| `localStorage` not shared between popup and service worker | Use `chrome.storage` everywhere |
| Content script runs on chrome:// pages | Use specific `matches` patterns, not `<all_urls>` blindly |
| MV2 deprecation warnings | Always use `"manifest_version": 3` |
