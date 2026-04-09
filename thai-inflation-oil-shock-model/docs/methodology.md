# Methodology

## Overview

This project estimates the pass-through of oil price shocks to Thailand's headline CPI inflation using **Local Projections** (Jordà, 2005). This approach is widely used by central banks and policy institutions for its flexibility and robustness.

## Data

### Brent Crude Oil Prices
- **Frequency:** Quarterly averages
- **Source:** EIA / World Bank
- **Period:** 2016-Q1 to 2025-Q3
- **Units:** USD per barrel

### Thailand CPI Index
- **Frequency:** Quarterly
- **Source:** Bank of Thailand
- **Period:** 2017-Q1 to 2025-Q3
- **Base:** 2019 = 100

## Variable Definitions

### Oil Shock
The oil shock is defined as the quarterly log change in Brent crude prices, multiplied by 100 to express in percentage terms:

```
oil_shock_pct = 100 × [ln(Brent_t) - ln(Brent_{t-1})]
```

This transformation:
- Is approximately equal to percentage change for small changes
- Is symmetric (a 10% increase and 10% decrease have equal magnitude)
- Is standard in the macroeconomic literature

### CPI YoY Inflation
Year-over-year inflation is computed from the quarterly CPI index:

```
YoY_Inflation = [(CPI_t / CPI_{t-4}) - 1] × 100
```

## Model Specification

For each forecast horizon h = 1, 2, ..., 8 quarters, we estimate:

```
CPI_YoY_{t+h} = α_h + β_h × oil_shock_t 
                + Σ(k=1 to 4) γ_k × CPI_YoY_{t-k}
                + Σ(k=1 to 4) δ_k × oil_shock_{t-k}
                + ε_{t+h}
```

Where:
- **β_h** is the coefficient of interest: the response of CPI YoY inflation at horizon h to a 1 percentage point oil shock at time t
- **4 lags of CPI YoY** control for inflation persistence
- **4 lags of oil shock** control for delayed pass-through from previous shocks

## Inference

Standard errors are computed using **Heteroskedasticity and Autocorrelation Consistent (HAC)** estimation (Newey-West), with:

```
maxlags = h + 1
```

This accounts for:
- Heteroskedasticity in the error term
- Serial correlation induced by overlapping forecast horizons

95% confidence intervals are constructed as:

```
[β_h - 1.96 × SE(β_h), β_h + 1.96 × SE(β_h)]
```

## Interpretation

The coefficient **β_h** has the following interpretation:

> A 1% increase in Brent crude oil prices is associated with a β_h percentage point change in Thailand's YoY CPI inflation after h quarters.

For scenario analysis, we scale linearly:

```
Impact(h, shock%) = β_h × shock%
```

For example, if β_1 = 0.072, then a 10% oil price shock implies:
- Q1 impact: 0.072 × 10 = 0.72 percentage points

## Limitations

1. **Reduced-form estimate:** This is not a structural model. It captures historical correlations, not causal mechanisms.

2. **Linear assumption:** The model assumes symmetric and proportional effects. In reality, large shocks may have non-linear effects.

3. **Sample size:** With ~25-30 observations for estimation, confidence intervals are relatively wide.

4. **Omitted variables:** The model does not control for other factors affecting inflation (monetary policy, exchange rates, supply shocks).

5. **Time-varying pass-through:** Pass-through may have changed over time due to policy changes or structural shifts in the economy.

## References

- Jordà, Ò. (2005). Estimation and inference of impulse responses by local projections. *American Economic Review*, 95(1), 161–182.

- Newey, W. K., & West, K. D. (1987). A simple, positive semi-definite, heteroskedasticity and autocorrelation consistent covariance matrix. *Econometrica*, 55(3), 703–708.
