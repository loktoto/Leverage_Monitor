# Performance Accounting

## Core comparison

Every trade is measured against the same-period unleveraged 1x underlying.

| Strategy vehicle | 1x benchmark |
|---|---|
| SSO | SPY |
| QLD | QQQ |
| Formal SOXX leveraged vehicle | SOXX |

## Price sides

For a long trade:

- leveraged product entry = ask;
- leveraged product current liquidation / exit = bid;
- 1x benchmark entry = underlying ask when observed at the same timestamp;
- 1x benchmark current / exit = underlying bid when observed at the same timestamp.

If the exact same-timestamp underlying quote is unavailable, use the closest reliable same-session observation and label the basis. Never silently mix timestamps.

## Return formulas

Leveraged return:

`R_L = P_L,current_or_exit / P_L,entry - 1`

1x benchmark return:

`R_1x = P_1x,current_or_exit / P_1x,entry - 1`

Excess return in percentage points:

`Excess_pp = 100 × (R_L - R_1x)`

Do not multiply the underlying return by 2x or 3x to estimate product return.

## OPEN trades

Show:

- entry timestamp and ask;
- current fresh bid when lifecycle-valid;
- leveraged P&L;
- same-period 1x P&L;
- excess return in percentage points;
- holding sessions;
- MAE/MFE/max drawdown when derivable from actual product history.

## EXIT trades

If a valid exit bid was observed, show realized:
- leveraged return;
- 1x return over identical interval;
- excess return;
- holding period;
- exit reason.

If no qualifying exit price was observed:
- exit price = N/A;
- realized leveraged return = N/A;
- realized 1x comparison = N/A for realized-trade accounting;
- excess return = N/A;
- never backfill.

## Cumulative accounting

When enough valid realized trades exist, maintain:

- completed priced-trade count;
- compounded leveraged-product return;
- compounded 1x benchmark return;
- cumulative excess return;
- wins / losses;
- current OPEN unrealized return separately.

Unpriced EXIT records remain lifecycle evidence but are excluded from realized-return compounding.

## Costs

Observed entry ask / current-or-exit bid naturally includes quoted spread crossing. Commission, financing and other costs are included only when explicitly configured in policy. Never invent a cost assumption merely to fill a field.
