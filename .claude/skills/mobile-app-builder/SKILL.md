---
name: mobile-app-builder
description: >
  Build mobile apps usable on iPhone and Android. Trigger when the user says "build a mobile
  app", "make this work on my phone", "I want an app I can use on mobile", "PWA", "React Native",
  or "Expo". Chooses the right path based on requirements: PWA (fastest, no install needed,
  builds on web skills) or Expo/React Native (native device APIs, app store distribution).
  Always clarifies the path before building.
---

# Mobile App Builder

You build apps that work on mobile phones. The right architecture depends on what the app needs to do — this skill picks the correct path and executes it fully.

---

## Phase 0 — Choose the Path

Ask (or infer) to determine the right approach:

```
Path A — Progressive Web App (PWA)
Path B — Expo (React Native)
```

### Decision tree

**Use Path A (PWA) if:**
- The core functionality is web-based (displaying content, forms, listings, tools)
- No need for native device APIs (camera, contacts, push notifications from a server, Bluetooth)
- User can access from browser — no app store required
- Building on top of an existing static site
- Need it working today, not after app review

**Use Path B (Expo) if:**
- Need native device features: camera, GPS with background tracking, biometric auth, NFC, Bluetooth
- App store presence is required (App Store / Google Play)
- Need offline-first with heavy local data sync
- Core experience requires native navigation patterns
- Will be used as a standalone app, not a browser bookmark

**When unsure:** default to PWA first. You can always migrate to Expo later. PWA ships in hours; Expo ships in days and requires a developer account + app review.

---

## Path A — Progressive Web App

### What a PWA adds to a static site

Three pieces turn a static site into an installable mobile app:
1. **`manifest.json`** — tells the browser the app's name, icons, and display mode
2. **Service Worker** — enables offline support and install prompt
3. **Icons** — multiple sizes for home screen and splash screens

### Phase A1 — Add to existing site (or build from scratch)

**Step 1: `manifest.json`** (place at root, next to `index.html`)
```json
{
  "name": "[Full App Name]",
  "short_name": "[Short Name — max 12 chars]",
  "description": "[One sentence]",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#[primary brand color]",
  "orientation": "portrait-primary",
  "icons": [
    { "src": "icons/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "icons/icon-512.png", "sizes": "512x512", "type": "image/png" },
    { "src": "icons/icon-512.png", "sizes": "512x512", "type": "image/png", "purpose": "maskable" }
  ]
}
```

**Step 2: Link manifest in `<head>`**
```html
<link rel="manifest" href="/manifest.json">
<meta name="theme-color" content="#[primary brand color]">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<meta name="apple-mobile-web-app-title" content="[Short Name]">
<!-- Apple doesn't use manifest icons — must specify separately -->
<link rel="apple-touch-icon" href="icons/icon-192.png">
```

**Step 3: Service Worker** (create `sw.js` at root)
```javascript
const CACHE_NAME = 'v1';
const ASSETS = [
  '/',
  '/index.html',
  '/manifest.json',
  // add your CSS/JS/image paths
];

// Install: pre-cache all assets
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME).then(cache => cache.addAll(ASSETS))
  );
  self.skipWaiting();
});

// Activate: clean old caches
self.addEventListener('activate', event => {
  event.waitUntil(
    caches.keys().then(keys =>
      Promise.all(keys.filter(k => k !== CACHE_NAME).map(k => caches.delete(k)))
    )
  );
  self.clients.claim();
});

// Fetch: serve from cache, fallback to network
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request).then(cached => cached || fetch(event.request))
  );
});
```

**Step 4: Register service worker** (in main JS, or inline at bottom of `<body>`)
```javascript
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/sw.js')
      .catch(err => console.warn('SW registration failed:', err));
  });
}
```

### Phase A2 — Mobile-specific CSS

Even if the site was mobile-responsive, PWA standalone mode needs extra care:

```css
/* Safe area insets — notch, home indicator */
:root {
  --safe-top: env(safe-area-inset-top, 0px);
  --safe-bottom: env(safe-area-inset-bottom, 0px);
  --safe-left: env(safe-area-inset-left, 0px);
  --safe-right: env(safe-area-inset-right, 0px);
}

body {
  padding-top: var(--safe-top);
  padding-bottom: var(--safe-bottom);
}

/* Prevent double-tap zoom on touch targets */
button, a, [role="button"] {
  touch-action: manipulation;
}

/* Touch target minimum size — 44px on any side */
button, a {
  min-height: 44px;
  min-width: 44px;
}

/* Prevent text resize on orientation change */
html {
  -webkit-text-size-adjust: 100%;
}

/* Remove tap highlight on iOS */
* {
  -webkit-tap-highlight-color: transparent;
}

/* Smooth scrolling on iOS */
.scrollable {
  -webkit-overflow-scrolling: touch;
  overflow-y: auto;
}
```

### Phase A3 — PWA Self-Test Checklist

