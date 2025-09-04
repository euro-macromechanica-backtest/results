<p align="center">Equity Curve — Compounding Mode (Risk 1%, $4.5 round‑turn per standard lot) 2009–2025‑07</p>

<p align="center"><img src="./equity_4.5usd_risk_1p00.png" alt="EMM M5 — Retail Advanced 4.5 USD/lot — Equity Curve" width="100%"></p>

# Euro Macromechanica (EMM) M5 Engine — Retail Advanced (4.5 USD/lot, 1% risk)

## 🧾 Track Description

This track records backtest results of the M5 **EMM** strategy at retail costs using a commission rebate: **4.5 USD per round‑turn per 1 standard lot (100,000 EUR)**, equivalent to **≈0.45 pip** on EURUSD. Per‑trade risk — **1% of capital at entry**.

- Data span: **headline 2009–2025‑07** (coverage: **199 months = 16 years and 7 months**)
- Instrument/TF: **EURUSD**, signaling logic on **M5**
- **Backtest time zone:** **UTC+0** (all timestamps in UTC+0)
- Cost model: commission **included** in PnL; **slippage** in this track was **not modeled**
- Base NAV for rebasing: **100,000 USD** (equity starts from the **first closed trade**)

## 🧭 Sub‑tracks

- **compounding_eoy_soy_base_100k** — compounding over the full period (EoY). For monthly return calculations, equity is rebased: insert an anchor of **100k at time t₀−ε** (an instant before t₀).
- **fixed_start_100k** — annual reset to 100k (each calendar year evaluated separately). Cross‑series risk metrics over the entire curve for fixed are **not aggregated**.

---

## 📈 Capital Dynamics — Year-End Equity & Annual Change (UTC+0) (by year-end close) `**compounding_eoy_soy_base_100k**`

| Year | Year‑End Equity (UTC+0) | Change vs prior year |
|---:|---:|---:|
| 2009 | 114735.04762 | +14.73505% |
| 2010 | 133601.31085 | +16.44333% |
| 2011 | 127022.20280 | -4.92443% |
| 2012 | 125860.79330 | -0.91434% |
| 2013 | 122261.93381 | -2.85940% |
| 2014 | 131293.72134 | +7.38724% |
| 2015 | 177172.64410 | +34.94373% |
| 2016 | 198020.93468 | +11.76722% |
| 2017 | 212836.24783 | +7.48169% |
| 2018 | 216455.84763 | +1.70065% |
| 2019 | 219092.87031 | +1.21827% |
| 2020 | 230443.25590 | +5.18063% |
| 2021 | 230654.17205 | +0.09153% |
| 2022 | 230324.12380 | -0.14309% |
| 2023 | 233315.03391 | +1.29857% |
| 2024 | 240529.69215 | +3.09224% |
| 2025-07 | 258145.17790 | +7.32362% |

### Result over 16 years and 7 months — +158145.17790 USD / + 158.14518%

---

## 📊 Quick Overview — Retail Advanced 4.5 USD/lot (1% risk)

- **Coverage:** 199 months (**2009‑01…2025‑07**).
- **CAGR:** 5.89%  |  **Vol (ann.):** 7.78%
- **Sharpe (ann., rf=0):** 0.78  |  **Sortino (ann.):** 1.22
- **MaxDD:** −18.34%  |  **MAR:** 0.321  |  **UW (longest):** 41 months  |  **Time UW:** 69.35%
- **By month:** positive — 59.80%; best/worst month: 10.89% / −5.20%.
- **Trades:** 2,736 | **Hit rate:** 69.30% | **PF:** 1.154 | **Payoff:** 0.511.  
  Average winner **0.389R**, loser **−0.761R**; **Expectancy:** 0.036R (median 0.305R).

### Additional metrics (compounding)

- **Ulcer Index:** 0.062  |  **Martin Ratio:** 0.98.
- **Skewness / Excess Kurtosis:** 0.866 / 4.035.
- **Rolling 12m (last window 2025‑07‑31):** return 5.90%, Sharpe 0.97.
- **Rolling 36m (last window 2025‑07‑31):** ret_ann 3.65%, maxdd_36m −8.44%, Calmar 0.43.
- **DD quantiles (left tail, monthly curve):** p95: −15.65%, p99: −17.20%
- **Kelly (winsor 1/99):** f_opt 0.0985, f_half 0.0493, f_quarter 0.0246.
- **Monte‑Carlo (block bootstrap, B=5000, L=3):** CAGR p05/p50/p95 = 2.89% / 5.88% / 9.08%; MaxDD p50/p95 = −12.55% / −7.93%; Pr(CAGR<0) = 0.06%; Pr(MaxDD ≤ −30%) = 0.42%.

