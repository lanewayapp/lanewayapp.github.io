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
