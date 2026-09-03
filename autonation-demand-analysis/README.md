# AutoNation Demand Analysis: Macro Indicators, Supply Constraints, and Inventory Strategy (2019-2025)

## Goal
AutoNation (AN) is used as a bellwether for consumer durables spending. This analysis tests whether macroeconomic and inventory indicators can predict shifts in AN's new-vehicle and used-vehicle unit sales and, where a real relationship exists, translates it into an operational recommendation.

## Details

### Data Sources

| Series | Source | Frequency |
|---|---|---|
| New/used vehicle units sold | AutoNation 10-Q/10-K filings (SEC EDGAR) | Quarterly |
| New/used vehicle inventory days supply | AutoNation 10-Q/10-K filings (SEC EDGAR) | Quarterly |
| Consumer sentiment (UMCSENT) | FRED | Quarterly (avg) |
| Unemployment rate (UNRATE) | FRED | Quarterly (avg) |
| Real disposable personal income (DSPIC96) | FRED | Quarterly (avg) |
| Personal saving rate (PSAVERT) | FRED | Quarterly (avg) |
| CPI: new vehicles (CUSR0000SETA01) | FRED | Quarterly (avg) |
| CPI: used vehicles (CUSR0000SETA02) | FRED | Quarterly (avg) |
| Total industry vehicle sales, SAAR (TOTALSA) | FRED | Quarterly (avg) |

All data covers 2019-2025. Quarterly AN unit sales were derived directly from Q1–Q3 10-Q disclosures; Q4 figures were derived as (annual 10-K total − sum of Q1-Q3).

### Timeline of Findings

**2020 (Q1-Q2): Pandemic shock.** Every indicator moves simultaneously: sentiment craters, unemployment spikes, disposable income and savings rate both spike (driven by stimulus payments, not spending behavior), industry-wide sales crash. AN's own units dip modestly and recover quickly. This period is treated as an anomaly throughout the analysis, since it can distort correlation results if not isolated.

**2021-2022: Supply-constrained substitution.** This period initially looked like a sentiment-driven shift toward used vehicles (sentiment was falling; used units were rising). Three independent pieces of evidence instead point to a supply-side explanation:  
- Total industry sales (not just AN) dropped sharply, an industry-wide event consistent with the chip shortage, not a demand-side story specific to AN's customers.
- AN's own new-vehicle days-supply fell to roughly 10–15 days, a direct measurement of inventory scarcity.
- New-vehicle CPI rose while new units fell (the signature of a supply-constrained price spike); used-vehicle CPI rose and used units rose simultaneously (the signature of a demand-pull surge as displaced new-car buyers competed for used inventory).

AN's own units held up better than the industry average during this window, suggesting the used-vehicle pivot helped offset the new-vehicle shortage better than competitors managed.  
Separately, unemployment fell to historic lows during this same window while sentiment kept declining, a divergence attributable to inflation eroding confidence even as the labor market stayed strong.

**2022-2023: Normalization.** Sentiment recovers off its floor; new-vehicle days-supply and CPI both ease; new-unit sales climb back from 2023-Q2 onward; used-vehicle CPI cools from its peak as rental-fleet resales and eased chip supply relieve the used market.

**2024-2025: A different kind of softening.** Sentiment declines again from 2024-Q2, but this time unemployment is also drifting upward, rather than staying strong as it did in 2021-2022. This is the first time since the pandemic shock that these two indicators have moved together. New-vehicle days-supply also rises again (to ~75-80 days by 2024-Q3), but the mechanism looks different from 2021-2022: inventory building up against softening demand, rather than scarcity.

