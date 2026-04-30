# SWP Estimator

A single-file, zero-dependency web app to simulate **Systematic Withdrawal Plans (SWP)** from a mutual fund corpus. Enter your parameters and instantly see how long your corpus lasts, month-by-month breakdowns, and interactive charts — all running locally in your browser.

## Features

- **Corpus depletion simulation** — calculates exactly how many years and months until the corpus is exhausted
- **LTCG tax modelling** — tax is applied on the gain fraction of each withdrawal, not the full amount
- **Inflation-adjusted withdrawals** — monthly withdrawal amount increases every 12 months by the specified annual inflation rate
- **SWP deferral** — set a growth-only phase (years + months) before withdrawals begin; corpus compounds untouched during this period
- **Target duration solver** — set the number of years/months you want the corpus to last; the app auto-calculates the required monthly withdrawal via binary search
- **Interactive chart** — line chart showing Closing Corpus, Cumulative Gains, and Cumulative Tax Paid over time (Chart.js)
- **Stale results indicator** — button pulses amber/red and a banner appears when inputs are changed after calculation
- **CSV export** — download the full monthly table as a CSV file
- **Monthly Return Rate** — auto-derived from Annual Return as `(1 + r)^(1/12) − 1`, displayed read-only

## Inputs

| Field | Description | Default |
|---|---|---|
| Initial Corpus | Starting fund value | ₹1,00,00,000 |
| Monthly Withdrawal | Amount withdrawn each month (or auto-calculated) | ₹1,00,000 |
| Annual Return | Expected annual return of the fund | 12% |
| Monthly Return Rate | Derived automatically, read-only | 0.9489% |
| Tax Rate | Tax on gains component of each withdrawal | 12.5% |
| Annual Inflation | Rate at which withdrawal increases each year | 6% |
| SWP Starts After | Deferral period before withdrawals begin | 0Y 1M |
| Target: Corpus Exhausted In | Optional — auto-sets withdrawal for a desired duration | — |

## Calculation Logic

Each month in the **SWP phase**:

```
Monthly Gain (E)  = Opening Balance (B) × Monthly Return Rate
Total             = B + E
Withdrawal (C)    = min(target withdrawal, Total)
Gain Fraction     = E / Total
Tax Outgo (D)     = C × Gain Fraction × Tax Rate
Closing Bal (F)   = Total − C − D
```

During the **deferral phase**, the corpus grows at the monthly rate with no withdrawal or tax.

Inflation is applied at the start of each new SWP year (every 12 SWP months).

## Usage

Open `index.html` directly in any modern browser — no server or build step needed.

```bash
open index.html
```

## Tech Stack

- Pure HTML / CSS / JavaScript (vanilla, no framework)
- [Chart.js 4.4](https://www.chartjs.org/) (loaded from CDN)