### Results fixed_start_100k (annual)

- **Positive years:** 13/16 years and 7 months (78.39%); best/worst: 2015 34.92% / 2011 −4.93%.

### Short comparison with 7 USD/lot (1% risk)

- **Return/risk:** 0.45 is higher on **CAGR** (5.89% vs 4.37%) and **Sharpe** (0.78 vs 0.60); volatilities are close (7.78% vs 7.64%).
- **Drawdowns:** 0.45 has a shallower **MaxDD** (−18.34% vs −20.33%) and better **MAR** (0.321 vs 0.215).
- **Monthly stability:** 59.80% positive vs 58.30% at 0.7; best/worst month slightly better at 0.45 (+10.89% / −5.20% vs +9.99% / −5.51%).
- **Underwater:** longest run at 0.45 — 41 months, at 0.7 — 42 months (recovery month not included).
- **Trades:** same count (**2,736**), but 0.45 shows better **PF** (~1.154 vs 1.118) and **expectancy** (~0.036R vs 0.028R).
- **Yearly extremes (comp):** best year stronger at 0.45 (2015 34.94% vs 2015 30.78%), worst year milder (2011 −4.92% vs 2011 −7.09%).

---

## 🔊 What the metrics say (Retail Advanced 0.45 — 4.5 USD/lot, 1% risk)

- **Return/risk.** Sharpe and Sortino exceed the “retail stress” track; **MAR** is better: the strategy consistently earns more than it loses in drawdowns at moderate annual volatility.

- **Drawdowns and “pain.”** **MaxDD** is moderate, **Ulcer Index** low‑to‑mid → the curve’s pain is more often **long but shallow**. **Time Underwater** is elevated: a meaningful share of time below the high‑water mark — this is about investor discipline rather than disasters. **Longest UW** is multi‑year: the worst case is a long wait for a new high, but the depth is controlled.

- **Return distribution shape.** Positive **skew** and positive **excess kurtosis**: the strategy has **occasional strong upsides**, with tails “fatter than normal.” Conclusion: risk management is mandatory, but rare large positives compensate for strings of small losses.

- **Rollings.** **Rolling‑12m** and **Rolling‑36m** show phase behavior: when the market is “in tune” with the model, Sharpe/Calmar in the windows rise markedly; in neutral phases they revert toward the average. Useful to monitor the current regime without “diluting” it with the full history.

- **DD quantiles.** Provides not a single MaxDD but the **typical depth of UW**: you can see how deep “tail” months are (e.g., p95/p99), which is handy for expectations and limits.

- **Trade structure.** **High hit rate**, **modest payoff (~0.5)**, **PF > 1**, **expectancy > 0** → the strategy “wins by frequency and loss control,” not by the size of individual winners.

- **Kelly.** Current risk **1.0R per trade** corresponds to **10× Kelly** (for a 1% risk unit). The institutional approach is **≤ ½ Kelly** (≈ **0.05R**), which is noticeably lower than the current load and reduces the depth/duration of drawdowns at the cost of some return.

- **Probabilistic profile (Monte‑Carlo).** Median outcomes show steady CAGR and moderate drawdowns; the probability of a **long‑term negative result is low**, and **very deep** drawdowns are a rare scenario with proper risk management.

**Bottom line.** 0.45 in the “retail advanced” mode is **moderate risk with improved return quality**: long but shallow UW, high win frequency, positive asymmetry, and a low probability of “black” scenarios.

---

## 📋 Methodology for metric calculation (4.5 USD/lot, 1% risk)

### What is computed and which files

```
compounding_eoy_soy_base_100k/metrics/
  monthly_returns.csv
  full_period_summary.csv
  yearly_summary.csv
  trades_full_period_summary.csv
  rolling_12m.csv
  rolling_36m.csv
  dd_quantiles.csv
  kelly_summary.csv
  monte_carlo_summary.csv

fixed_start_100k/metrics/
  monthly_returns.csv
  yearly_summary.csv
  trades_full_period_summary.csv
```

### CSV Schemas (column names)

**compounding_eoy_soy_base_100k**

- `monthly_returns.csv`:  
  `year, month, ret_m`
- `full_period_summary.csv`:  
  `months, cagr, vol_ann, sharpe_ann, sortino_ann, maxdd, mar_full, longest_underwater_months, best_month, worst_month, pos_months_pct, n_trades, hit_rate, profit_factor, avg_win_r, avg_loss_r, payoff_ratio, expectancy_r_mean, expectancy_r_median, std_r, min_r, max_r, ulcer_index, martin_ratio, time_underwater_pct, skewness, kurtosis_excess`
