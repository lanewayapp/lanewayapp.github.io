# Laneway brand assets

Generated from the mark shipped in `index.html` (favicon SVG + header lockup). Source of truth is `mark/Laneway Mark Dark.svg`.

## Palette
| Token | Hex |
|---|---|
| Ink (tile, text) | `#172133` |
| Amber (route, dot) | `#E89E2E` |
| Cream (page) | `#F7F5ED` |
| Cream tile | `#F2EFE6` |
| Rail (hairline) | `#D9D4C7` |
| Subtle text | `#6B7587` |

## File naming

Files are named in title case with spaces, e.g. `Laneway Mark Dark 512.png`. In HTML and CSS references, encode the spaces as `%20`.

## Contents
- `mark/` - tile mark, dark + light, SVG and PNG at 1024/512/256/128/64/32.
- `wordmark/` - horizontal lockups (on ink, on cream, transparent ink, transparent cream) and stacked lockups.
- `avatar/` - 1024px full-bleed profile pictures, safe for a circle crop.
- `favicon/` - `Favicon.svg` plus PNG at 16/32/48/180/192/512, all with transparent rounded corners. Also `Favicon Maskable 512.png` (full bleed, the mark scaled to 78 percent so it survives any Android launcher mask), and `Windows Tile 128/270/558.png` (the route alone on transparent, for `browserconfig.xml`).
- The site root carries the files clients ask for by name, which is why they break the title case rule: `favicon.ico` (16/32/48, built from the favicon PNGs), `apple-touch-icon.png` and `apple-touch-icon-precomposed.png` (180x180, opaque, full bleed) and `browserconfig.xml`. There is no web manifest for now; see below.
- `social/` - the 1200x630 card that shared links show. `Laneway Link Preview.gif` is the `og:image` on every page: a 3.6 second loop of a dot riding the route, over a grid that slowly breathes, with a pulse on the status dot. `Laneway Link Preview.png` is the still, used as `twitter:image`. Both are recorded from `Laneway Link Preview.html`: the still is a plain screenshot at 1200x630, and the loop is 72 screenshots at `seek(i/72)`, joined at 20 fps. iMessage shows only a portrait slice from the middle of the card, about 420px wide, so anything that must be read stays in the centre 400px.
- `connections/` - three generated, web-sized sample Osaka photos for the illustrative Connections concept. They are not customer photos.

## Usage

Every page carries this block, directly under the viewport meta. `404.html` uses root paths (`/favicon.ico`) because Pages serves it at any depth. The `data:` URI is `apple-touch-icon.png` in base64, about 3.7 KB, shortened here.

```html
<link rel="apple-touch-icon" sizes="180x180" href="data:image/png;base64,iVBORw0KGgo...">
<link rel="icon" href="favicon.ico?v=2" sizes="16x16 32x32 48x48">
<meta name="apple-mobile-web-app-title" content="Laneway">
<meta name="application-name" content="Laneway">
```

- The touch icon is the app icon's geometry, square and opaque. iOS rounds the corners itself and fills any transparent pixel with black, so `Favicon 180.png`, which has transparent corners, must never be used as the touch icon.
- Why it is inlined, and why there is no SVG icon link (lanewayapp/Laneway#165): an iPhone kept showing a letter "L" in Favourites and on the Home Screen. It did so after a 180x180 PNG touch icon was served at the root and linked from every page, and again after its URL was versioned. Two other projects hit the same wall in the same order. They fixed it on a device by inlining the touch icon as a PNG `data:` URI and making the tab icon a raster instead of an SVG, both in one change. The root cause was never found, so this site does both.
- To change the touch icon, regenerate `apple-touch-icon.png` (and the precomposed copy), then rebuild the URI with `python3 -c "import base64; print('data:image/png;base64,' + base64.b64encode(open('apple-touch-icon.png','rb').read()).decode())"` and paste it into all five pages.
- `?v=N` on the `.ico` and in `browserconfig.xml` lets caches keyed by URL pick up a change. Bump `N` everywhere when any of them changes.
- No web manifest, on purpose, until the iPhone test says otherwise. Reports from iOS 27.2 say iOS takes the Home Screen icon from the manifest when one is linked. The first manifest here had `any` icons with transparent rounded corners, and iOS discards transparent icons for a letter tile. Every touch icon fix shipped alongside that manifest failed. It was removed on its own so the result would say whether it was the cause. If it comes back, every icon in it must be opaque and full bleed like the touch icon, with a 180 among them, plus `Favicon Maskable 512.png` for Android. It must keep `display: browser`, so saving the site never makes something that opens like the app, which is not out yet.
- An icon already saved to Favourites or the Home Screen keeps its old picture. Delete it and add it again after any change.

## Rules
- Clear space around the mark equals the amber dot's diameter.
- Never recolor the route. Amber on ink, or amber on cream, only.
- Minimum tile size 16px; below 24px use the mark without the wordmark.
- Wordmark is set at weight 800, tracking `-0.055em`.
