---
name: Check THORChain liquidity, savers and RUNEPool positions
description: >-
  Read an address's liquidity-provider, savers-vault, and RUNEPool positions
  across THORNode and enrich with Midgard member history. Read-only, no auth.
api: openapi/thorchain-thornode-openapi.yaml
operations: [liquidityProvider, saver, runeProvider, GetMemberDetail, GetSaverDetail]
---

# Check positions for an address

## Steps

1. **Liquidity-provider position.** Call `liquidityProvider`
   (`GET /thorchain/pool/{asset}/liquidity_provider/{address}`) for the LP's
   units, deposited asset/rune value, and pending amounts in one pool. Use
   `liquidityProviders` to list all LPs in a pool.

2. **Savers position.** Call `saver`
   (`GET /thorchain/pool/{asset}/saver/{address}`) for a single-sided savings
   position (deposit value, growth, last-add height).

3. **RUNEPool position.** Call `runeProvider`
   (`GET /thorchain/rune_provider/{address}`) for the address's RUNEPool
   contribution and current value.

4. **Enrich with history (Midgard).** Call `GetMemberDetail`
   (`GET /v2/member/{address}`) for a cross-pool membership summary, and
   `GetSaverDetail` (`GET /v2/saver/{address}`) for savers history.

## Rules
- A 404 means the address has no position in that pool — treat as "empty", not an error.
- Values are 1e8 fixed-point; a position spans asset + rune legs (dual-sided LPs).
- See errors/thorchain-problem-types.yml and conventions/thorchain-conventions.yml.
