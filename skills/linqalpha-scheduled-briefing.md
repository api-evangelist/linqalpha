---
name: Schedule and retrieve a LinqAlpha research briefing
description: Create a recurring investment-research briefing over a topic and set of tickers, run it on demand, and pull the delivered content with its citations.
api: openapi/linqalpha-openapi-original.json
operations:
  - createBriefing
  - listBriefings
  - updateBriefing
  - deleteBriefing
  - briefingPreview
  - briefingRun
  - listBriefingDeliveries
  - getBriefingDeliveryDirect
  - getBriefingDeliveryReferences
generated: '2026-07-19'
method: generated
source: openapi/linqalpha-openapi-original.json
---

# Schedule and retrieve a LinqAlpha research briefing

A Briefing is a saved, scheduled research prompt over a topic and a set of tickers. Each run
produces a Delivery containing the generated briefing and the sources it cited.

Every operationId below is declared verbatim in the published LinqAlpha OpenAPI.

## Before you start

- Base URL is `https://api.linqalpha.com`.
- Authenticate every request with the `X-API-KEY` header. There are no scopes on the REST API.
- If you hold a platform-tier key, pass `organization_id` (and `user_id` where accepted) to select
  the tenant; omit it and the key's primary organization is used.
- Tickers must be resolved to Bloomberg stock ids before use — call `GET /v1/map_tickers` with the
  ticker strings and keep the returned `stock_ids`.

## Steps

1. **Resolve tickers.** `GET /v1/map_tickers` with the ticker strings you were given. Use the
   returned `stock_ids` for every later call; do not pass raw tickers where a `stock_ids` field
   is expected.

2. **Try it before you save it.** Call `briefingPreview` (`POST /v1/briefings/preview`) with an
   ad-hoc prompt. This is fire-and-forget: it returns a pending delivery handle immediately.
   Poll `getBriefingDeliveryDirect` (`GET /v1/briefings/deliveries/{delivery_id}`) until the
   delivery leaves `pending`. Do not block on the enqueue call.

3. **Create the schedule.** `createBriefing` (`POST /v1/briefings`) with the topic, `stock_ids`
   (or a `watchlist_group_id`), and the schedule, in a single call.

   There is **no idempotency key on this API**. If a create times out, do not blindly retry —
   call `listBriefings` (`GET /v1/briefings`) first and check whether the briefing already
   exists, or you will create a duplicate schedule.

4. **Run on demand.** `briefingRun` (`POST /v1/briefings/{id}/generate`) executes an existing
   briefing outside its schedule. For a live token stream instead of a polled result, use
   `briefingSse` (`POST /v1/briefings/sse`) and consume `text/event-stream`.

5. **Read the history.** `listBriefingDeliveries` (`GET /v1/briefings/{id}/deliveries`) returns
   the delivery history. This list is paginated with `page` and `per_page` — page numbers, not
   cursors. Keep requesting pages until a short page comes back.

6. **Fetch content and citations.** `getBriefingDeliveryDirect` returns the full delivery detail.
   `getBriefingDeliveryReferences` (`GET /v1/briefings/deliveries/{delivery_id}/references`)
   returns the citation sources behind it. Always surface the references alongside the text —
   the briefing content carries `[N]` markers that only resolve through this call.

7. **Maintain.** `updateBriefing` (`PATCH /v1/briefings/{id}`) — all fields optional.
   `deleteBriefing` (`DELETE /v1/briefings/{id}`).

## Delivery status values

`pending`, `sent`, `failed`, `resynced`, `skipped`. Treat `pending` as "keep polling";
`failed` and `skipped` are terminal.

## Error handling

Errors are **not** RFC 9457. Every failure returns HTTP 4xx/5xx with:

```json
{ "error": { "code": "INVALID_REQUEST_BODY", "message": "..." }, "payload": null }
```

Branch on `error.code`, never on `error.message`. Note `error.msg` is deprecated — read
`error.message`.

- `API_KEY_MISSING` / `INVALID_API_KEY` (401) — fix the header; do not retry as-is.
- `ORGANIZATION_ID_MISSING` (400) — platform key without an `organization_id`.
- `INVALID_REQUEST_BODY` (400) — validation; do not retry unchanged.
- 5xx (`*_FAIL`) — transient, safe to retry reads with backoff. Retrying **writes** is not safe
  without first checking for a duplicate (see step 3).

Full catalog: `errors/linqalpha-problem-types.yml`.
