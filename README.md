# uniswap-v2

A [nuthatch](https://github.com/nuthatch-org/nuthatch) nest: **Uniswap V2 on Ethereum**.

Every pair the factory has created, and the swaps, mints, burns and syncs on them.

One binary, one config file, no graph-node, no gateway, no query fees.

## What it indexes

**Chain:** `mainnet`. **1 contract**, **5 tables**.

| alias | address |
|---|---|
| `factory` | `0x5c69bee701ef814a2b6a3edd4b1652cb9cc5aa6f` |

## Verified

Indexed blocks **25,781,624 to 25,811,560** and sealed **51,543 events**. Every table below is generated from the vendored ABIs, and the run above is what this nest actually decoded, not an estimate.

## Read this before trusting it

- Deliberately the trading surface only. The pair contract is also an ERC-20 for its LP shares; indexing that `Transfer`/`Approval` pair would add two rows per liquidity movement on every pair ever created, for data none of the trading views read.

## Run it

```sh
nuthatch init --from https://github.com/nuthatch-org/uniswap-v2
cd uniswap-v2
nuthatch dev --dir . --backfill 50000 --seal-direct
nuthatch sql --dir . "SELECT count(*) FROM \"factory__pair_created\""
```

The endpoint in `nuthatch.toml` is keyless and public, so this file is publishable: a `nuthatch.toml` is pinned into the nest's content address and must never carry a credential. It is enough to follow the tip. A **backfill** wants archive depth it may not have: pass your own with `--rpc`, and check it first with `nuthatch doctor --rpc <url>`.

## Tables

```
factory__pair_created
pair__burn
pair__mint
pair__swap
pair__sync
```
