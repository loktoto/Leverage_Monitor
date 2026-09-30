# Methodology

## 1. Objective

Operate one deterministic leverage-monitoring strategy as a realistic **paper-live journal**, as if the strategy were using its own capital. The purpose is to measure whether the rules add value versus their unleveraged 1x benchmarks without contaminating the record with discretionary user-specific decisions.

This repository does not have broker order authority.

## 2. Universe and authority

### SOXX — formal production
SOXX is the only formal production signal. Its formal state is generated only from fully completed US regular-session daily bars.

### SPY and QQQ — live-test
SPY/SSO and QQQ/QLD may be recorded prospectively when the current production live-test policy authorizes them. These records are paper-live observations, not broker executions.

### SMH — context only
SMH is a semiconductor cross-check. It can confirm or contradict SOXX contextually, but has zero formal production weight.

## 3. SOXX formal model

Using completed US RTH daily closes only:

1. SMA50 = 50-session simple moving average.
2. SMA200 = 200-session simple moving average.
3. RV40 = sample standard deviation (`ddof=1`) of the latest 40 simple close-to-close returns × sqrt(252).
4. Raw exposure:
   `R = clip(0.40 / RV40, 0.50, 1.50)`
5. If SMA50 > SMA200, smooth raw exposure using EWM span 5.
6. If SMA50 <= SMA200, exposure is 0.50x.
7. Formal leverage entry = prior E <= 1.0 and current E > 1.0.
8. Formal leverage exit = prior E > 1.0 and current E <= 1.0.

No incomplete RTH bar, premarket price or after-hours print may create a formal cross.

## 4. Live-test lifecycle

User-facing lifecycle states are only:

- `OPEN`
- `EXIT`

There are no pending states.

### OPEN
A trade becomes OPEN only when:
- the underlying strategy has a valid new completed-RTH entry signal;
- required same-run validation passes;
- the actual leveraged product is available;
- a fresh qualifying IBKR RTH quote is observed;
- spread and quote-integrity rules pass.

For long positions, entry price is the observed **ask**.

### EXIT
An exit/invalidation trigger changes the strategy state to EXIT immediately.

For a long position, the recorded exit price is the first qualifying fresh IBKR RTH **bid** observed after the trigger.

If a fresh qualifying RTH exit bid was not captured during the original run:
- state is still EXIT immediately;
- first attempt an audited historical Level-1 reconstruction;
- use the first timestamped RTH bid at/after the original eligible time that passes the same spread/quote-integrity gates;
- prefer IBKR historical Level-1 when available, otherwise Alpaca SIP historical Level-1;
- label the result `HISTORICAL_QUOTE_RECONSTRUCTION`;
- if no authoritative archive exists, keep exit price and realized P&L as N/A.

A reconstruction is not arbitrary backfilling: later closes, premarket/AH prices, OHLC bars treated as bid/ask, interpolation, estimates, and cherry-picked favourable quotes remain prohibited.

A later re-entry is always a new trade instance.

## 5. Real-life simulation discipline

The journal acts as if the strategy were self-funded:

- no signal is suppressed because of the user's real holdings or preferences;
- no discretionary “wait because it feels risky” override is permitted;
- no retrospective ideal fill is permitted; audited replay of the first qualifying historical Level-1 quote is permitted when the original observation was missed;
- actual leveraged-product prices must be used;
- underlying return × leverage is never accepted as a substitute for product return;
- spreads, stale quotes, missing observations and lifecycle latency are part of the measured strategy reality.

This is still paper-live research. No order is sent to a broker.

## 6. Data governance

### IBKR
Primary authority for:
- completed RTH history;
- fresh bid/ask observations;
- lifecycle entry/current/exit marks.

Each failed required endpoint is retried once.

### Alpaca
Independent completed-close parity and historical quote archive.

For close parity:
1. SIP
2. delayed_sip
3. IEX

PASS requires the same completed date and close difference <= 0.20%. Never blend with IBKR.

For missed lifecycle reconstruction, Alpaca SIP historical Level-1 quotes may supply the first qualifying RTH bid/ask when IBKR historical Level-1 is unavailable. Delayed-SIP/IEX are lower-fidelity references and must be labelled explicitly.

### Longbridge
Independent contextual validator only. Zero formal signal weight.

### GitHub
Authority for:
- model identity;
- policy;
- live-test state;
- ledger;
- methodology;
- daily audit record.

GitHub is never fresh market data.

## 7. Validation

For any preliminary action confidence > 6.0, the same run must perform a second validation pass. At minimum:

- second IBKR observation/history where relevant;
- independent recomputation;
- Alpaca completed-close parity;
- at least two relevant contextual/cross-market checks when useful;
- active search for contrary evidence.

If required validation for a new entry is unavailable, final confidence is capped at 6.0 and a new OPEN is not recorded.

## 8. Daily audit record

Every 17:00 HKT run writes a journal entry containing:

- exact HKT/ET timestamps and US session;
- latest completed RTH date;
- all six production-file SHAs;
- source status and IBKR retry result;
- SOXX close/SMA50/SMA200/RV40/raw R/E/formal state;
- SPY and QQQ strategy/live-test state;
- SMH contextual confirmation/contradiction;
- OPEN/EXIT/no-action decision trace;
- actual product lifecycle price/mark when valid;
- same-period 1x benchmark;
- leveraged return, 1x return and excess return;
- validation result and contrary evidence;
- repository/state/ledger changes;
- methodology/policy version.

## 9. Methodology changes

A rule change must:
1. update the relevant machine-readable production policy;
2. update the matching human-readable documentation;
3. add an entry to `docs/CHANGELOG.md`;
4. never rewrite historical trades to make the new rule look better.

Historical records remain evaluated under the rule set that existed when the event was recorded.


## 10. Historical quote reconstruction

The 17:00 HKT schedule frequently occurs before US RTH. Therefore lifecycle prices do not have to remain missing merely because the scheduled report ran premarket.

When a strategy action becomes eligible for the next US RTH:
1. record the signal prospectively at the 17:00 HKT run;
2. on the next run, query the archived Level-1 record for that eligible RTH session;
3. deterministically select the first quote that satisfies the original rule and execution gates;
4. use ask for a long entry and bid for a long exit;
5. store exact timestamp, source, spread, feed/fidelity and reconstruction time;
6. compare the leveraged product with the closest same-session 1x quote at the same timestamp;
7. never choose a later quote because it produces a better result.

This makes the paper-live journal replayable while preserving execution realism.
