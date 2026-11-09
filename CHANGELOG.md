# Changelog

The data model's versions. A record's shape only changes with a new version; new optional fields can be added within
one. The schema's `$id` carries the version (`https://videotapes.io/schema/sleeve-2.json`).

## v2: 2026-11-09

The first public version: the records exactly as the API returns them, generated from the API's own description so the
two can't drift apart.

- **Sleeve** and **film** schemas (`schema/sleeve.schema.json`, `schema/film.schema.json`), with real examples.
- Field names follow the API (`tmdbId`, `regionCountry`, `catalogueNumber`…).
- New since v1: `kind` (fan-made covers), `artist`, `flags` and `replacementWanted`, 4K `display` versions,
  `folds.source`, `lookalikeGroup`, `matchConfidence`.
- Left out of the public record: the internal workflow (statuses other than approved, how each tag was set, where scans
  were found, mirror bookkeeping). Only approved sleeves are published.

## v1: 2026-11-04

A first draft, shared between the site and its import tools. Never published.
