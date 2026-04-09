# Thailand Oil Price Shock → Inflation Pass-Through Model

Estimating how oil price shocks transmit to Thai headline CPI inflation using Local Projections.

## Overview

This project quantifies the pass-through of global oil price movements to Thailand's consumer price index. Using quarterly data from 2017–2025, I estimate impulse response functions that show how a 1% increase in Brent crude prices affects Thai YoY inflation over the following 8 quarters.

## Key Findings

| Horizon | Impact (pp per 1% oil shock) | Significant? |
|---------|------------------------------|--------------|
| Q1 | +0.072 | ✓ |
| Q2 | +0.054 | ✓ |
| Q3 | +0.077 | ✓ |
| Q4 | +0.017 | — |
| Q5 | +0.038 | ✓ |
| Q6 | +0.038 | ✓ |
| Q7 | −0.005 | — |
| Q8 | −0.019 | ✓ |
![Impulse Response Function](results/irf_plot.png)
**Interpretation:** A 10% oil price increase raises Thai inflation by approximately 0.7–0.8 percentage points in the first 3 quarters. The effect then fades and slightly reverses by Q8, consistent with partial and transitory pass-through found in the literature.

## Methodology

- **Model:** Local Projections (Jordà, 2005)
- **Shock variable:** Quarterly log change in Brent crude price (× 100 = % change)
- **Outcome:** Thailand headline CPI YoY inflation
- **Specification:** CPI YoY at horizon *h* regressed on contemporaneous oil shock, 4 lags of CPI YoY, and 4 lags of oil shocks
- **Inference:** HAC (Newey–West) standard errors with maxlags = *h* + 1
- **Scenarios:** Linear scaling of β(*h*) for +10%, +20%, +30% oil shock scenarios

> This is a reduced-form historical pass-through estimate, not a structural forecast.

## Repository Structure

```
thai-inflation-oil-shock-model/
├── README.md
├── requirements.txt
├── notebooks/
│   └── analysis.ipynb          # Main analysis notebook
├── data/
│   ├── brent_quarterly.xlsx    # Brent crude oil prices (quarterly)
│   └── thai_cpi_quarterly.xlsx # Thailand CPI index (quarterly)
├── results/
│   ├── irf_oil_to_cpi_quarterly.csv        # Impulse response estimates
│   └── scenario_impacts_oil_to_cpi_quarterly.csv  # Scenario analysis
└── docs/
    └── methodology.md          # Detailed methodology explanation
```

## Data Sources

- **Brent Crude Oil Prices:** [EIA / World Bank / FRED]
- **Thailand CPI Index:** Bank of Thailand

## Quick Start

```bash
# Clone the repository
git clone https://github.com/[username]/thai-inflation-oil-shock-model.git
cd thai-inflation-oil-shock-model

# Install dependencies
pip install -r requirements.txt

# Run the analysis
jupyter notebook notebooks/analysis.ipynb
```

## Requirements

- Python 3.8+
- pandas
- numpy
- statsmodels
- matplotlib
- openpyxl

## Scenario Analysis

For a **20% oil price shock** (e.g., geopolitical crisis):

| Quarter | CPI Impact (pp) | 95% CI |
|---------|-----------------|--------|
| Q1 | +1.43 | [0.85, 2.01] |
| Q2 | +1.08 | [0.47, 1.69] |
| Q3 | +1.54 | [0.88, 2.20] |
| Q4 | +0.33 | [−0.24, 0.90] |

## License

MIT

## Author

[Your Name]

## References

- Jordà, Ò. (2005). Estimation and inference of impulse responses by local projections. *American Economic Review*, 95(1), 161–182.
