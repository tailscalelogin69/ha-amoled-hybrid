# Home Assistant Amoled Dark Dynamic Theme

An AMOLED / pure-black dark theme for Home Assistant that **automatically follows the device system theme**.

- **Light mode** → official Home Assistant default light theme  
- **Dark mode** → pure black (AMOLED-friendly)

Two variants are provided:

| Theme name     | Behaviour in dark mode                                      |
|----------------|-------------------------------------------------------------|
| **Amoled Dyn** | Background + sidebar pure black, cards slightly elevated (`#141414`) |
| **Amoled+ Dyn**| Everything pure black (cards included)                      |

## Credits

Original Amoled theme by [Xitee1](https://github.com/Xitee1/ha-amoled-theme).  
This fork adds proper `modes:` support so the theme can switch with the system light/dark setting when Theme mode is set to **Auto**.

## Installation

### HACS (recommended)

1. HACS → Frontend → ⋮ → Custom repositories  
2. Repository: `https://github.com/tailscalelogin69/ha-amoled-hybrid`  
   Category: **Theme**  
3. Search for “Amoled” and install  
4. Restart Home Assistant (or call `frontend.reload_themes`)

### Manual

```bash
# inside your Home Assistant config folder
mkdir -p themes
# copy themes/amoled_dyn.yaml into themes/
```

Add (or keep) this in `configuration.yaml`:

```yaml
frontend:
  themes: !include_dir_merge_named themes
```

Restart Home Assistant.

## Activation

1. Open your **user profile** (bottom of the sidebar).  
2. Theme → choose **Amoled Dyn** or **Amoled+ Dyn**.  
3. Theme mode → **Auto** (this is the key setting for dynamic switching).

The frontend will now follow the OS / browser / companion-app light/dark preference.

## Compatibility

Structured for Home Assistant **2024.x – 2026.x**.

- Uses the modern `modes: light / dark` format documented at  
  https://www.home-assistant.io/integrations/frontend/#defining-themes  
- Extra variables (`app-header-*`, `secondary-background-color`, text colours, borders, etc.) are included so the theme stays consistent with recent frontend changes (including companion-app status-bar colours).

## Known limitation – iOS Companion App (background / app switcher)

**Symptom**  
When the Home Assistant iOS app is in the background (or visible in the app switcher) and you change the system light/dark mode, the HA UI stays on the previous theme until you bring the app back to the foreground.

**Cause**  
This is a limitation of the **Home Assistant Companion app for iOS**, not of this theme.

- The HA frontend decides light/dark via the CSS media query `prefers-color-scheme` and a JavaScript `matchMedia` listener.  
- On iOS the content lives inside a `WKWebView`. While the app is suspended / backgrounded, the WebView does not reliably receive or process trait-collection / colour-scheme changes, and the snapshot shown in the app switcher is not refreshed.  
- Native apps update their snapshots through UIKit; a web-based UI cannot do the same without explicit companion-app support.

**Work-around**  
Simply open the Home Assistant app again – the theme switches correctly as soon as the WebView becomes active.

This behaviour is the same for virtually every web-based (WKWebView) app on iOS. Fixing it would require changes in the official iOS companion app (e.g. forcing a style refresh on `traitCollectionDidChange` even when backgrounded, or updating the app-switcher snapshot). It cannot be solved from a theme YAML file.

## Reloading after changes

```yaml
# Developer Tools → Actions
action: frontend.reload_themes
```

Or restart Home Assistant.

## License

Same as the upstream project (community theme).
