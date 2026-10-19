## 2026-10-19 — Daily schedule tied to US stock RTH open + 1 minute

- Changed the existing Unified Leverage Board automation from fixed 22:31 Asia/Hong_Kong every day to **09:31 America/New_York on each actual US stock regular-trading-session day**, automatically accounting for US daylight saving time: 21:31 HKT (EDT) / 22:31 HKT (EST).
- Weekday scheduling checks the official US RTH calendar and skips ordinary open-plus-one-minute reports on stock-market holidays and closures; it never claims the incomplete opening-day bar is a completed close.
- The first eligible 09:30 ET Level-1 RTH quote may be missed at 09:31 ET; continue to require first-qualified-original-time historical L1 replay when available, otherwise mark exact lifecycle fill N/A and retry later; 09:31 latest quote is not silently treated as 09:30 fill.
- Schedule and documentation change only: SOXX formal model, trade lifecycle, performance, confidence validation, data-source authority, GitHub audit and broker-order prohibition unchanged.

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

## 2026-10-11 — SOXX canonical USD route and three-product daily performance

- Selected **USD** as the single prospective canonical leveraged ETF for SOXX, effective 2026-10-11 HKT. SOXL remains a research alternative only.
- The formal SOXX completed-RTH SMA/RV/EWM signal and historical entry/exit thresholds remain unchanged.
- Explicitly prohibit retrospectively declaring the 2026-10-01 USD shadow quote to be an official OPEN after the later route selection.
- Mandate the three-row **USD / QLD / SSO** actual-product performance board in every leverage report. Show matching SOXX/QQQ/SPY 1x returns, excess pp and realized/OPEN/N/A classification.
- USD tracks a different semiconductor index than SOXX; this basis mismatch and daily-reset path risk must be disclosed. No broker-order authority granted.

## 2026-10-12 — Operational schedule change to 22:00 HKT

- Rescheduled the existing `Unified Leverage Board` daily automation from 17:00 to **22:00 Asia/Hong_Kong**, effective from 2026-10-13.
- Updated the production operations schedule, reporting scope, daily runbook and methodology's timing notes to match.
- During US daylight saving time 22:00 HKT = 10:00 ET (RTH); during US standard time 22:00 HKT = 09:00 ET (before RTH). A daily completed-RTH signal remains authoritative only after that RTH session has closed.
- No change to SOXX formal formula, SPY/QQQ test signals, lifecycle quote integrity, product selection, portfolio independence or broker-order prohibition.

## 2026-10-12 — 22:31 HKT schedule and evidence-based charting

- Updated daily Unified Leverage Board exact schedule to **22:31 Asia/Hong_Kong**, effective 2026-10-13. 22:31 HKT equals 10:31 ET in US daylight time and 09:31 ET in standard time, ordinarily within US RTH on trading days.
- Updated operations and reporting policy and methodology/runbook timing notes. Completed US RTH closes remain the ONLY formal signal cutoff, regardless of intraday quotes.
- Added supplementary visual presentation: SOXX E/1x cross chart, four-asset normalized completed-RTH trends, and recorded leveraged-product versus matching 1x return chart when valid. Charts must not use fabricated series, substitute for tables, or mix incomplete and completed daily data.
- SOXX formula, 2026-10-11 USD prospective route, SPY/QQQ lifecycle, user-portfolio independence, historical Level-1 reconstruction, and no-broker-order rule are unchanged.

## 2026-10-14 — Mobile-first daily report presentation

- Replaced wide decision/live-test/product Markdown tables with four/two/three narrow vertically stacked panels for iPhone readability; original analytical fields remain mandatory and complete.
- The user-facing report prioritizes action, short labels, data timestamp and freshness; graphs render inline only where validated. Full production-file SHA values remain in the GitHub audit, with concise verified-write status presented to the user.
- No change to the 22:31 HKT schedule, SOXX production model, SPY/QQQ lifecycle, performance calculations, validation standards, repository audit obligations or prohibition on broker orders.
