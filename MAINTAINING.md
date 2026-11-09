# Maintaining this repo

For whoever looks after videotapes.io.

## Where things come from

| Here | Made by | Edit by hand? |
|---|---|---|
| `schema/*.json`, `api/openapi.json` | `apps/web/scripts/export-public-schema.ts` in the site's repo, from `apps/web/lib/openapi.ts` | No: change `lib/openapi.ts`, then re-export |
| `examples/*.json` | Real records from the live API | Refresh when fields change |
| `docs/fields.md` | The field table follows `schema/sleeve.schema.json`; the rest is prose | Yes, keep it in step |
| `README.md`, `docs/*.md`, `CHANGELOG.md` | Hand-written | Yes |

The site's tests check that the API's sleeve fields and `lib/openapi.ts` list the same fields, each with a description
(`apps/web/test/openapi.test.ts`), so what's exported here matches what the API returns.

## Updating the schema

1. In the site's repo, change the API and `lib/openapi.ts` together; the tests must pass.
2. Export into a checkout of this repo, from `apps/web`:
   `node --import tsx scripts/export-public-schema.ts <path to videotapes-data>`
3. Refresh `examples/` if a field was added, update `docs/fields.md`, and add a `CHANGELOG.md` entry. A removed or
   renamed field means a new version: bump `VERSION` in the export script first.
4. Validate the examples against the schemas (any JSON Schema 2020-12 validator, for example
   `npx ajv-cli validate --spec=draft2020 -s schema/sleeve.schema.json -d examples/sleeve.json -c ajv-formats`).

## Never put here

Images, credentials, the import pipeline, source lists, or anything that identifies the people who sent scans or
suggestions. This repo is public.
