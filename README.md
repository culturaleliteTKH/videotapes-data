# videotapes-data

The open data model behind [videotapes.io](https://videotapes.io), a community archive of VHS sleeve artwork: the
front covers, spines and back blurbs of tapes from rental shops and living rooms around the world.

Every sleeve on the site is matched to the film it belongs to (by its [TMDb](https://www.themoviedb.org) ID) and to the
market it was printed for, and tagged with what's printed on it: label, catalogue number, certificate, release type and
so on. This repo describes that data so anyone can use it.

| | |
|---|---|
| [`schema/`](schema) | JSON Schema (2020-12) for a **sleeve** and a **film** record |
| [`examples/`](examples) | Real records, as the API returns them |
| [`api/openapi.json`](api/openapi.json) | The full API description (OpenAPI 3.1) |
| [`docs/fields.md`](docs/fields.md) | Every field explained, with its allowed values |
| [`docs/api.md`](docs/api.md) | How to read the data live: endpoints, limits, images |
| [`docs/images.md`](docs/images.md) | Using the scans: spreads, folds, sizes |
| [`CHANGELOG.md`](CHANGELOG.md) | How the data model has changed |
| [`TERMS.md`](TERMS.md) | **Using the data and the images**: what's free, what isn't, takedowns |

**Data downloads** (the whole catalogue as files) will be published here once the first release is ready. Until then,
the [API](docs/api.md) serves the same records live.

## Quick start

The API needs a token for now: ask at videotapesdotio@gmail.com (see [docs/api.md](docs/api.md#access-and-limits)).

```sh
curl -H "Authorization: Bearer svt_…" https://videotapes.io/api/v1/films/32450    # Brain Donors: the film and its sleeves
```

Each sleeve has image URLs on a public CDN. To go through the whole catalogue, page through `GET /api/v1/sleeves`. See
[docs/api.md](docs/api.md) for all endpoints.

## What's in a record

A **film** is a TMDb ID, its title and year, and its approved sleeves. A **sleeve** is one scanned release:

- **Which release:** `region` (US, UK, EU, JP, Other) and `regionCountry`, `label`, `catalogueNumber`, `tapeYear`,
  `releaseType` (rental, sell-through, ex-rental), `caseType`, `certificate`, `videoSystem`, `language`, and the
  `titleOnSleeve` as printed in its own language.
- **The image:** `spreadUrl` (back, spine and front in one image), `thumbnails`, 4K `display` versions, the size, and
  `folds`, where the spine starts and ends.
- **Its condition:** `flags` (sticker, damage, watermark, low quality); `replacementWanted` when a better scan would help.
- **Identity:** a stable `id`, the `sha256` of the original file and a perceptual hash (`phash`) for finding the same
  artwork scanned twice.

Fan-made covers (`kind: custom`) are kept apart from real releases and are only returned when asked for.

Everything about the film itself (plot, genres, cast, posters) is TMDb's: look it up there by `tmdbId`.

## Licences

In short: the data is free; the images are linked, not licensed. The full terms are in [`TERMS.md`](TERMS.md).

- **Metadata: [CC0 1.0](LICENSE).** No rights reserved: copy it, mirror it, build on it, commercially or not. Credit
  ("data from videotapes.io") is appreciated, not required.
- **Artwork: © its respective rights holders** (studios, distributors, artists). It is shown on videotapes.io for
  preservation and research. The CC0 licence covers the data, not the images.
- **Film titles and years** come from TMDb. This product uses the TMDb API but is not endorsed or certified by TMDb.

## Corrections and questions

- **A sleeve's details are wrong?** Use "Suggest an edit" on its film page at videotapes.io. Edits are checked by hand.
- **A sleeve shouldn't be there?** See the takedown section of the [legal page](https://videotapes.io/legal).
- **Questions about the data model or the API,** or something missing from it: open an issue here.

See [CONTRIBUTING.md](CONTRIBUTING.md).
