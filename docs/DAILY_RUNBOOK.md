# Daily Runbook — US RTH Open + 1 Minute (09:31 ET)

## Purpose

Run the strategy journal once per US equity regular trading session at 09:31 America/New_York (one minute after the 09:30 ET open) and publish the Unified Leverage Decision Board. The run records what the strategy would do, not what the user personally should do.

## Run sequence

1. Fetch all six production authority files from `loktoto/Leverage_Monitor@main`.
2. Record each file SHA.
3. Resolve HKT/ET, US market session and latest fully completed US RTH date.
4. Pull sufficient IBKR completed-RTH history for SOXX, SPY, QQQ and SMH.
5. Retry each failed required IBKR endpoint once.
6. Recompute SOXX formal model from source history.
7. Recompute current SPY/QQQ live-test entry/exit conditions.
8. Check every OPEN trade for exit/invalidation.
9. If US RTH is open, obtain a fresh lifecycle-side quote.
10. If a prior eligible RTH lifecycle observation was missed, reconstruct the first qualifying timestamped historical Level-1 quote under the reconstruction policy.
11. Run Alpaca completed-close parity using SIP → delayed_sip → IEX.
12. Use Longbridge only as an independent contextual check.
13. If preliminary confidence > 6.0, run second-pass validation.
14. Record any genuine OPEN/EXIT lifecycle event or audited historical reconstruction in state and ledger.
15. Calculate leveraged-product return, same-period 1x return and excess return.
16. Update the monthly `journal/YYYY-MM.md` with the complete audit entry.
17. Update methodology docs only if a rule/policy actually changed.
18. Deliver the user-facing board.

## Important timing limitation

Schedule is anchored to **09:31 America/New_York on US equity regular-session trading days**, exactly one minute after the ordinary 09:30 ET market open. This is **21:31 HKT in US daylight time (EDT)** and **22:31 HKT in US standard time (EST)**. Weekday scheduler attempts must verify the actual NYSE/Nasdaq market calendar: on a US holiday, emergency closure, or any day with no actual RTH open, do not publish an ordinary open-plus-one-minute market report and never manufacture an opening quote. Current-day RTH bars are incomplete at 09:31 ET: formal daily-close signals must use the **latest prior completed RTH session**. A live 09:31 quote is not automatically the **first** valid quote from the original 09:30 eligibility time. When that first qualifying bid/ask was missed, replay original-time historical timestamped Level-1 (IBKR if available, else Alpaca SIP) under the original spread/halt/integrity gates; if unavailable at 09:31, mark lifecycle price N/A and repeat reconstruction on later runs, never use a future favorable or after-hours quote as a substitute. All paper trades remain non-broker executions.

This is an audited reconstruction, not an invented fill. Later closes, premarket/AH prices, OHLC approximations, interpolation or cherry-picked quotes are prohibited.

## Supplementary verified-data visualizations

When data are valid, show SOXX completed-RTH E relative to 1x, a normalized completed-RTH performance comparison of the four monitored assets and actual recorded leveraged-product versus 1x returns. Charts do not replace the mandatory four-row decision table, two-test lifecycle table or three-product performance scoreboard. Never substitute estimated prices for missing quote-side evidence, and mark current RTH vs completed RTH clearly.

## Repository write rules

### Always allowed
- derived indicators;
- signal/state decisions;
- lifecycle timestamps;
- qualifying observed bid/ask/price;
- performance metrics;
- source labels;
- validation outcome;
- policy version;
- daily audit narrative.

### Never commit
- raw licensed IBKR bars;
- account balances;
- user positions;
- real broker orders/executions;
- credentials/tokens;
- session data.

## User-facing state vocabulary

Use only `OPEN` and `EXIT` for trade lifecycle. Audit-only outcomes such as DATA_CONFLICT may appear as data-quality labels, not as a third trade state.

## Canonical vehicle and compulsory product performance (effective 2026-10-11 HKT)

SOXX's selected leveraged-product route is **USD only**, prospective from 2026-10-11. Re-read `production/leverage_signal.json` and `production/leverage_live_test_policy.json` every run. Do not open USD retroactively from 2026-10-01 merely because eligible historical SIP quotes exist; await a prospective qualifying lifecycle event.

After the four-asset decision table (and the max-two live-test lifecycle table), **always show a separate three-row USD / QLD / SSO performance scoreboard**. For each vehicle, label OPEN or EXIT where applicable, or say "no recorded OPEN"; show exact entry ask and mark/exit bid, matching SOXX/QQQ/SPY 1x figures, leveraged-product return, excess pp, timestamp/source/fidelity and realized versus unrealized. When no canonical OPEN exists, official P&L must be N/A; optional pre-selection shadow figures must be identified as hypothetical. Disclose discrepancies between an audited journal lifecycle and live-test state/ledger until reconciled.

## Mobile-first user-facing report layout (effective 2026-10-14)

The earlier 11-column Markdown decision table and wide lifecycle/product tables rendered as literal pipe-delimited source on iPhone, making the report unreadable. **Do not print any wide or pipe-delimited Markdown table in the user-facing report.**

- Display the four-asset decision board as exactly **four vertical mobile-friendly panels**: SOXX, SPY, QQQ, SMH. Each must retain **all** original decision-table fields: asset, decision, vehicle, authority/state, completed signal and date, trigger distance, leverage-vs-1x, contrary evidence, M/D/E/L, Final confidence, and direct Cantonese verdict. Keep 1–2 important values per line.
- Display live-test SPY/SSO and QQQ/QLD as **up to two vertical panels**, with original entry, qualifying current/exit bid, product and 1x return, excess pp, MAE/MFE/DD, holding sessions, invalidation and GitHub write status.
- Display the compulsory USD, QLD, SSO performance scoreboard as **exactly three vertical product panels** with all original columns retained semantically, including dates/sources, realized/unrealized/N/A distinction.
- Show SOXX formal SMA50/SMA200/RV40/raw R/E and next-cross distance in short labeled rows, not a wide table.
- Lead with action first and HKT/ET, session, latest completed RTH, and data freshness. Native inline charts may supplement the prose only when sufficient trustworthy time-series data exist. Charts must never replace a decision or fabricate results.
- Store all six full GitHub production-file SHAs and detailed audit in journal/YYYY-MM.md. In the user reply use a compact GitHub commit/read-back status and an audit link; avoid a wall of long SHA strings.
- This is a **presentation-only change**: no model formula, lifecycle rule, confidence gate, data source authority, original audit field, security policy or broker-order authority changes.
