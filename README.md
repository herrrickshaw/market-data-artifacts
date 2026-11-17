# Market Data Artifacts (private backup)

<!-- how-to-use-with-ai -->
> **New here?** See [How to use this repo with AI (Claude, Gemini, ChatGPT)](HOW_TO_USE_WITH_AI.md) — clone it, paste it into an AI tool, and ask in plain English.


Generated-but-not-committed **stock-market data** from the
[`global-market-scanners`](https://github.com/herrrickshaw/global-market-scanners)
platform. Private backup only — derived market data, no personal files, no secrets.

## Contents
| Folder | What |
|---|---|
| `caches/` | Derived data caches: `edgar_facts.db.zst` (point-in-time SEC EDGAR facts), `fundamentals_cache.db` (yfinance fundamentals), `market.duckdb` (analytical warehouse), `viability*.db` / `dvm_*.db` / `pit_backtest*.db` (backtest & screen results) |
| `seed_ohlc/` | 19-market daily OHLC seed (`cleaned_long_*.parquet`) — US, CN, JP, KR, TW, CA, AU, HK, UK, EU, SE, SG, BR, SA, DE, CH, ZA, FI, DK |

Large files via Git LFS. These are **regenerable** by the platform's producers; this
is a convenience backup so a fresh machine doesn't re-fetch everything.

The four largest SQLite caches (`edgar_facts.db`, `viability.db`, `viability_21d.db`,
`dvm_global.db`) are stored zstd-compressed as `*.db.zst` (~85% smaller). Decompress
before use; the output is byte-identical to the original `.db`:

```bash
for f in caches/*.db.zst; do zstd -d -f "$f"; done
```

Not included (too large / out of scope): `~/Downloads/data` full scan outputs (~1.4 GB).
