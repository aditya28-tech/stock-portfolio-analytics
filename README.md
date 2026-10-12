# Stock Portfolio Analytics: Performance, Risk \& Construction

!\[Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python\&logoColor=white)
!\[Power BI](https://img.shields.io/badge/Power%20BI-Report-F2C811?logo=powerbi\&logoColor=black)
!\[pandas](https://img.shields.io/badge/pandas-NumPy%20%7C%20SciPy-150458?logo=pandas\&logoColor=white)

An end-to-end portfolio analytics project that tracks a ₹10 lakh investment in 8 large-cap Indian stocks over 3 years, measures it against a Nifty 50 benchmark, and explains **why** it performed the way it did using institutional-style risk and portfolio construction analysis.

> \*\*Disclaimer:\*\* This is a sample portfolio built for learning purposes only. Nothing in this repository is investment advice.

\---

## The Question

> \*Does holding a "diversified" basket of well-known blue-chip stocks beat simply buying the index?\*

**Short answer:** Not in this case. The portfolio returned **0.2%** over 3 years while a Nifty 50 ETF returned **18.0%**. The rest of this project is about finding out why.

\---

## Dashboard Preview

### Page 1: Performance Overview

!\[Performance Overview](images/page1\_performance.png)

### Page 2: Risk Analytics

!\[Risk Analytics](images/page2\_risk.png)

### Page 3: Construction \& Outlook

!\[Construction \& Outlook](images/page3\_construction.png)

\---

## Key Findings

|Metric|Portfolio (Buy \& Hold)|Quarterly Rebalanced|Nifty 50 ETF|
|-|-|-|-|
|Total Return|0.2%|2.1%|18.0%|
|CAGR|0.1%|0.7%|5.7%|
|Volatility (annual)|13.5%|13.6%|12.1%|
|Sharpe Ratio|−0.39|−0.34|0.01|
|Sortino Ratio|−0.55|−0.48|0.01|
|Max Drawdown|−23.4%|−23.7%|−15.2%|
|Beta|1.00|0.99|—|
|Alpha (annual)|−5.4%|−4.8%|—|
|Downside / Upside Capture|104% / 97%|103% / 97%|—|

*Period: 13 Oct 2023 to 09 Oct 2026. Initial investment: ₹10,00,000. Final value (buy \& hold): ₹10,02,300.*

### 1\. The portfolio caught the market's falls, but not its rises

Downside capture was **104%** while upside capture was only **97%**. On bad days the portfolio fell slightly more than the market, and on good days it rose slightly less. Repeated over 3 years, this small asymmetry produced a **17.8 percentage point** shortfall against the benchmark.

### 2\. Two stocks were really one bet

TCS and Infosys had a return correlation of **0.74**, by far the highest pair in the portfolio. The 25% allocation to IT behaved like a single concentrated position rather than two independent holdings, and these were the two largest drags on return (TCS ≈ −5.1 points, Infosys ≈ −2.1 points). The IT sector's share of the portfolio shrank from 25% to 17.8%.

### 3\. Eight stocks carried more risk than fifty

With a beta of **1.00**, the portfolio took on full market risk, plus extra stock-specific risk from holding only 8 companies. As a result, its volatility (13.5% vs 12.1%) and maximum drawdown (−23.4% vs −15.2%) were both worse than the index. A diversification ratio of **1.65** shows diversification did reduce risk compared with holding the stocks individually, but not as much as a 50-stock index.

### 4\. Return attribution: a few winners, more losers

|Stock|Contribution to Total Return|
|-|-|
|Sun Pharma|≈ +5.8 pts|
|Larsen \& Toubro|≈ +3.5 pts|
|Maruti Suzuki|≈ +1.0 pts|
|Reliance Industries|≈ +0.1 pts|
|HDFC Bank|≈ −0.6 pts|
|Infosys|≈ −2.1 pts|
|Hindustan Unilever|≈ −2.4 pts|
|TCS|≈ −5.1 pts|

Larsen \& Toubro was also a **risk hotspot**: its share of portfolio risk was noticeably larger than its share of capital.

### 5\. "Optimal" portfolios are built on hindsight

The maximum-Sharpe optimiser allocated **40% to Larsen \& Toubro, 40% to Sun Pharma and 20% to Maruti Suzuki**, and nothing to the other five stocks. These are exactly the three best past performers. The optimiser didn't find a hidden insight; it simply picked yesterday's winners, up to the 40% position limit. This is a clear, visual example of why optimisation on historical data often disappoints out of sample.

The minimum-risk portfolio, by contrast, tilted towards steadier, less correlated names such as Sun Pharma, Hindustan Unilever and HDFC Bank.

### 6\. Rebalancing helped slightly

Resetting to target weights every quarter returned **2.1% vs 0.2%** for buy-and-hold, even after a 0.2% transaction cost on every rupee traded, by systematically trimming winners and adding to laggards.

### 7\. The 1-year outlook is wide and uncertain

A 10,000-path Monte Carlo simulation (starting from ₹10,02,300) produced:

|Outcome|Value in 1 Year|Change|
|-|-|-|
|Bad case (5th percentile)|₹8,25,340|≈ −17.7%|
|Median|₹10,30,930|≈ +2.9%|
|Good case (95th percentile)|₹12,84,180|≈ +28.1%|

There is a **42% chance of losing money** and only a **40% chance of beating a 6.5% government bond**. Because the simulation resamples the last 3 years of returns, this reads as *"if the next year resembles the last three"*, not as a forecast.

### Context

Even the Nifty 50 ETF's CAGR (5.7%) was below the 6.5% risk-free rate, giving it a Sharpe ratio of roughly zero. This was a difficult period for Indian large caps in general, which the portfolio's concentration then amplified.

\---

## Methodology

### Portfolio

|Stock|Sector|Weight|
|-|-|-|
|HDFC Bank|Banking|15%|
|TCS|IT|15%|
|Reliance Industries|Energy|15%|
|Larsen \& Toubro|Infrastructure|15%|
|Sun Pharma|Pharma|10%|
|Hindustan Unilever|FMCG|10%|
|Maruti Suzuki|Auto|10%|
|Infosys|IT|10%|

### Data and benchmark

* **Source:** Yahoo Finance daily closing prices via `yfinance`, adjusted for splits and dividends.
* **Benchmark:** `NIFTYBEES.NS`, a Nifty 50 ETF that reinvests dividends. The price index (`^NSEI`) excludes dividends, while adjusted stock prices include them, so comparing against the price index would unfairly favour the portfolio. Using an investable, dividend-inclusive benchmark keeps the comparison fair.
* **Data checks:** forward-filling of market holidays, and an automatic flag for any one-day move above 25% (a typical sign of an unadjusted split or demerger).

### Strategies

* **Buy \& hold:** invest at target weights on day one and never trade.
* **Quarterly rebalanced:** reset to target weights on the first trading day of each quarter, with a 0.2% cost on the value traded.

### Metrics

|Metric|Definition|
|-|-|
|CAGR|$(V\_{end}/V\_{start})^{1/years} - 1$|
|Volatility|Std. dev. of daily returns × $\\sqrt{252}$|
|Sharpe Ratio|Mean daily excess return × 252 ÷ annual volatility|
|Sortino Ratio|Mean daily excess return × 252 ÷ annual downside deviation|
|Max Drawdown|Largest fall from a previous peak|
|Calmar Ratio|CAGR ÷ \|Max Drawdown\||
|VaR (95%, 1-day)|5th percentile of daily returns (historical)|
|CVaR (95%, 1-day)|Average return on days at or below the VaR|
|Beta|Cov(portfolio, benchmark) ÷ Var(benchmark)|
|Alpha|Annualised excess return not explained by beta (Jensen's alpha)|
|Tracking Error|Annualised std. dev. of (portfolio − benchmark) returns|
|Information Ratio|Annualised active return ÷ tracking error|
|Up / Down Capture|Average portfolio return on benchmark up / down days ÷ benchmark's average|

Risk-free rate: 6.5% per year (approximate Indian 10-year government bond yield), converted to a daily rate.

### Advanced analysis

* **Rolling metrics:** 1-year (252-day) rolling return, volatility, Sharpe ratio and beta.
* **Risk contribution:** each stock's share of portfolio volatility, calculated as $w\_i (\\Sigma w)\_i / \\sigma\_p$, compared against its capital weight.
* **Correlation matrix** and **diversification ratio** (weighted average stand-alone volatility ÷ portfolio volatility).
* **Return attribution:** each holding's rupee profit or loss as a share of the initial investment. The contributions sum exactly to the portfolio's total return.
* **Efficient frontier:** constrained mean-variance optimisation with SciPy (SLSQP): long-only, fully invested, maximum 40% per stock. Includes 3,000 random portfolios, the frontier, and the minimum-risk and maximum-Sharpe portfolios.
* **Monte Carlo simulation:** 10,000 one-year paths created by bootstrapping (resampling with replacement) historical daily portfolio returns at current weights, which preserves real fat tails rather than assuming a normal distribution.

\---

## Dashboard Design (Power BI)

* **Star-schema data model** with a dedicated `Calendar` table and a stock dimension table, so slicers filter consistently across visuals.
* **Dedicated `\_Measures` table** for all DAX measures, including date-aware measures that recalculate returns for any period selected in the slicer.
* **Custom theme** (`portfolio\_theme.json`) for consistent colours: navy for the portfolio, grey for the benchmark, and green and red for gains and losses.
* **Diverging colour scales** centred at zero for the monthly returns heatmap and correlation matrix.
* **Methodology panel** in the report itself, so every assumption is visible to the reader.

|Page|Purpose|Key visuals|
|-|-|-|
|Performance Overview|How did it perform, and where did returns come from?|KPI cards, growth vs benchmark, monthly heatmap, return attribution, sector allocation|
|Risk Analytics|How risky was it, and where did the risk come from?|Drawdown chart, rolling volatility, share of money vs share of risk, correlation heatmap, full scorecard|
|Construction \& Outlook|Could a better mix have done better, and what might happen next?|Efficient frontier, optimal weights, Monte Carlo fan chart, outcome distribution, assumptions|

\---

## Tech Stack

* **Python:** pandas, NumPy, SciPy, yfinance, Matplotlib (Google Colab)
* **Power BI Desktop:** Power Query, DAX, data modelling, conditional formatting

\---

## Repository Structure

```
stock-portfolio-analytics/
├── README.md
├── notebook/
│   └── Stock\_Portfolio\_Dashboard.ipynb    # Data pipeline, metrics and models
├── powerbi/
│   ├── Stock\_Portfolio\_Analytics.pbix     # 3-page Power BI report
│   └── portfolio\_theme.json               # Custom report theme
├── data/                                   # 15 CSV files exported for Power BI
│   ├── portfolio\_timeseries.csv
│   ├── stock\_summary.csv
│   ├── kpi\_summary.csv
│   └── ...
└── images/
    ├── page1\_performance.png
    ├── page2\_risk.png
    └── page3\_construction.png
```

\---

## How to Reproduce

1. Open `notebook/Stock\_Portfolio\_Dashboard.ipynb` in [Google Colab](https://colab.research.google.com).
2. Run all cells in order. The final cell downloads `powerbi\_data\_pro.zip` containing 15 CSV files.
3. Extract the CSVs, open `powerbi/Stock\_Portfolio\_Analytics.pbix` in Power BI Desktop, and update the data source path via **Transform Data → Data source settings**.
4. Click **Refresh**.

To analyse a different portfolio, edit the `PORTFOLIO` dictionary in the settings cell (weights must sum to 1.0) and re-run the notebook. Results will change with the run date, because the analysis always uses the most recent 3 years of data.

\---

## Limitations

* **Hindsight bias:** the efficient frontier and optimal weights use the same historical data they are evaluated on. They show what *would have* worked, not what *will* work.
* **Monte Carlo assumes the future resembles the past 3 years**, which were weak for this portfolio.
* **Expected returns on the frontier** use the arithmetic mean of daily returns × 252, so they differ from realised CAGR because of volatility drag.
* **Not modelled:** taxes, cash drag, dividend timing, intraday prices and changes in the risk-free rate over time.
* **Small sample:** 8 stocks over 3 years is a single, specific period and shouldn't be generalised.

\---

## What I Learned

* Diversification is about **how differently assets move**, not how many you own. Two highly correlated stocks offer little more protection than one.
* **Capital weight is not the same as risk weight.** A stock can hold 15% of the money but drive a much larger share of the risk.
* Small asymmetries, like capturing 104% of the downside but only 97% of the upside, **compound into large performance gaps** over time.
* Backward-looking optimisation is a useful diagnostic, but a **poor forecasting tool**.
* Choosing a **fair benchmark** (dividend-inclusive and investable) is as important as the analysis itself.

\---

## Author

**Aditya Kumar Tiwari**
[LinkedIn](www.linkedin.com/in/aditya-tiwari-3a947a25b) · aditya28sbh@gmail.com

*Feedback and suggestions are welcome. Feel free to open an issue.*

