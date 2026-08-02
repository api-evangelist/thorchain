---
name: Quote and track a THORChain cross-chain swap
description: >-
  Get a swap quote (expected output, fees, slippage, inbound vault address and
  memo), then track the swap to completion by transaction hash. Read-only,
  no auth. The actual swap is executed by broadcasting a signed L1 transaction
  to the returned inbound address with the returned memo — THORChain has no
  authenticated write API.
api: openapi/thorchain-thornode-openapi.yaml
operations: [quoteswap, inboundAddresses, txStatus, txStages, txSigners]
---

# Quote and track a THORChain swap

THORChain swaps native L1 assets (e.g. BTC -> ETH) with no wrapping. All calls
below are unauthenticated GETs. Assets use `CHAIN.TICKER-ID` notation and
amounts are 1e8 fixed-point integers.

## Steps

1. **Get a quote.** Call `quoteswap` (`GET /thorchain/quote/swap`) with
   `from_asset`, `to_asset`, `amount`, and `destination` (the recipient address
   on the destination chain). The response includes `expected_amount_out`,
   fees, slippage (`fees.slippage_bps`), `inbound_address`, `memo`, and any
   `expiry`. Respect `inbound_confirmation_blocks` and `outbound_delay_seconds`.

2. **Confirm the inbound target is live.** Cross-check `inbound_addresses`
   (`GET /thorchain/inbound_addresses`) for the source chain — verify the chain
   is not `halted` and use the current `address`, `router` (for EVM), and
   `gas_rate`. NEVER reuse a cached vault address; vaults churn.

3. **Execute off-API (on-chain).** Broadcast a signed transaction on the source
   chain sending `amount` to `inbound_address` with the quote's `memo` in the
   transaction memo/data field. This step is done with a wallet/signer, not
   this API.

4. **Track status.** Poll `txStatus` (`GET /thorchain/tx/status/{hash}`) with
   the inbound tx hash for high-level stage completion, and `txStages`
   (`GET /thorchain/tx/stages/{hash}`) for detailed stage flags
   (`inbound_observed`, `swap_finalised`, `outbound_signed`). Use `txSigners`
   (`GET /thorchain/tx/details/{hash}`) for full observed detail.

## Rules
- No idempotency keys (read-only API); on-chain writes are idempotent by tx hash.
- On 404 from status endpoints, the inbound tx has not yet been observed — keep polling.
- See conventions/thorchain-conventions.yml (asset notation, memos, amounts) and
  errors/thorchain-problem-types.yml.
