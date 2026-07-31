---
name: Query a Meter account
description: Read an account's balance, energy, contract code, and storage on the Meter blockchain via the native RESTful API.
api: openapi/meter-openapi-original.yml
operations:
  - GET /accounts/{address}
  - GET /accounts/{address}/code
  - GET /accounts/{address}/storage/{key}
  - POST /accounts/{address}
---

# Query a Meter account

Meter is EVM-compatible; addresses are 20-byte hex (`0x…`). The native RESTful API is
public and unauthenticated. Base URL: `https://mainnet.meter.io` (or
`https://testnet.meter.io` for testnet).

## Steps

1. **Get account detail** — `GET /accounts/{address}` returns `balance`, `energy`, and
   `hasCode`. Add `?revision=<number|id|best|finalized>` for point-in-time state.
2. **Get contract code** — if `hasCode` is true, `GET /accounts/{address}/code` returns the deployed bytecode.
3. **Read a storage slot** — `GET /accounts/{address}/storage/{key}` returns the raw value at a storage key.
4. **Simulate a call (read-only)** — `POST /accounts/{address}` with a `CallData` body
   (`data`, `value`, `gas`, `caller`) executes account code without committing a
   transaction; the `CallResult` reports `reverted` and `vmError`.

## Rules

- No auth header is required; public endpoints are rate-limited (use a dedicated node
  provider for volume).
- Reads are idempotent and safe. Use `revision=finalized` for settled state.
- See `conventions/meter-conventions.yml` for revision addressing and pagination.
