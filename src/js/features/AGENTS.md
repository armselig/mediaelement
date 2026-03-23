# src/js/features

Player UI control features. Each file mounts one controlbar element.

## FILES

| File | Builds | State classes on container div |
|------|--------|-------------------------------|
| `playpause.js` | play/pause/replay button | `.mejs__play` / `.mejs__pause` / `.mejs__replay` |
| `volume.js` | mute button + slider | `.mejs__mute` / `.mejs__unmute` |
| `fullscreen.js` | fullscreen toggle button | (none initially) / `.mejs__fullscreen` / `.mejs__unfullscreen` |
| `progress.js` | seekbar + buffering rail | — |
| `time.js` | current time / duration display | — |
| `tracks.js` | CC + chapters button + menus | `.mejs__captions-enabled` (on captionsButton when track active) |

## PATTERN — Adding a Feature

```js
// 1. Extend config
Object.assign(config, { myOptionKey: defaultValue });

// 2. Extend prototype — method MUST be prefixed with `build`
Object.assign(MediaElementPlayer.prototype, {
    buildmyfeature(player, controls, layers, media) {
        // create DOM, bind events, call player.addControlElement(el, 'myfeature')
    }
});
```

Feature is activated by adding `'myfeature'` to the `features` config array.

## ICON VISIBILITY MECHANISM

`generateControlButton(playerId, ariaLabel, title, iconSprite, iconsArray, classPrefix)` renders **all icons hidden**. A CSS rule on the container div's **state class** reveals the correct one:

```css
.mejs__play svg.mejs__icon-play   { display: block; }
.mejs__pause svg.mejs__icon-pause { display: block; }
```

To add an icon variant: append icon ID to `iconsArray`, add corresponding CSS rule using existing state class.

## ANTI-PATTERNS

- Never use `.mejs__active` — this class does not exist; use actual state classes listed above
- Fullscreen button has **no initial state class** — CSS must handle the classless initial state
- Mobile: `focusout` does not work reliably on mobile (known issue in `tracks.js:171,216`)
- Feature build method must be on `MediaElementPlayer.prototype` — not a standalone function
