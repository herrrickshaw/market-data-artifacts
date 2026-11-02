# Market data artifacts (LFS backup of generated stock-market data)

A data-only backup of derived stock-market datasets produced by the
[`global-market-scanners`](https://github.com/herrrickshaw/global-market-scanners)
platform: SEC EDGAR / yfinance fundamentals caches, a DuckDB analytical warehouse,
backtest and screen result databases, and a 19-market daily OHLC seed in Parquet.
Its purpose is to let a fresh machine restore these caches instead of re-fetching
everything. There is no source code in this repository.

> Status: archived snapshot (data-only, no code). Derived market data only; no
> personal files, no secrets. Educational use only; not investment advice.
> The repository is public on GitHub even though it was created as a "private backup".

<!-- how-to-use-with-ai -->
> **New here?** See [How to use this repo with AI (Claude, Gemini, ChatGPT)](HOW_TO_USE_WITH_AI.md).

## What it does
- Stores 11 derived cache databases under `caches/` (SQLite `.db` and one DuckDB file).
- Stores a daily OHLC seed for 19 markets under `seed_ohlc/` (`cleaned_long_<MARKET>.parquet`).
- Tracks all 30 data files through Git LFS (`*.db`, `*.duckdb`, `*.parquet` in `.gitattributes`), 240.1 MB in total (audit 2026-07-22).
- Runs a gitleaks secret scan on every push and pull request (`.github/workflows/quality-gates.yml`).

## Quick start
```bash
# The account's Git LFS budget is exhausted (see Limitations), so skip the smudge step
# to avoid clone errors; data files arrive as ~130-byte pointer stubs.
GIT_LFS_SKIP_SMUDGE=1 git clone https://github.com/herrrickshaw/market-data-artifacts.git
cd market-data-artifacts
git lfs ls-files        # lists the 30 tracked objects and their OIDs
```

There is nothing to build or run. If LFS downloads become available again,
`git lfs pull` restores the real files.

## Repository layout
| Path | Purpose |
|---|---|
| `caches/edgar_facts.db` | Point-in-time SEC EDGAR company facts |
| `caches/fundamentals_cache.db` | yfinance fundamentals cache |
| `caches/market.duckdb` | Analytical warehouse |
| `caches/viability*.db`, `caches/dvm_*.db`, `caches/pit_backtest*.db` | Backtest and screen results |
| `seed_ohlc/cleaned_long_*.parquet` | Daily OHLC seed, one file per market: US, CN, JP, KR, TW, CA, AU, HK, UK, EU, SE, SG, BR, SA, DE, CH, ZA, FI, DK |
| `DATA_ACCESS.md` | LFS status, where the data really comes from, no-new-LFS policy |
| `HOW_TO_USE_WITH_AI.md` | Plain-language guide for handing the repo to an AI assistant |
| `.github/workflows/quality-gates.yml` | gitleaks secret scan |

All files here are tracked artefacts; nothing is generated inside this repo.

## Data sources
| Source | What is taken | Access route | Licence / caveat |
|---|---|---|---|
| SEC EDGAR (via `global-market-scanners`) | Point-in-time company facts | Collected upstream, cached as `edgar_facts.db` | Public US filings |
| yfinance (via `global-market-scanners`) | Fundamentals | Collected upstream, cached as `fundamentals_cache.db` | Yahoo Finance terms apply |
| `global-market-scanners` producers | OHLC seed, warehouse, backtest results | Regenerable by the platform | Derived data; not an original source |

The data is sourced from `global-market-scanners`, not from `global-market-data`.

## Limitations and known issues
- 🔴 **Git LFS is currently unreachable**: the account's LFS budget is exhausted, so
  `git clone` / `git lfs pull` cannot download the data files. This is account-wide,
  not specific to this repo. Clone with `GIT_LFS_SKIP_SMUDGE=1`.
- **Policy: do not add new LFS objects** — they would be born unreachable. New data goes
  in as gzipped/Parquet regular git objects under 50 MB, one canonical format, with its
  collector script committed alongside.
- Contents overlap `global-stock-screener`; check there first for a readable copy.
- Not included (too large / out of scope): the full scan outputs in `~/Downloads/data` (~1.4 GB).
- All files are regenerable by the platform's producers; this is a convenience backup, not an authoritative source.

## Related repositories
- [`global-market-scanners`](https://github.com/herrrickshaw/global-market-scanners) — the platform that produces these artefacts
- [`repo-data-dedup`](https://github.com/herrrickshaw/repo-data-dedup) — pointer inventory, dedup plan, [`SOP_DATA_SOURCES.md`](https://github.com/herrrickshaw/repo-data-dedup/blob/main/SOP_DATA_SOURCES.md) re-collection SOP, [`PLAYBOOK.md`](https://github.com/herrrickshaw/repo-data-dedup/blob/main/PLAYBOOK.md) migration recipe off LFS
- `global-stock-screener` — holds an overlapping, readable copy of these caches

## Licence
No licence file yet — all rights reserved.
