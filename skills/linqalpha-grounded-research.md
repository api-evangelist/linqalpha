---
name: Run grounded LinqAlpha analytics and verify the citations
description: Ask an agentic investment-research question over SEC filings, transcripts and news, capture the answer's citations, resolve the source documents, and score how well each claim is grounded.
api: openapi/linqalpha-openapi-original.json
operations:
  - "POST /v2/analytics/sse"
  - "GET /v2/analytics/conversations/{conversation_id}/references"
  - "POST /v2/analytics/messages/{chat_message_id}/judge"
  - "GET /v2/documents/{document_id}/presigned_url"
  - "GET /v1/map_tickers"
  - "POST /v1/conversations/{conversation_id}/feedback"
generated: '2026-07-19'
method: generated
source: openapi/linqalpha-openapi-original.json
---

# Run grounded LinqAlpha analytics and verify the citations

This is LinqAlpha's marquee flow: a multi-step agentic answer over primary financial sources,
followed by the citation trail that proves it.

> **Addressing note.** The published OpenAPI declares operationIds only for the Briefing tag.
> The operations in this skill are addressed by method + path, exactly as they appear in the
> spec. Do not guess an operationId for them — none is published.

## Before you start

- Base URL `https://api.linqalpha.com`; authenticate with the `X-API-KEY` header.
- Platform-tier keys must pass `organization_id` (and `user_id` where accepted).
- Resolve tickers to Bloomberg ids with `GET /v1/map_tickers` before filtering on `stock_ids`.

## Steps

1. **Ask the question.** `POST /v2/analytics/sse`. The response is a Server-Sent Events stream,
   not a JSON body — consume it incrementally with `Accept: text/event-stream`.

   Choose sources with `search_types`: `external` (SEC filings, transcripts, IR decks, news),
   `rms` (your organization's internal documents), `structured`. Under `filters.rms`,
   `hard_filters` exclude non-matching documents (AND across fields, OR within an array) while
   `soft_filters` only boost relevance.

2. **Consume the stream correctly.** Events arrive in this order:

   ```
   conversation        -> conversation_id for the session
   message             -> echo of your query
   status: start
   [ agentic loop: tool_use_block -> tool_result_block -> think ]   (repeats)
   status: keep_alive  -> heartbeat, emitted on gaps over ~15s
   answer              -> the answer, with inline [N] citation markers
   status: finish
   ```

   Capture two ids as they go by: `conversation_id` from the `conversation` event, and
   `chat_message_id` from the event whose `event_name == "chat_message_id"`. You cannot recover
   them afterwards — everything downstream needs them.

   Treat `status: keep_alive` as liveness only. Do not time out a stream that is still
   heartbeating; agentic runs are long.

   To abandon a run early, call `POST /v1/stop_stream`.

3. **Pull the citations.** After `status: finish`, call
   `GET /v2/analytics/conversations/{conversation_id}/references`. This returns the normalized
   evidence references with enriched metadata. The `[N]` markers in the answer resolve against
   this list — never present the answer without it.

4. **Open a source document.** `GET /v2/documents/{document_id}/presigned_url` returns a
   time-limited download URL (about 10 minutes). Fetch it promptly; do not cache or store the URL.
   Use `search_type=rms` for internal documents, `external` (the default) for filings and
   transcripts.

5. **Verify the grounding.** `POST /v2/analytics/messages/{chat_message_id}/judge` runs a
   source-grounding judge over the answer. Supply your own `prompt` — it is the entire judging
   instruction, LinqAlpha adds no criteria — and optionally a `response_schema` shaping the
   verdict. The judge reads every source the answer actually cited and flags any figure that
   contradicts its source or appears in none of them. It is deterministic (temperature 0).

   The judge is tenant-scoped: judging another organization's answer returns 404, not 403.

6. **Record quality.** `POST /v1/conversations/{conversation_id}/feedback` submits a rating and
   comment; `DELETE` on the same path removes it.

## Vault documents — the silent-failure trap

To chat over documents uploaded through the Vault flow, pass their `rms_document_id` in
`search_types.rms[].document_ids` **and set `source` to `"vault"`**.

Omit `source: "vault"` and the document is searched generically: it is not retrieved for direct
citation, the `[N]` markers do not resolve, and the references call returns an empty list — while
the answer still looks complete. This fails silently, so always assert that the references list is
non-empty before trusting a vault-grounded answer.

Also note `document_ids` here means `rms_document_id` values returned by
`POST /v2/vault/confirm` — not the `document_id` from the presign step — and `workspace` must
match the workspace the document was uploaded into.

## Conventions

- **Idempotency: not supported.** No idempotency key exists anywhere in this API. A retried
  analytics call starts a new conversation and re-bills the work.
- **Pagination:** `page` / `per_page` on list endpoints. No cursors.
- **Rate limits:** the only published limit is 60 requests/minute per user on the MCP server.
  No rate-limit response headers are documented on the REST API — back off on 5xx rather than
  relying on headers.
- **Errors:** vendor envelope `{ "error": { "code", "message" }, "payload": null }`, not RFC 9457.
  Branch on `error.code`; `error.msg` is deprecated in favour of `error.message`. See
  `errors/linqalpha-problem-types.yml`.
