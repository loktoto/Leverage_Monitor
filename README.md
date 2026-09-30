# Leverage Monitor

Canonical repository for the **Unified Leverage Decision Board** and its paper-live strategy journal.

## Operating mode

The monitor is run as if it were using its own capital, but it is **paper-live only**:

- follow the frozen strategy mechanically;
- record every valid `OPEN` / `EXIT`;
- use observed executable-side prices (long entry = ask, long exit/current liquidation = bid);
- compare the actual leveraged product with the same-period 1x underlying;
- preserve the complete decision, validation and lifecycle audit trail;
- do **not** use the user's personal portfolio, holdings or preferences to alter a signal;
- never create, modify or transmit broker orders.

Scheduled user-facing report: **17:00 Asia/Hong_Kong every day**.

## Authority hierarchy

| Asset | Role |
|---|---|
| SOXX | Formal production signal |
| SPY / QQQ | Production live-test / paper-live journal when policy authorizes |
| SMH | Context only; zero formal trade authority |

## Repository structure

```text
production/
  leverage_signal.json
  leverage_signal_reporting_policy.json
  leverage_live_test_policy.json
  leverage_live_test_state.json
  leverage_live_test_ledger.csv
  leverage_operations.json

docs/
  METHODOLOGY.md
  DAILY_RUNBOOK.md
  PERFORMANCE_ACCOUNTING.md
  CHANGELOG.md

journal/
  README.md
  YYYY-MM.md
```

The six files under `production/` are the machine-readable authority. The files under `docs/` explain the same system in human-readable form. `journal/` contains the derived daily audit record.

## Source hierarchy

1. **IBKR** — primary authority for completed US RTH bars and fresh executable-side observations.
2. **Alpaca** — independent completed-close parity: SIP → delayed_sip → IEX.
3. **Longbridge** — independent context only; zero formal authority.
4. **GitHub** — model identity, policy, state, ledger, methodology and audit. Never a fresh market-data source.

Never average conflicting sources. Never label stale data live. Never use incomplete RTH as a formal close.

## Repository hygiene

Do not commit raw/licensed IBKR bars, broker account data, positions, orders, executions, credentials, session material, ad-hoc source dumps or unrelated generated output.

Derived signal metrics, lifecycle observations, performance calculations, validation results, source labels and daily audit notes are allowed.

See `docs/METHODOLOGY.md` for the complete methodology.
