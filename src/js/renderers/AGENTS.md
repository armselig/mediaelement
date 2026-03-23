# src/js/renderers

Media format backends. Each file registers one renderer via `renderer.add()`.

## FILES

| File | Renderer name | Media type |
|------|--------------|------------|
| `html5.js` | `html5` | Native `<video>`/`<audio>`; always first in selection order |
| `hls.js` | `native_hls` | HLS via hls.js |
| `dash.js` | `native_dash` | MPEG-DASH via dash.js |
| `youtube.js` | `youtube_iframe` | YouTube IFrame API |
| `vimeo.js` | `vimeo_iframe` | Vimeo Player API |
| `dailymotion.js` | `dm_iframe` | Dailymotion IFrame API |
| `soundcloud.js` | `soundcloud_iframe` | SoundCloud Widget API |
| `facebook.js` | `facebook_iframe` | Facebook Video SDK |
| `twitch.js` | `twitch_iframe` | Twitch Embed API |

## RENDERER OBJECT CONTRACT

```js
const myRenderer = {
    name: 'unique_name',          // REQUIRED; must match options.prefix
    options: { prefix: 'unique_name' },
    canPlayType: (type) => /* Boolean */,
    create: (mediaElement, options, mediaFiles) => {
        // Must return container object with:
        //   hide(), show(), setSize(), destroy()
        //   get/set for each mejs.html5media.properties entry
        //   methods for each mejs.html5media.methods entry
        // Must fire: rendererready, loadeddata, loadedmetadata, canplay
    }
};
renderer.add(myRenderer);
```

## SELECTION ORDER

Default auto-rank: native renderers first (`/^(html5|native)/i`), iframe renderers second (`/iframe$/i`). Override via `renderers: ['html5', 'youtube_iframe']` in player options.

## ANTI-PATTERNS

- Avoid `for…of` loops — breaks bundle size optimization (use indexed `for` loop)
- Renderer `name` must match `options.prefix` exactly
- After adding a renderer: update `Gruntfile.js` browserify bundle AND add entry to `/test/player.html` + `/demo/index.html`
- `apiStack` pattern: queue get/set/call operations before the player is ready, flush in `window['__ready__' + container.id]`

## NOTES

- Individual renderer bundles (`build/renderers/[name].js`) are built separately for on-demand loading
- `typeChecks.push(…)` used to detect renderer type from URL extension when MIME type unavailable