### What Was Tested and Did Not Hold Up
The original thesis was that consumer-health indicators (sentiment, unemployment, income, savings rate) would show a multi-quarter leading relationship to AN's units sold, giving AN advance warning of demand shifts.  
Full-period correlation (2019–2025) appeared to support this: unemployment and savings rate showed their strongest relationship to used-unit sales at a 4-quarter lag (r ≈ 0.69 and 0.67, respectively).  
This result did not survive testing. Re-running the same calculation on 2022-2025 only (excluding the pandemic period) collapsed both relationships to weak, contemporaneous correlations (r ≈ -0.51 at lag 0, weakening, not strengthening, with lag). The original 4-quarter-lag finding was very likely a pandemic-specific artifact: the 2020-2021 stimulus-driven savings spike happened to precede the 2021-2022 used-vehicle surge by about a year, which produced a correlation that does not represent a repeatable pattern.  
**Conclusion:** consumer sentiment and unemployment do not provide a reliable multi-quarter early warning for AN's demand shifts outside of the 2020–2021 anomaly. This is a real finding, not a null result, it rules out an entire class of forecasting approach for this business.

### What Held Up: The Supply-Side Signal
The same out-of-sample test (2022-2025 only) was applied to the inventory-based relationships, since AN's own days-supply data is something the company measures and controls directly:
- new_days_supply → used_units: remained strongly negative (r = -0.738 at lag 0, -0.687 at lag 1), the new-to-used substitution mechanism holds outside the pandemic window, not just during the chip shortage.
- new_days_supply → new_units: positive and strengthening with lag (r = 0.508 at lag 0 → 0.611 at lag 1 → 0.663 at lag 2), a genuine ~1-2 quarter leading relationship. The likely mechanism: rising new-vehicle inventory triggers discounting/promotional response, which lifts new-unit sales 1–2 quarters later.

**Limitations on this finding:** the sample size is small (14-18 quarterly observations for these lags), so treat the exact correlation magnitudes as directional rather than precise. There is also a plausible alternative explanation that cannot be ruled out with correlation alone: days-supply is partly calculated from recent sales volume (inventory ÷ sales rate), so a temporarily weak sales quarter can mechanically inflate days-supply, and a subsequent reversion to normal sales could produce the same lagged pattern without any real discounting behavior occurring. This is flagged as an open limitation, not resolved by the current dataset.

## Results / Recommendation
Consumer sentiment and unemployment should not be used as AN's primary early-warning system for demand shifts, the data does not support a reliable lead time from these indicators outside of the unique 2020-2021 stimulus period.  

