# src/js/utils

Shared utilities consumed by core, features, and renderers.

## FILES

| File | Key exports | Notes |
|------|-------------|-------|
| `constants.js` | `IS_IOS`, `IS_ANDROID`, `IS_CHROME`, `IS_FIREFOX`, `IS_SAFARI`, `IS_IE`, `IS_EDGE`, `HAS_MSE`, `SUPPORT_PASSIVE_EVENT`, `SUPPORTS_NATIVE_HLS` | UA detection via regex; `SUPPORT_POINTER_EVENTS` hardcoded `true` (supported everywhere now) |
| `dom.js` | `hasClass`, `addClass`, `removeClass`, `toggleClass`, `offset`, `loadScript`, `fadeIn`, `fadeOut`, `siblings` | Vanilla DOM; `classList` fast path + regex fallback for IE |
| `general.js` | `createEvent`, `splitEvents`, `isString`, `isObjectEmpty`, `escapeHTML`, `isNodeAfter` | Misc helpers |
| `generate.js` | `generateControlButton` | Builds button HTML with SVG `<use>` per icon; validates all args |
| `media.js` | `getTypeFromFile`, `formatType`, `absolutizeUrl` | MIME type resolution from URL |
| `time.js` | `secondsToTimeCode`, `timeCodeToSeconds`, `calculateTimeFormat` | Time formatting |
| `polyfill.js` | (side-effect) | Polyfills for `Object.assign`, `Promise`, `requestAnimationFrame`, etc. |

## WHERE TO LOOK

- UA / feature detection → `constants.js` (do not duplicate UA sniffs elsewhere)
- DOM manipulation → `dom.js` (no jQuery dependency in utils)
- Button/icon HTML generation → `generate.js` `generateControlButton()`
- Time display formatting → `time.js`

## NOTES

- All utils also attach to `mejs.Utils.*` for legacy plugin access
- `loadScript()` returns a Promise — use for async renderer SDK loading
- `polyfill.js` must be the **first** file in Browserify bundle entries