- `yearly_summary.csv`:  
  `year, ret_year, maxdd_year, trades, hit_rate, profit_factor`
- `trades_full_period_summary.csv`:  
  `n_trades, hit_rate, profit_factor, avg_win_r, avg_loss_r, payoff_ratio, expectancy_r_mean, expectancy_r_median, std_r, min_r, max_r`
- `rolling_12m.csv`:  
  `date, roll_12m_return, roll_12m_sharpe`
- `rolling_36m.csv`:  
  `date_end, ret_36m_ann, maxdd_36m, calmar_36m`
- `dd_quantiles.csv`:  
  `quantile, drawdown_quantile`
- `kelly_summary.csv`:  
  `winsor_lo, winsor_hi, f_opt, f_half, f_quarter, obj_at_f_opt`
- `monte_carlo_summary.csv`:  
  `months, bootstrap_B, block_len_months, cagr_p05, cagr_p50, cagr_p95, maxdd_p50, maxdd_p95, p_cagr_lt_0, p_maxdd_lt_-0.30`

**fixed_start_100k**

- `monthly_returns.csv`:  
  `year, month, ret_m`
- `yearly_summary.csv`:  
  `year, ret_year, maxdd_year, trades, hit_rate, profit_factor`
- `trades_full_period_summary.csv`:  
  `n_trades, hit_rate, profit_factor, avg_win_r, avg_loss_r, payoff_ratio, expectancy_r_mean, expectancy_r_median, std_r, min_r, max_r`

> For **fixed**, a cross‑period `full_period_summary.csv` is **not calculated** (annual reset to 100k).

---

### Rules and conventions

- **Time zone:** UTC+0.  
- **Scale:** monthly **returns** from the **last EoM NAV** (month‑end); missing EoM — **ffill**.  
- **Anchor NAV = 100,000 USD.**  
  - **Compounding:** anchor at `t₀−ε` (before the first point).  
  - **Fixed:** each year — anchor at `YYYY‑01‑01 00:00 UTC−ε`.  
- **Months without trades** are not removed: `ret_m = 0`.  
- **Returns:** arithmetic (not log).  
- **Variances/σ:** sample, `ddof = 1`.  
- **R‑metrics (1% risk):**  
  if `pnl_pct` is in fractions → `R = pnl_pct`; if in percent → `R = pnl_%/100`.  
  `hit_rate = share(R>0)`; `PF = sum(R>0)/|sum(R<=0)|` (on **sums**).

---

### R-metrics (Risk 1% — STRICT)

- **Base:** **1R = 1.0%** of capital at entry.  
- If `pnl_pct` is **in fractions** (e.g., `0.012` = +1.2%) → `R = pnl_pct / 0.01`.  
- If `pnl_%` is **in percent** (e.g., `1.2` = +1.2%) → `R = pnl_% / 1.0`.  
- If `pnl_r`/`r` exists in `trades`, use it **only** if it’s already R at 1% risk.

- Introduce epsilon `eps = 1e-12` for robust comparisons to zero.  
- **Classification:**  
  — **win:** `R > +eps`; **loss:** `R < −eps`; zeros are excluded from the win/loss groups.  

- **Metrics (on R):**  
  — `hit_rate = share(R > +eps)` (zeros are not wins);  
  — `profit_factor = sum(R[R>+eps]) / abs(sum(R[R<−eps]))` (**on sums**, zeros do not participate);  
  — `avg_win_r = mean(R[R>+eps])`, `avg_loss_r = mean(R[R<−eps])`;  
  — `payoff_ratio = avg_win_r / |avg_loss_r|`;  
  — `expectancy_r_mean/median`, `std_r`, `min_r`, `max_r` — computed on **all** R as-is (zeros included).

---

### Formulas (institutional definitions)

**Return/risk (on monthly returns `r_m`)**
- `CAGR = (∏(1 + r_m))^(12/N) − 1`, where `N` is the number of months.  
- `vol_ann = stdev(r_m, ddof=1) · √12`.  
- `Sharpe_ann = (mean(r_m − rf_m) / stdev(r_m − rf_m, ddof=1)) · √12`, default `rf = 0`.  
- `Sortino_ann = (mean(r_m) / stdev(r_m[r_m < 0], ddof=1)) · √12` (downside‑σ only over **negative** months, target=0).

