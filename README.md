# Leverage Monitor

Canonical repository for the Unified Leverage Decision Board.

## Production authority

Only files under `production/` are authoritative for the leverage monitor:

- `leverage_signal.json` — formal SOXX model identity and rules
- `leverage_signal_reporting_policy.json` — reporting and validation policy
- `leverage_live_test_policy.json` — SPY/SSO and QQQ/QLD live-test lifecycle policy
- `leverage_live_test_state.json` — current live-test state
- `leverage_live_test_ledger.csv` — lifecycle audit ledger
- `leverage_operations.json` — source / contract / operational settings

## Repository hygiene

Do not commit:
- raw or licensed IBKR market data
- broker account, position, order, execution, credential, or session data
- ad-hoc source snapshots
- generated reports / previews
- temporary backtest artifacts
- unrelated TA, travel, or web files

The monitor is research / strategy-journal infrastructure only. It must never create, modify, or transmit broker orders.
