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
