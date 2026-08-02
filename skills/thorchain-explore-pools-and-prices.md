---
name: Explore THORChain pools, depths and prices
description: >-
  Discover available liquidity pools, read a single pool's depth/price/status,
  pull historical depth and swap-volume series from Midgard, and read oracle
  prices. All read-only, no auth.
api: openapi/thorchain-thornode-openapi.yaml
operations: [pools, pool, oraclePrices, GetPoolStats, GetDepthHistory, GetSwapHistory]
---

# Explore pools and prices

## Steps

1. **List pools.** Call `pools` (`GET /thorchain/pools`) for every pool with
   `balance_asset`, `balance_rune`, `asset_tor_price`, and `status`
   (`Available` / `Staged` / `Suspended`). Only `Available` pools can be
   swapped.

2. **Inspect one pool.** Call `pool` (`GET /thorchain/pool/{asset}`) for a
   single pool's live depths, LP units, and synth/savers utilisation.

3. **Read prices.** Call `oraclePrices` (`GET /thorchain/oracle/prices`) for
   the network's oracle price feed.

4. **Historical analytics (Midgard).** For time-series, use the Midgard indexer:
   `GetPoolStats` (`GET /v2/pool/{asset}/stats`) for aggregate pool stats,
   `GetDepthHistory` (`GET /v2/history/depths/{pool}`) for depth/price series,
   and `GetSwapHistory` (`GET /v2/history/swaps`) for volume/fee series. These
   accept `interval` (hour/day/week/month), `count`, and `from`/`to` unix bounds.

## Rules
- THORNode gives live state; Midgard gives history/analytics. Pick accordingly.
- Amounts are 1e8 fixed-point; convert to USD via `asset_tor_price` (TOR is the USD peg).
- See conventions/thorchain-conventions.yml for pagination and asset notation.
