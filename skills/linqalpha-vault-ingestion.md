---
name: Upload internal documents to LinqAlpha and make them searchable
description: Ingest your own research documents through the Vault or RMS source-batch flow, poll until they are indexed, and confirm they are retrievable before querying over them.
api: openapi/linqalpha-openapi-original.json
operations:
  - "POST /v2/vault/presigned_url"
  - "POST /v2/vault/confirm"
  - "GET /v2/vault/status"
  - "POST /v1/source_batches"
  - "POST /v1/sources/upload_url"
  - "POST /v1/sources"
  - "GET /v1/sources/{source_id}"
  - "GET /v1/status/documents"
  - "GET /v1/status/sync"
generated: '2026-07-19'
method: generated
source: openapi/linqalpha-openapi-original.json
---

# Upload internal documents to LinqAlpha and make them searchable

LinqAlpha exposes two parallel ingestion paths for customer-owned content. Both are three-step
presign / upload / register flows with asynchronous indexing, and both require polling before the
content is usable.

> **Addressing note.** None of these operations declares an operationId in the published OpenAPI.
> They are addressed by method + path as the spec defines them.

## Which path to use

- **Vault** (`/v2/vault/*`) — the current flow. Returns an `rms_document_id`, which is the id you
  pass to analytics queries.
- **RMS source batches** (`/v1/source_batches`, `/v1/sources`) — the older grouped flow. Use when
  you need documents organized into a batch that a conversation can reference wholesale via
  `source_batch_id`.

## Vault flow

1. **Get an upload URL.** `POST /v2/vault/presigned_url` returns a single-use presigned PUT URL.
   Record the `workspace` you upload into — you must pass the same value at confirm time and again
   when querying.

2. **Upload the bytes.** `PUT` the file body directly to that URL. This goes to object storage,
   not to the LinqAlpha API — do not send your `X-API-KEY` on this request, and do not retry
   against the same single-use URL. If the PUT fails, request a fresh URL.

3. **Register it.** `POST /v2/vault/confirm` registers the uploaded file and triggers async
   ingestion (parse, chunk, embed, index). It returns immediately with an `rms_document_id`.
   Returning does not mean the document is searchable.

4. **Poll until searchable.** `GET /v2/vault/status` for one or more documents. Wait until status
   is `Synced`. Querying before that yields empty or partial retrieval with no error.

## RMS source-batch flow

1. `POST /v1/source_batches` — create the container first; a batch must exist before any upload.
   Keep the returned `source_batch_id`.
2. `POST /v1/sources/upload_url` — presigned S3 PUT URL bound to a server-generated object key.
3. `PUT` the file body to that URL.
4. `POST /v1/sources` — register the upload under the batch and queue parsing.
5. `GET /v1/sources/{source_id}` — poll until `status` is `"success"`. Only then is the source
   searchable in chat or search conversations that reference its `source_batch_id`.

## Checking sync health

- `GET /v1/status/documents` — per-document sync status. Requires at least one filter (`path`,
  `name`, or `document_ids`).
- `GET /v1/status/containers` — per-container status with direct child ids. Also requires a filter.
- `GET /v1/status/sync` — organization-wide overview: searchable-aware document counts and recent
  sync job history. Use this to answer "is my corpus fully indexed".

## Terminal failures

Document entries return a deterministic `fail_code`, `null` unless the document terminally failed.
Branch on it rather than parsing status text:

- `PASSWORD_PROTECTED` — the file is encrypted; it will never index. Ask for an unlocked copy.
- `FILE_SIZE_EXCEEDED` — split the document and re-upload.
- `CONVERSION_FAILED` — the parser could not read the format; re-export it (e.g. to PDF).

Treat all three as terminal. Do not re-poll and do not re-upload the identical file.

## Then query over it

Pass the `rms_document_id` values in `search_types.rms[].document_ids` on
`POST /v2/analytics/sse`, and **set `source` to `"vault"`** for vault documents. Omitting it makes
retrieval fail silently — the answer still renders but cites nothing. See
`skills/linqalpha-grounded-research.md`.

## Conventions

- **No idempotency keys.** A retried `confirm` or `POST /v1/sources` can register the same file
  twice. Check status before retrying a registration.
- Errors use the vendor envelope `{ "error": { "code", "message" }, "payload": null }`; branch on
  `error.code`. See `errors/linqalpha-problem-types.yml`.
