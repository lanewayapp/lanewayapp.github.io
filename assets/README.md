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
- `favicon/` - `Favicon.svg` plus PNG at 16/32/48/180/192/512.
- `social/` - the 1200x630 card that shared links show. `Laneway Link Preview.gif` is the `og:image` on every page: a 3.6 second loop of a dot riding the route, over a grid that slowly breathes, with a pulse on the status dot. `Laneway Link Preview.png` is the still, used as `twitter:image`. Both are recorded from `Laneway Link Preview.html`: the still is a plain screenshot at 1200x630, and the loop is 72 screenshots at `seek(i/72)`, joined at 20 fps. iMessage shows only a portrait slice from the middle of the card, about 420px wide, so anything that must be read stays in the centre 400px.
- `connections/` - three generated, web-sized sample Osaka photos for the illustrative Connections concept. They are not customer photos.

## Usage
```html
<link rel="icon" href="/assets/favicon/Favicon.svg" type="image/svg+xml">
<link rel="icon" href="/assets/favicon/Favicon%2032.png" sizes="32x32">
<link rel="apple-touch-icon" href="/assets/favicon/Favicon%20180.png">
```

## Rules
- Clear space around the mark equals the amber dot's diameter.
- Never recolor the route. Amber on ink, or amber on cream, only.
- Minimum tile size 16px; below 24px use the mark without the wordmark.
- Wordmark is set at weight 800, tracking `-0.055em`.
