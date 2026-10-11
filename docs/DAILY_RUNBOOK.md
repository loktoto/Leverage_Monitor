# Daily Runbook — 17:00 HKT

## Purpose

Run the strategy journal once per day and publish the Unified Leverage Decision Board. The run records what the strategy would do, not what the user personally should do.

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

17:00 HKT is normally before US RTH during daylight-saving months. A 17:00 run must **not** manufacture an RTH entry or exit. If the strategy action becomes eligible for a later RTH session, the journal records the signal prospectively. A later run should then replay the timestamped historical Level-1 archive and recover the first qualifying RTH bid/ask when an authoritative archive is available.

This is an audited reconstruction, not an invented fill. Later closes, premarket/AH prices, OHLC approximations, interpolation or cherry-picked quotes are prohibited.

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