- [ ] `manifest.json` is valid (test with Chrome DevTools → Application → Manifest)
- [ ] Service worker is registered and active (Application → Service Workers)
- [ ] App passes Chrome's PWA installability criteria (no warnings in Manifest panel)
- [ ] "Add to Home Screen" prompt appears on Android Chrome after 2+ visits
- [ ] App works offline (DevTools → Network → set to Offline, reload)
- [ ] `theme-color` matches brand color and renders correctly on status bar
- [ ] Apple touch icon shows correctly on iOS (add to home from Safari)
- [ ] All touch targets are ≥44px
- [ ] No horizontal overflow on 375px (iPhone SE width)
- [ ] Safe area insets applied — content not hidden by notch or home indicator

---

## Path B — Expo (React Native)

### Phase B0 — Prerequisites check

Before scaffolding, confirm:
- Node.js and npm are installed
- Expo Go app installed on test device (for development — no Xcode/Android Studio needed)
- Expo account created at expo.dev (free tier is sufficient)

### Phase B1 — Scaffold

```bash
npx create-expo-app@latest [app-name] --template blank-typescript
cd [app-name]
npx expo start
```

Scan the QR code with Expo Go on your phone to test immediately.

### Phase B2 — Project structure

```
[app-name]/
├── app/                  ← Expo Router file-based navigation
│   ├── (tabs)/
│   │   ├── _layout.tsx
│   │   ├── index.tsx     ← Home tab
│   │   └── [other-tabs].tsx
│   ├── _layout.tsx       ← Root layout
│   └── modal.tsx
├── components/           ← Shared UI components
├── constants/
│   └── Colors.ts         ← Theme colors
├── hooks/
├── assets/
│   └── images/
├── app.json              ← Expo config (name, icon, splash, permissions)
└── package.json
```

### Phase B3 — Key patterns

**Navigation (Expo Router — file-based, like Next.js):**
```typescript
// app/(tabs)/index.tsx — home screen
import { Link } from 'expo-router';

export default function HomeScreen() {
  return (
    <Link href="/detail">Go to detail</Link>
  );
}
```

**Safe area handling:**
```typescript
import { SafeAreaView } from 'react-native-safe-area-context';

export default function Screen() {
  return (
    <SafeAreaView style={{ flex: 1 }}>
      {/* content */}
    </SafeAreaView>
  );
}
```

**Platform-specific styles:**
```typescript
import { Platform, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  container: {
    paddingTop: Platform.OS === 'ios' ? 20 : 0,
    ...Platform.select({
      ios: { shadowColor: '#000', shadowOpacity: 0.1, shadowRadius: 4 },
      android: { elevation: 4 },
    }),
  },
});
```

**Touch targets — minimum 44pt (not px) on both platforms:**
```typescript
import { Pressable } from 'react-native';

// Use Pressable over TouchableOpacity — newer, more customisable
<Pressable
  onPress={handlePress}
  style={({ pressed }) => [styles.btn, pressed && styles.btnPressed]}
  hitSlop={8} // extends tap area beyond visual bounds
>
  <Text>Tap me</Text>
</Pressable>
```

**app.json — key fields to set immediately:**
```json
{
  "expo": {
    "name": "[App Name]",
    "slug": "[app-slug]",
    "version": "1.0.0",
    "icon": "./assets/images/icon.png",
    "splash": {
      "image": "./assets/images/splash-icon.png",
      "backgroundColor": "#ffffff"
    },
    "ios": {
      "bundleIdentifier": "com.[yourname].[appname]",
      "supportsTablet": false
    },
    "android": {
      "package": "com.[yourname].[appname]",
      "adaptiveIcon": {
        "foregroundImage": "./assets/images/adaptive-icon.png",
        "backgroundColor": "#ffffff"
      }
    },
    "plugins": []
  }
}
```

### Phase B4 — Mobile-specific rules for Expo

- **No `<div>`, `<p>`, `<img>`** — use `<View>`, `<Text>`, `<Image>` from `react-native`
- **No CSS** — use `StyleSheet.create()`. All dimensions are density-independent points.
- **Fonts need explicit loading** — use `expo-font` with `useFonts()` hook
- **All lists must use `FlatList` or `SectionList`** — never `Array.map` inside `ScrollView` for long lists (performance)
- **Network requests work normally** — `fetch()` is available
- **No `localStorage`** — use `@react-native-async-storage/async-storage` or Expo SecureStore

### Phase B5 — Expo build + distribution

For personal use (no app store), use **Expo Go** during development, then build a standalone APK/IPA:
```bash
# Build for Android (APK — sideloadable, no Play Store needed)
npx eas build --platform android --profile preview

# Build for iOS (requires Apple Developer account - $99/year)
npx eas build --platform ios --profile preview
```

For app store distribution:
```bash
npx eas build --platform all --profile production
npx eas submit --platform all
```

---

## Shared Rules (Both Paths)

- Touch targets minimum 44px/pt on all interactive elements
- No hover-only interactions — everything must work with touch
- All forms must work with virtual keyboard (account for keyboard pushing content up)
- Text minimum 16px for body copy — smaller is unreadable on mobile
- Loading states are mandatory — network requests take longer on mobile data
- No autoplaying video with sound — will be blocked on iOS
- Test on real device in addition to simulator — scroll behavior and input feel differ significantly
