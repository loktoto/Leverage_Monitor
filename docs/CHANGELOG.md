# Methodology Changelog

## 2026-09-30 — Repository and operating-model reset

- Canonical repository changed to `loktoto/Leverage_Monitor`.
- Daily user-facing run fixed at 17:00 Asia/Hong_Kong.
- Operating mode explicitly defined as `PAPER_LIVE_AS_IF_SELF_FUNDED`.
- User's actual holdings/preferences are excluded from strategy decisions.
- Trade lifecycle simplified to `OPEN` / `EXIT` only.
- Added mandatory actual leveraged-product vs same-period 1x benchmark performance accounting.
- Added daily repository audit journal.
- Added human-readable methodology, runbook and performance-accounting documents.
- Broker order authority remains NONE.
- SOXX formal signal formula is unchanged.


## 2026-09-30 — Audited historical Level-1 reconstruction

- Corrected the assumption that a missed 17:00 lifecycle quote must remain permanently N/A.
- Added deterministic reconstruction from timestamped historical Level-1 records.
- Selection is the first qualifying RTH quote after the original eligible timestamp, using the original spread/integrity gates.
- IBKR historical Level-1 is preferred when available; Alpaca SIP historical quotes are the canonical fallback.
- Later closes, premarket/AH quotes, OHLC approximations, interpolation and cherry-picked prices remain prohibited.
- Reconstructed records are explicitly labelled and are not broker executions.
- Reconstructed the two previously unpriced 2026-09-11 exits from Alpaca SIP Level-1 records.
