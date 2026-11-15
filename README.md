# Token Radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/jekglory/token-radar-01/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/jekglory/token-radar-01/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A token launch radar for make.fun's `token-radar-01` bounty ($1,325).

It reads a sealed DefiLlama snapshot (758 protocols, 2026-11-14), finds the protocols
that record no token and are not a wrapped, staked or bridged product of one that
trades, and ranks them by a comps-calibrated implied value. Every row carries a
six-step farm plan with costs and times.

## Model (radar-2)

`implied = TVL x category multiple x growth x maturity x breadth`

- **Category multiple**: the median market-cap/TVL of the live-token protocols in
  the same category (at least 3 comps, else the global median 0.2460x), clamped to
  0.08–1.2x. Lending prices near 0.13x TVL; bridges near 0.99x. No name is
  special-cased.
- **Growth**: `1 + clamp(change_7d, -60, 120)/100 x 0.5`.
- **Maturity**: `0.6 + min(age_days, 1095)/1095 x 0.4` — how long the protocol has
  run with no token (unknown age: 365 days).
- **Breadth**: `1 + 0.02 x min(chains - 1, 4)`.

## Board

178 rows: the full 40-protocol watchlist, plus 138 early pre-token protocols below
the $50M floor that pass the watchlist's own exclusion rule. 32 live-token
protocols are set aside with their market caps, never mixed into the ranking.

## Run

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input <snapshot.json>
```

Node 22+, standard library only. The dashboard in `dashboard/` is a static page
with no network: open `out/dashboard/index.html` after a run. Screenshots live
in `screenshots/`.

---

<sub>Entered on [make.fun](https://make.fun/?ref=wkrc9se2&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>

