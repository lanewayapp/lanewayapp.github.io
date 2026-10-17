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
- `favicon/` - `Favicon.svg` plus PNG at 16/32/48/180/192/512, all with transparent rounded corners. Also `Favicon Maskable 512.png` (full bleed, the mark scaled to 78 percent so it survives any Android launcher mask), `Safari Pinned Tab.svg` (the route alone in black, for `mask-icon`), and `Windows Tile 128/270/558.png` (the route alone on transparent, for `browserconfig.xml`).
- The site root carries the files clients ask for by name, which is why they break the title case rule: `favicon.ico` (16/32/48, built from the favicon PNGs), `favicon.svg` (the mark with no metadata), `apple-touch-icon.png` and `apple-touch-icon-precomposed.png` (180x180, opaque, full bleed), `site.webmanifest` and `browserconfig.xml`.
- `social/` - the 1200x630 card that shared links show. `Laneway Link Preview.gif` is the `og:image` on every page: a 3.6 second loop of a dot riding the route, over a grid that slowly breathes, with a pulse on the status dot. `Laneway Link Preview.png` is the still, used as `twitter:image`. Both are recorded from `Laneway Link Preview.html`: the still is a plain screenshot at 1200x630, and the loop is 72 screenshots at `seek(i/72)`, joined at 20 fps. iMessage shows only a portrait slice from the middle of the card, about 420px wide, so anything that must be read stays in the centre 400px.
- `connections/` - three generated, web-sized sample Osaka photos for the illustrative Connections concept. They are not customer photos.

## Usage

Every page carries this block. `404.html` uses root paths (`/favicon.ico`) because Pages serves it at any depth.

```html
<link rel="icon" href="favicon.ico?v=2" sizes="32x32">
<link rel="icon" href="favicon.svg?v=2" type="image/svg+xml">
<link rel="apple-touch-icon" href="apple-touch-icon.png?v=2" sizes="180x180">
<link rel="mask-icon" href="assets/favicon/Safari%20Pinned%20Tab.svg?v=2" color="#E89E2E">
<link rel="manifest" href="site.webmanifest?v=2">
<meta name="apple-mobile-web-app-title" content="Laneway">
<meta name="application-name" content="Laneway">
```

- The touch icon is the app icon's geometry, square and opaque. iOS rounds the corners itself and fills any transparent pixel with black, so `Favicon 180.png`, which has transparent corners, must never be used as the touch icon. iOS does not read SVG for Favourites or the Home Screen; without a PNG touch icon Safari draws a letter from the page title.
- `sizes="32x32"` on the `.ico` stops Chrome preferring it over the SVG.
- Every icon URL carries `?v=N`: the head block, the manifest's icons and `browserconfig.xml`. Caches keyed by URL then pick up a changed icon. When any icon changes, bump `N` everywhere at once.
- What `?v=N` does not fix: on an iPhone that saved the site to Favourites before the icons existed, the share sheet showed the mark while Add to Favourites kept the letter, before and after `?v=2`. The URL was new, so the stale record is most likely per site and on the device, out of the server's reach.
- The manifest lists PNGs only. iOS cannot use an SVG manifest icon, and Chrome has a PNG at every size it needs.
- The manifest says `display: browser`, so saving the site makes a shortcut to a web page, never something that opens like the app, which is not out yet.
- Safari caches these icons hard. After a change, delete the Favourite or Home Screen icon and add it again; if the old icon persists, clear the site's data under Settings, Apps, Safari, Advanced, Website Data.

## Rules
- Clear space around the mark equals the amber dot's diameter.
- Never recolor the route. Amber on ink, or amber on cream, only.
- Minimum tile size 16px; below 24px use the mark without the wordmark.
- Wordmark is set at weight 800, tracking `-0.055em`.
