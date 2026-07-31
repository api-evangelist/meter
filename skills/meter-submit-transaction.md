---
name: Submit a Meter transaction
description: Broadcast a signed transaction to the Meter blockchain and retrieve its receipt via the native RESTful API.
api: openapi/meter-openapi-original.yml
operations:
  - POST /transactions
  - GET /transactions/{id}
  - GET /transactions/{id}/receipt
---

# Submit a Meter transaction

Sign the transaction client-side (the native Meter tx format differs from Ethereum's;
use the `meterify` library, or use the Ethereum-RPC endpoint with standard tooling).
Base URL: `https://mainnet.meter.io`.

## Steps

1. **Commit the transaction** — `POST /transactions` with a `RawTx` body (the RLP-encoded
   signed transaction). The response returns the transaction `id`.
2. **Poll the transaction** — `GET /transactions/{id}` confirms inclusion; returns `null`
   until mined.
3. **Fetch the receipt** — `GET /transactions/{id}/receipt` returns `gasUsed`, `paid`,
   `reverted`, and `outputs[]` (emitted events and transfers). If `reverted` is true the
   transaction failed on-chain despite being included.

## Rules

- Submission is idempotent at the chain level: re-broadcasting the same signed tx (same
  `id`) does not double-execute. There is no `Idempotency-Key` header.
- Always check `reverted` before treating a tx as successful.
- See `conventions/meter-conventions.yml` and `data-model/meter-data-model.yml`.
