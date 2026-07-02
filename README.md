# Market Data Artifacts (private backup)

Generated-but-not-committed **stock-market data** from the
[`global-market-scanners`](https://github.com/herrrickshaw/global-market-scanners)
platform. Private backup only — derived market data, no personal files, no secrets.

## Contents
| Folder | What |
|---|---|
| `caches/` | Derived data caches: `edgar_facts.db` (point-in-time SEC EDGAR facts), `fundamentals_cache.db` (yfinance fundamentals), `market.duckdb` (analytical warehouse), `viability*.db` / `dvm_*.db` / `pit_backtest*.db` (backtest & screen results) |
| `seed_ohlc/` | 19-market daily OHLC seed (`cleaned_long_*.parquet`) — US, CN, JP, KR, TW, CA, AU, HK, UK, EU, SE, SG, BR, SA, DE, CH, ZA, FI, DK |

Large files via Git LFS. These are **regenerable** by the platform's producers; this
is a convenience backup so a fresh machine doesn't re-fetch everything.

Not included (too large / out of scope): `~/Downloads/data` full scan outputs (~1.4 GB).
