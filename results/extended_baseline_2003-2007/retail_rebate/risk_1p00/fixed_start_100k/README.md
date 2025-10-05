# Euro Macromechanica (EMM) M5 Engine — Extended Baseline (2003–2007) — Retail Rebate (5 USD/lot, risk 1%) – Fixed Start 100k

<p align="center">Balance Curve — Fixed Start 100k Mode (Risk 1%, $5 round-turn per standard lot, M5 EMM cost model v1.0) 2003–2007</p>

<p align="center"><img src="../../../../../docs/assets/2003-2007_5usd_balance_curve_fix.png" alt="EMM M5 — Retail Rebate — Extended Baseline 5 USD/lot — Balance Curve" width="100%"></p>

## 🧾 Track Description

This track reports backtest results for the M5 EMM strategy under **Retail Rebate** transaction costs: **5 USD per round‑turn per 1 standard lot (100 000 EUR)**, equivalent to **≈0.5 pips** on EURUSD, with a **dynamic cost model (spread & slippage) M5 EMM cost model v1.0**. Capitalization mode — **annual reset to 100 000 USD**. Per‑trade risk — **1% of balance at entry**.

- Data range: **Extended Baseline 2003-01 – 2007-12** (coverage: **60 months without gaps = 5 years**)
- Instrument/TF: **EURUSD**, signal logic on **M5**
- **Backtest time zone:** **UTC+0** (all timestamps in UTC+0)
- Cost model: commission, spread, and slippage **included** in PnL
- Base NAV for rebasing: **100 000 USD** (`fixed_start_100k` — annual reset to 100k)

---

## 📈 Year-End Balance `fixed_start_100k`

| Year | balance at year-end (UTC+0) | year-end percentage (rounded to 5 decimals) |
|---|---:|---:|
| 2003 | 108452.10914 | +8.45211% |
| 2004 | 107933.27301 | +7.93327% |
| 2005 | 96409.04960 | -3.59095% |
| 2006 | 103323.74153 | +3.32374% |
| 2007 | 100279.43279 | +0.27943% |

### Result over 5 years ~ +16,397.60 USD / +16.40%

---

## 🧾 Cost Model

- **Commission:** 5 USD per round‑turn per 1 standard lot (100k EUR)  
- **Cost model (commission, spread, slippage) M5 EMM cost model v1.0** — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv).
- All costs are **included** in PnL.

> Details of the dynamic cost model are provided in [`Euro Macromechanica (EMM) Backtest — Overview and Methodology`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)

---

## 📊 Summary — Retail Rebate 5 USD/lot, `fixed_start_100k`, risk 1%

### Full period summary
- **CAGR 3.18%** with annual volatility **3.81%**; risk–return profile — **Sharpe 0.84**, **Sortino 1.44**, **MAR (Full period Calmar) 0.62**.
- Drawdowns (on the continuous curve): **EoM MaxDD −5.11%**; recovery time — **not recovered (n/a)**; deeper intramonth (**−7.08%**), **TTR — not recovered (n/a)**. Time underwater: **EoM 38 months**, **Intramonth 38 months**.
- Monthly premium: mean/median month **0.27% / 0.22%**.
- Calendar stability: best year **2003 (8.45%)**, worst **2005 (−3.59%)**; “zero” months **4**.
- Sample size: coverage **5** years; number of trades: **342**.
- Stress markers: **EoM MaxDD ≈ −5.11%**, **Intramonth MaxDD ≈ −7.08%**; expectation benchmark — **average month ≈ 0.27%**.
**Summary.** The profile appears moderately stable — the equity curve rises evenly with low variability; drawdowns are shallow and manageable, although recovery of prior peaks may take time; the monthly “premium” is small but steady, and calendar‑year results avoid extremes.

