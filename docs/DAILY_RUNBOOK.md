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
9. Obtain fresh lifecycle-side quote only when the market/session makes it valid.
10. Run Alpaca completed-close parity using SIP → delayed_sip → IEX.
11. Use Longbridge only as an independent contextual check.
12. If preliminary confidence > 6.0, run second-pass validation.
13. Record any genuine OPEN/EXIT lifecycle event in state and ledger.
14. Calculate leveraged-product return, same-period 1x return and excess return.
15. Update the monthly `journal/YYYY-MM.md` with the complete audit entry.
16. Update methodology docs only if a rule/policy actually changed.
17. Deliver the user-facing board.

## Important timing limitation

17:00 HKT is normally before US RTH during daylight-saving months. A 17:00 run must **not** manufacture an RTH entry or exit. If the strategy requires a fresh RTH execution-side observation but no qualifying RTH quote exists at run time, the journal records the strategy signal/state and the missing lifecycle observation explicitly.

No retrospective quote may be invented later to make the record look cleaner.

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
