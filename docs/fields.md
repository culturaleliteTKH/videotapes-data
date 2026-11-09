# Fields

The shape of the two records, field by field. The machine-readable version is [`schema/`](../schema); the API returns
exactly these ([`api/openapi.json`](../api/openapi.json)). Fields not listed as required may be missing or `null`.

## Sleeve

[`schema/sleeve.schema.json`](../schema/sleeve.schema.json) · example: [`examples/sleeve.json`](../examples/sleeve.json)

| Field | Type | Required | Meaning |
|---|---|---|---|
| `id` | string (uuid) | yes | Stable ID. Never reused; kept through edits. |
| `tmdbId` | integer | yes | The film this sleeve belongs to (TMDb ID). |
| `kind` | one of `release`, `custom` | no | release: a real video release. custom: fan-made artwork, only returned when asked for (custom=include or custom=fallback). |
| `layout` | one of `spread`, `front_only`, `unknown` | yes | spread: the whole wrap, read back | spine | front. front_only: just the front. unknown: not yet worked out. |
| `region` | one of `US`, `UK`, `EU`, `JP`, `Other`, or null | yes | The market group the sleeve was printed for. Null for fan-made covers. |
| `regionCountry` | string, or null | no | The country within the region, as a name ("Germany", "Finland"), when known. |
| `videoSystem` | one of `NTSC`, `PAL`, `SECAM`, or null | no | The tape's TV system. |
| `label` | string, or null | no | The video label or distributor as printed ("Palace Video", "Thorn EMI"). |
| `tapeYear` | integer, or null | no | Year of this video release (often the copyright line), not the film's year. |
| `catalogueNumber` | string, or null | no | The label's catalogue number as printed. |
| `releaseType` | one of `rental`, `sell_through`, `ex_rental`, or null | no | rental, sell_through (retail) or ex_rental. |
| `caseType` | one of `slipcase`, `clamshell`, `big_box`, or null | no | The packaging: a card slipcase, a plastic clamshell, or a big box. |
| `certificate` | string, or null | no | The age rating as printed ("18", "FSK 16", "R"). |
| `language` | string, or null | no | The language of the sleeve's text, as a name ("German"). |
| `titleOnSleeve` | string, or null | no | The title as printed on the sleeve, in its own language and script. The film's title is TMDb's English (US) one. |
| `artist` | string, or null | no | Fan-made covers only: who made it, if known. |
| `replacementWanted` | boolean | no | A better scan is wanted: flags.damage or flags.lowQuality. |
| `quality` | integer, or null | no | Retired: always null. Use flags.lowQuality. |
| `flags` | object | yes | What's wrong with this scan, if anything. |
| `flags.sticker` | boolean | | A shop or price sticker on the artwork. |
| `flags.damage` | boolean | | Damaged or incomplete scan. |
| `flags.watermark` | boolean | | A watermark over the artwork. |
| `flags.lowQuality` | boolean | | Low resolution or a poor photo. |
| `width` | integer | yes | The spread image's width in pixels. |
| `height` | integer | yes | The spread image's height in pixels. |
| `spreadUrl` | string (uri) | yes | The whole cleaned-up scan on the public CDN (no token needed). |
| `thumbnails` | object | yes | Smaller copies of the spread on the CDN, 400 and 1,200 px wide. |
| `thumbnails.w400` | string (uri) | |  |
| `thumbnails.w1200` | string (uri) | |  |
| `display` | object | no | 4K versions of the spread and the front, with their pixel sizes. URLs are API paths (send the token) that redirect to the CDN. AVIF for apps that decode it, WebP otherwise. |
| `display.spread` | object (see below) | |  |
| `display.front` | object (see below), or null | |  |
| `folds` | object, or null | yes | Where the spine starts and ends, as fractions of the spread's width. Null unless the layout is spread. |
| `folds.left` | number | | Between the back and the spine. |
| `folds.right` | number | | Between the spine and the front. |
| `folds.source` | one of `manual`, `estimated` | | Marked by a person, or estimated from the case's proportions. |
| `updatedAt` | string (date-time) | yes | Last change to this sleeve's tags or image. |
| `sha256` | string, or null | no | SHA-256 of the original scan file, for spotting the same file again. |
| `phash` | string, or null | no | 64-bit difference hash of the image, for spotting the same artwork scanned or saved differently. |
| `dateAdded` | string (date-time) | no | When the sleeve was added. |
| `lookalikeGroup` | string, or null | no | Sleeves sharing this value look alike (the same artwork in several releases): a browser can show one and link to the rest. |
| `matchConfidence` | one of `high`, `medium`, `low`, `none`, or null | no | How sure the match to the film is, for sleeves that weren't checked by hand. |

`display.spread` and `display.front` each have `width`, `height`, and `avif` and `webp` URLs (see
[images.md](images.md)). `display.front` is `null` when a sleeve has no front panel to cut.

## Film

[`schema/film.schema.json`](../schema/film.schema.json) · example: [`examples/film.json`](../examples/film.json)

| Field | Type | Required | Meaning |
|---|---|---|---|
| `tmdbId` | integer | yes | The film's ID on The Movie Database: the key everything hangs on. |
| `title` | string | yes | The film's English (US) title on TMDb. |
| `year` | integer, or null | no | The film's release year. |
| `imdbId` | string, or null | no | The film's IMDb ID (`tt` and digits). |
| `sleeves` | array of sleeves | yes | Approved sleeves: releases first, then fan-made ones if asked for; newest first within each. |
| `licence` | object | no | `metadata` (`CC0-1.0`) and `artwork` (its rights holders). On every API response. |
| `attribution` | string | no | How to credit videotapes.io. |

## Regions

A sleeve's `region` is the market it was printed for, in five groups:

| `region` | Covers |
|---|---|
| `US` | The United States |
| `UK` | The United Kingdom |
| `EU` | The rest of Europe: `regionCountry` says which country (Germany, Finland, the Netherlands…) |
| `JP` | Japan |
| `Other` | Everywhere else: `regionCountry` says where (Australia, Canada, Brazil, South Korea…) |

Fan-made covers have no region.

## Conventions

- **Names, not codes:** `regionCountry` and `language` are English names ("Germany", "German").
- **As printed:** `label`, `catalogueNumber`, `certificate` and `titleOnSleeve` are copied from the sleeve, so their
  spelling and capitals follow it.
- **Dates** are ISO 8601 in UTC.
- **IDs are stable.** A sleeve's `id` never changes or gets reused. A sleeve taken down on request answers
  `410 Gone` from the API.