### Trades summary
- Sample size: **342** trades; win rate **70.47%**.
- Profile quality: **Profit factor 1.22**, **Payoff 0.51** (avg win/|avg loss|).
- Per‑trade expectancy: **mean 0.05 R**, **median 0.28 R**.
- R‑distribution: **σ ≈ 0.56 R**, **min -1.02 R**, **max 0.57 R**.
- Average outcomes: **avg win 0.37 R**, **avg loss -0.73 R**.
> **Summary:** positive expected value with a high share of winning trades and payoff below 1 indicates a “frequent small wins vs. rarer larger losses” profile. Stability depends on tight loss control and disciplined execution.

### Yearly summary
- Coverage: **5** years (2003–2007). Mean/median calendar year: **3.28% / 3.32%**.
- Best year: **2003 (8.45%)**; worst year: **2005 (-3.59%)**.
- Drawdowns (within the year, from peak): **EoM -4.38% → -1.04%**, **Intramonth -5.20% → -2.21%**.
- Trading activity: total trades across years **342**; yearly averages — win rate **70.03%**, PF **1.22**.
> **Summary:** at the yearly horizon, results are moderate and even: the average calendar result is stable while within‑year drawdowns remain controlled; trading metrics support a picture of resilience without extremes.

### Monthly returns
- Coverage: **60** months (2003–2007). Mean/median month: **0.27% / 0.22%** (P10/P90: **−1.06% / 1.74%**).
- Symmetry: positive months **37**, negative **19**, zero **4**.
- Extremes: best month **2004-02 (3.00%)**, worst month **2005-07 (−2.84%)**.
- Runs by month: maximum winning streak — **10** consecutive months, maximum losing streak — **4** consecutive months; months with zero return interrupt any streak.
> **Summary:** a small, steady monthly premium; resilience is supported by a high share of positive months and the absence of long losing streaks.

### Cash flows (USD)
- Coverage: **4** events across 2003–2006.
- Cash flows: **deposits (cash‑in)** 3,590.95 (1 event), **withdrawals (cash‑out)** 19,709.12 (3 events) 
- **Profit for the last year** 279.43.
> **Summary:** net profit ~ **16,397.60**.

### Conclusion
Overall, the track shows steady growth with low variability: drawdowns are shallow, though recovery of prior peaks can be prolonged and time “underwater” can be extended; the monthly premium is small but stable, and yearly results avoid extremes. Trading statistics indicate a “frequent small wins with payoff below 1” model — positive expectancy is achieved via a high share of winning trades alongside disciplined risk management and loss control. Monthly dynamics are dominated by positive months without extended losing streaks. The rebasing ledger reflects a positive result — net profit was withdrawn, aligning with the overall picture of stable profile behavior.

### Full methodology and metric definitions in [`docs/metrics_methodology/metrics_schema.json`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json) / [`docs/metrics_methodology/metrics_schema.md`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md).

### Metrics files

```
metrics/
  monthly_returns.csv
  full_period_summary.csv
  rebasing_applied.csv
  yearly_summary.csv
  trades_full_period_summary.csv
```

### Metrics were computed based on non‑public files `trades_YYYY.csv` and `balance_YYYY.csv`

---

## 📎 Links

- **Euro Macromechanica (EMM) Backtest — Overview and Methodology**: repository root **[README.md](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)**
- Cost model (commission, spread, slippage) M5 EMM cost model v1.0 — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv)
- General information about the contents of `results`: **[results/README.md](https://github.com/euro-macromechanica-backtest/results/blob/main/results/README.md)**
- Inputs and provenance: **[INPUTS-PIN.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.md)** / **[INPUTS-PROVENANCE.md](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md)**
- Full audit guide: **[docs/AUDIT.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.md)**
- Data quality policy: **[data_quality_policy/policy_v1.0.md](https://github.com/euro-macromechanica-backtest/results/blob/main/data_quality_policy/policy_v1.0.md)**
- Metric calculation methodology: **[docs/metrics_methodology/metrics_schema.md](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md)** / **[docs/metrics_methodology/metrics_schema.json](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json)**
