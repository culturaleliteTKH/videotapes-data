# The API

Read the data live at `https://videotapes.io/api/v1`. The full description is
[`api/openapi.json`](../api/openapi.json) (OpenAPI 3.1), also served at
[videotapes.io/api/v1/openapi.json](https://videotapes.io/api/v1/openapi.json); a readable version is at
[videotapes.io/docs](https://videotapes.io/docs).

## Endpoints

| | |
|---|---|
| `GET /films/{tmdbId}` | A film and its approved sleeves ([film record](fields.md#film)) |
| `GET /films/{tmdbId}/preferred?region=UK` | The sleeve shown first for a film, in one region or any |
| `POST /preferred/lookup` | Preferred sleeves for many films at once: `{"tmdbIds": [105, 165, 196]}` (up to 20,000) |
| `GET /sleeves` | Every approved sleeve, newest first, 50 a page (`limit` up to 100): `q` searches titles, `region` filters, `cursor` is the previous page's `next` |
| `GET /sleeves/{id}` | One sleeve ([sleeve record](fields.md#sleeve)) |
| `GET /sleeves/{id}/image` | Redirects to an image: `part` (`spread`, `front`, `spine`, `back`), `w` or `size=display`, `format` (see [images.md](images.md)) |

Fan-made covers are left out unless asked for: `custom=include` adds them, `custom=fallback` returns them only for films
with no real release.

## Access and limits

- **Reading needs a token for now**, sent as `Authorization: Bearer svt_…`. Ask for one at
  videotapesdotio@gmail.com, saying what you'd like to build. A token allows 600 requests a minute. (When reads open
  to everyone, requests without a token will get 60 a minute per address; [videotapes.io/docs](https://videotapes.io/docs)
  always says which applies.)
- Past the limit the API answers `429` with a `Retry-After` header. Any origin may call it (CORS is open).
- **Images** are on a public CDN (`covers.videotapes.io`) and need no token: the URLs in a record work anywhere.

## Responses

- JSON, with the record's fields as listed in [fields.md](fields.md), plus `licence` and `attribution` on every response.
- `404` for a film or sleeve that doesn't exist; `410 Gone` for a sleeve taken down on request. Treat a `410` as a
  deletion and drop any copy you hold.
- New fields may be added at any time; clients should ignore fields they don't know. Removing or renaming a field
  means a new schema version (see [CHANGELOG.md](../CHANGELOG.md)).
