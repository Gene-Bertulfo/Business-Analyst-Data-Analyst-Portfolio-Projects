# AutoNation Demand Analysis: Macro Indicators, Supply Constraints, and Inventory Strategy (2019–2025)

## Goal
AutoNation (AN) is used as a bellwether for consumer durables spending. This analysis tests whether macroeconomic and inventory indicators can predict shifts in AN's new-vehicle and used-vehicle unit sales and, where a real relationship exists, translates it into an operational recommendation.

## Description & Details

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

All data covers 2019–2025. Quarterly AN unit sales were derived directly from Q1–Q3 10-Q disclosures; Q4 figures were derived as (annual 10-K total − sum of Q1–Q3).

### Timeline of Findings

**2020 (Q1–Q2): Pandemic shock.** Every indicator moves simultaneously: sentiment craters, unemployment spikes, disposable income and savings rate both spike (driven by stimulus payments, not spending behavior), industry-wide sales crash. AN's own units dip modestly and recover quickly. This period is treated as an anomaly throughout the analysis, since it can distort correlation results if not isolated.

**2021–2022: Supply-constrained substitution.** This period initially looked like a sentiment-driven shift toward used vehicles (sentiment was falling; used units were rising). Three independent pieces of evidence instead point to a supply-side explanation:  
- Total industry sales (not just AN) dropped sharply, an industry-wide event consistent with the chip shortage, not a demand-side story specific to AN's customers.
- AN's own new-vehicle days-supply fell to roughly 10–15 days, a direct measurement of inventory scarcity.
- New-vehicle CPI rose while new units fell (the signature of a supply-constrained price spike); used-vehicle CPI rose and used units rose simultaneously (the signature of a demand-pull surge as displaced new-car buyers competed for used inventory).

AN's own units held up better than the industry average during this window, suggesting the used-vehicle pivot helped offset the new-vehicle shortage better than competitors managed.  
Separately, unemployment fell to historic lows during this same window while sentiment kept declining, a divergence attributable to inflation eroding confidence even as the labor market stayed strong.

## Results

## Tech Stack

## Skills Demonstrated

## In-depth tech details of the project

