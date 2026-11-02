# Central bank interest rates since 2008

Every interest rate decision of ten central banks since 2008, as CSV and JSON, updated automatically after every decision.

| Bank | Code | Rate |
| --- | --- | --- |
| Federal Reserve | `fed` | federal funds rate (target range: the file gives its top) |
| European Central Bank | `ecb` | deposit facility rate (blank before March 2015: see the notes) |
| Bank of England | `boe` | Bank Rate |
| Bank of Japan | `boj` | policy rate |
| Bank of Canada | `boc` | overnight rate target |
| Reserve Bank of Australia | `rba` | cash rate target |
| Reserve Bank of New Zealand | `rbnz` | official cash rate |
| Swiss National Bank | `snb` | SNB policy rate |
| Norges Bank | `norges` | policy rate |
| Sveriges Riksbank | `riksbank` | policy rate |

Browse it with charts, meeting dates and what each decision did to the currency at **[InterestRateDecision.com](https://interestratedecision.com/)**.

## Files

| File | What it holds |
| --- | --- |
| [`data/decisions.csv`](data/decisions.csv) | Every decision since 2008: `bank`, `bank_name`, `time_utc`, `date`, `rate` (per cent, after the decision), `change_bp` (basis points against the decision before) |
| [`data/rates.json`](data/rates.json) | Each bank's rate today, its last change and its next decision |
| [`data/next.json`](data/next.json) | The decisions to come, with their times in UTC |

The same files are served live at `https://interestratedecision.com/data/decisions.csv`, `/data/rates.json` and `/data/next.json`. This repository copies them every six hours (`.github/workflows/sync.yml`), so its history is a record of every change.

### Quick start

```python
import pandas as pd
df = pd.read_csv("https://raw.githubusercontent.com/InterestRateDecision/central-bank-interest-rates/main/data/decisions.csv", parse_dates=["time_utc"])
df[df.bank == "fed"].set_index("time_utc").rate.plot(drawstyle="steps-post")
```

```r
d <- read.csv("https://raw.githubusercontent.com/InterestRateDecision/central-bank-interest-rates/main/data/decisions.csv")
```

```js
const rates = await (await fetch("https://interestratedecision.com/data/rates.json")).json();
```

## Notes

- Times are the announcement in UTC. The Bank of Japan sets no fixed hour; its times are the calendar's estimate.
- A few meetings are missing from the source calendar. Where a missed one changed the rate, the next row's `change_bp` is against the last row listed, so a jump can be two decisions.
- Before March 2015 the source calendar gives the ECB's deposit rate only on the day a change took effect, so the ECB's `rate` is blank on those rows and `change_bp` is the change in its main refinancing rate.
- Economists' consensus is not in these files: it belongs to the calendar it comes from.
- Errors of fact are listed at [interestratedecision.com/corrections/](https://interestratedecision.com/corrections/). Found one? Open an issue.

## Licence

Free to use in anything, including commercial work, **with a visible link to [InterestRateDecision.com](https://interestratedecision.com/)**. Rates and dates are public facts; the compilation is InterestRateDecision.com's. Full terms: [interestratedecision.com/central-bank-data/](https://interestratedecision.com/central-bank-data/#licence).

Suggested citation: *InterestRateDecision.com, "Central bank interest rates since 2008", https://interestratedecision.com/central-bank-data/*
