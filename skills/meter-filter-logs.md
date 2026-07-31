---
name: Filter and stream Meter logs
description: Query event and transfer logs by criteria and range, and subscribe to live block/event/transfer streams over WebSocket.
api: openapi/meter-openapi-original.yml
operations:
  - POST /logs/event
  - POST /logs/transfer
  - GET /subscriptions/event
  - GET /subscriptions/transfer
  - GET /subscriptions/block
---

# Filter and stream Meter logs

Base URL: `https://mainnet.meter.io`; WebSocket: `wss://ws.meter.io`.

## Steps

1. **Filter event logs** — `POST /logs/event` with an `EventFilter` body: a `range`
   (block/time from–to), `options` (`offset`, `limit`), a `criteriaSet` (address +
   `topic0..topic4`), and `order`.
2. **Filter transfer logs** — `POST /logs/transfer` with a `TransferFilter`
   (`txOrigin`/`sender`/`recipient` criteria).
3. **Stream live** — open a WebSocket to `/subscriptions/event`, `/subscriptions/transfer`,
   or `/subscriptions/block` to receive new items as they are produced.

## Rules

- Prefer the singular `/logs/event` and `/logs/transfer` endpoints; the plural
  `/logs/events`, `/logs/transfers`, `/events`, `/transfers` variants are **deprecated**
  (see `lifecycle/meter-lifecycle.yml`).
- Page with `options.offset` / `options.limit`.
- The subscription channels are documented as an AsyncAPI in
  `asyncapi/meter-subscriptions-asyncapi.yml`.
