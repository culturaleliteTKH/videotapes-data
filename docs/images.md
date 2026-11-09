# Using the scans

## The spread

Most sleeves are scanned flat as one image, a **spread**, reading **back | spine | front** from left to right
(`layout: "spread"`). Some are fronts only (`layout: "front_only"`).

`folds.left` and `folds.right` are where the spine starts and ends, as fractions of the spread's width. So for a spread
`width` pixels wide:

```
back   = 0                    .. folds.left  × width
spine  = folds.left  × width  .. folds.right × width
front  = folds.right × width  .. width
```

`folds.source` is `manual` when a person marked them and `estimated` when they come from the standard proportions of the
case. `folds` is `null` for anything but a spread. To wrap a spread onto a 3D tape box, map the three slices onto the
box's back, spine and front.

## Sizes

| | Where | Use |
|---|---|---|
| `thumbnails.w400` | CDN, WebP | Grids and lists |
| `thumbnails.w1200` | CDN, WebP | A detail view |
| `spreadUrl` | CDN, JPEG | The whole cleaned-up scan at its own size (`width` × `height`) |
| `display.spread`, `display.front` | API path → CDN | 4K versions (the spread 3,200 px wide, the front 2,160 px tall; never upscaled), AVIF or WebP |
| `/sleeves/{id}/image?part=…&w=…` | API path → CDN | One panel (`front`, `spine`, `back`) at a width rounded up to 200, 400, 600, 800, 1,200, 1,600 or 2,400 px; never upscaled |

Image URLs change when a sleeve's image is redone (the path holds a revision), so store the sleeve's `id` and refresh
URLs from the API rather than keeping them forever.

## Rights

The artwork belongs to its rights holders. videotapes.io shows it for preservation and research; the CC0 licence on
this repo covers the metadata only. If you show the images, say where they come from, and respect takedowns (a `410`
from the API).