Instead, new-vehicle inventory days-supply is a more defensible, real-time signal AN already has direct visibility into and control over. The data supports two specific, actionable relationships:
- **When new-vehicle days-supply rises** (inventory building up faster than it's selling), expect and prepare for a lift in new-vehicle unit sales roughly 1-2 quarters out, this is the window to plan discounting/promotional pushes to clear that inventory, since the data shows this pattern already happens (likely via dealer-initiated discounting), rather than waiting reactively.
- **When new-vehicle days-supply falls** (inventory tightening), expect increased used-vehicle demand in that same quarter, AN should proactively shift inventory allocation, marketing spend, and F&I focus toward used vehicles at that point, rather than treating the demand shift as a surprise after it's already happened.  

One important qualifier baked into the recommendation: **don't apply this automatically to the current 2024-2025 environment.** Days-supply is rising right now too, but for a different reason than 2021-2022, back then it was scarcity (chip shortage) forcing the shift to used; right now it looks more like inventory piling up because demand is genuinely softening (consumer sentiment declining while unemployment gradually climbs, a combination not seen since the pandemic). The trigger metric is the same, but the recommended response isn't automatically "lean into used" this time, that call should be re-evaluated against whether this is a supply story or a demand story, since the data suggests it's likely the latter now.

## Tech Stack
Python, BigQuery, Power BI

## Skills Demonstrated

## Tech Showcase

### Pull the full filing list (10-Q and 10-K)

```python
def get_filings(CIK_PADDED, HEADERS):
  url = f"https://data.sec.gov/submissions/CIK{CIK_PADDED}.json"
  data = requests.get(url, headers=HEADERS).json()
  recent = data["filings"]["recent"]
  df = pd.DataFrame({
      "form": recent["form"],
      "accessionNumber": recent["accessionNumber"],
      "filingDate": recent["filingDate"],
      "reportDate": recent["reportDate"],
      "primaryDocument": recent["primaryDocument"]
  })
  return df[df["form"].isin(["10-Q", "10-K"])].reset_index(drop=True)

filings = get_filings(CIK_PADDED, HEADERS)
filings = filings[filings["reportDate"] >= "2019-01-01"]   # the project window
print(len(filings), "filings found")
filings.head()
```

### Parse "Retail vehicle unit sales" out of each filing

```python
def extract_units(url, headers):
    r = requests.get(url, headers=headers)
    soup = BeautifulSoup(r.text, "html.parser")
    new_units, used_units = None, None
    for table in soup.find_all("table"):
        in_section = False
        for row in table.find_all("tr"):
            cells = [c.get_text(strip=True) for c in row.find_all(["td", "th"])]
            label = " ".join(cells).strip().lower()

            if "retail vehicle unit sales" in label:
                in_section = True
                continue
```

### Forward to BigQuery

```python
table_id = f"{PROJECT_ID}.{DATASET_ID}.units_sold_raw"
job = client.load_table_from_dataframe(
    units_df, table_id,
    job_config=bigquery.LoadJobConfig(write_disposition="WRITE_TRUNCATE")
)
job.result()
print("Loaded", job.output_rows, "rows to", table_id)
```

### Extract Data from FRED with API Key

```python
def get_fred_series(series_id, api_key):
    url = "https://api.stlouisfed.org/fred/series/observations"
    params = {
        "series_id": series_id,
        "api_key": api_key,
        "file_type": "json",
        "frequency": "q",
        "aggregation_method": "avg",
        "observation_start": "2019-01-01",
    }
    r = requests.get(url, params=params)
    r.raise_for_status()
    data = r.json()["observations"]
    df = pd.DataFrame(data)[["date", "value"]]
    df["value"] = pd.to_numeric(df["value"], errors="coerce")
    return df

fred_frames = []
for series_id, label in SERIES.items():
    df = get_fred_series(series_id, FRED_API_KEY)
    df["series"] = label
    df["series_id"] = series_id
    fred_frames.append(df)

fred_df = pd.concat(fred_frames, ignore_index=True)
fred_df.rename(columns={"date": "reportDate"}, inplace=True)
fred_df["reportDate"] = pd.to_datetime(fred_df["reportDate"]).dt.date

fred_df
```

### Lag/cross-correlation Calculation

```python
results = []
for indicator in indicators:
    for target in targets:
        for lag in range(0, 5):
            shifted = df[indicator].shift(lag)
            valid = df[[target]].join(shifted.rename("shifted")).dropna()
            if len(valid) > 5:
                corr = valid[target].corr(valid["shifted"])
                results.append({
                    "indicator": indicator,
                    "target": target,
                    "lag_quarters": lag,
                    "correlation": round(corr, 3),
                    "n": len(valid)
                })

lag_results = pd.DataFrame(results)

best_lags = lag_results.loc[lag_results.groupby(["indicator", "target"])["correlation"].apply(lambda x: x.abs().idxmax())]
best_lags = best_lags.sort_values("correlation", key=abs, ascending=False).reset_index(drop=True)
best_lags
```

### Caveat: each phase has ~half the data, so treat this as even more directional than the full-period result

```python
for phase_name, phase_df in df.groupby("phase"):
    print(f"\n=== {phase_name} (n={len(phase_df)}) ===")
    phase_results = []
    for indicator in indicators:
        for target in targets:
            for lag in range(0, 3):  # fewer lags given smaller n per phase
                shifted = phase_df[indicator].shift(lag)
                valid = phase_df[[target]].join(shifted.rename("shifted")).dropna()
                if len(valid) > 4:
                    corr = valid[target].corr(valid["shifted"])
                    phase_results.append({"indicator": indicator, "target": target, "lag": lag, "corr": round(corr, 3), "n": len(valid)})
    print(pd.DataFrame(phase_results).sort_values("corr", key=abs, ascending=False).head(10))
```