**Curve/drawdowns (monthly scale)**
- `eq_t = ∏(1 + r_m)`, `dd_t = eq_t / cummax(eq) − 1`.  
- `maxdd` = minimum of `dd_t` (negative).  
- `longest_underwater_months` — length of the longest run of months with `dd_t < 0` (the recovery month with `dd=0` is **not included**).  
- `time_underwater_pct = share(dd_t < 0)` (fraction of months “underwater”).  
- `mar_full = cagr / |maxdd|`.

**Ulcer / Martin / Distribution moments**
- `ulcer_index = sqrt( mean( max(0, −dd_t)^2 ) )`.  
- `martin_ratio = (mean(r_m)·12) / ulcer_index`.  
- `skewness` — sample skewness of returns `r_m`; `kurtosis_excess` — Fisher excess kurtosis (minus 3).

**Rolling metrics**
- `rolling_12m`:  
  `roll_12m_return = ∏(1+r_m_window) − 1`;  
  `roll_12m_sharpe = (mean/σ)·√12` within the 12m window (rf=0, `ddof=1`).  
- `rolling_36m (Calmar)`:
  `ret_36m_ann = (∏(1+r_m_window))^(12/36) − 1`;  
  `maxdd_36m` — MaxDD on the monthly curve within the window;  
  `calmar_36m = ret_36m_ann / |maxdd_36m|`.

**Drawdown quantiles**
- `dd_quantiles`: store left‑tail percentiles of the `dd_t` distribution (e.g., `p95`, `p99`), **with a negative sign**.

**Kelly (from the trade R‑distribution)**
- Winsorize `R` tails at `winsor_lo=1%`, `winsor_hi=99%` → `R_w`.  
- Search `f ∈ [0, f_cap]`, where `f_cap = min(10%, 0.95/|min(R_w)|)`.  
- Objective: `maximize E[ log(1 + f·R_w) ]`.  
- Report: `f_opt`, `f_half = f_opt/2`, `f_quarter = f_opt/4`, `obj_at_f_opt`.

**Monte‑Carlo (circular block bootstrap)**
- Default parameters: `B = 5000`, `block_len_months = 3`, fixed seed.  
- On each run of length `N` months, compute `CAGR*`, `MaxDD*` (monthly curve).  
- Report: `cagr_p05/p50/p95`, `maxdd_p50/p95`, `p_cagr_lt_0`, `p_maxdd_lt_-0.30`.

---

### What is **not** published for fixed

- Cross‑series `Sharpe/MAR/MaxDD` and extended metrics — **compounding only**.  
- For `fixed` — **in‑year/annual**: `ret_year`, `maxdd_year`, and annual trade metrics.

---

### Quick integrity checks

- Coverage: **199 months** (2009‑01 … 2025‑07) — no gaps.  
- `∏(1 + ret_m)` (comp) ≈ `NAV_last / 100000`.  
- `months` in `full_period_summary.csv` = rows in `monthly_returns.csv`.  
- `n_trades(full)` = sum of `yearly_summary.trades` = `trades_full_period_summary.n_trades`.  
- For `rolling_12m/36m` the first 11/35 months are empty (no window).

---

### Nuances (address common questions)

- **Rounding (institutional):**  
  CAGR/vol/maxdd/best/worst — **6** decimals; Sharpe/Sortino — **4**; PF/Payoff — **3**; hit_rate/pos_months — **6**; R‑metrics — **6**; Ulcer — **6**; Martin/Calmar — **3**.  
- **Last incomplete month:** included; NAV ffill to EoM.  
- **Months without trades:** keep (`ret_m = 0`).  
- **Sortino:** downside‑σ only for `r_m < 0`, target=0, `ddof=1`.  
- **Sharpe:** `rf=0` (if no rf series). With an annual rf → monthly `rf_m = (1+rf_ann)^(1/12) − 1`.  
- **PF / hit rate:** zeros are not wins; PF — on **sums**; if there are no losses — `PF = inf`.  
- **Underwater:** criterion **`dd < 0`**, recovery month (`dd = 0`) **not included**.  
- **dd_quantiles:** values are **negative** (left tail), do not take absolute value.

> Exact definitions/units/rounding are duplicated in `metrics_schema.json` and are used for automatic pipeline validation.

---

## 🔍 Transparency & Reproducibility

- General info: root **README.md**
- Inputs and provenance: **docs/AUDIT.md / INPUTS‑PIN.md**
- Order execution mechanics: **strategy_proof/README.md**
- Metrics were computed from non‑public files `trades_YYYY.csv` and `equity_YYYY.csv`. Access: see **COMMERCIAL.md**.
- The current track is a **demo M5 (~10–15% of the full EMM logic)**; the calendar filter is light; TCA/slippage — out of scope. 
