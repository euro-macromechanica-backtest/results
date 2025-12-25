# Euro Macromechanica (EMM) M5 Engine – Backtest Results

## ‼️ Must read — [Euro Macromechanica (EMM) Backtest — Overview and Methodology](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)

---

## 📚 Backtest periods under Data Quality Policy v1.0

- **Core Baseline** 2008–2025-08 — **`core_baseline_2008-2025-08/`** — primary backtest interval at **minute** data quality (17 years 8 months)
- **Extended Baseline** 2003–2007 — **`extended_baseline_2003-2007/`** — interval with lower **minute** data quality (5 years)
- **Stress** 2001–2002 — **`stress_2001-2002/`** — stress period (illustrative on a degraded **minute** quote stream (M1); outside the *baseline* interval)
- **Composite Baseline (Extended + Core)** 2003–2025-08 — **`composite-baseline_extended-core_2003-2025-08/`** — illustrative interval on mixed‑quality data — a reference *pooled view*, published as a *reference* figure over a long historical horizon. It covers virtually the entire “retail‑broker trading era” for the euro and nearly the whole lifespan of the euro as the EU’s single currency (since cash introduction in 2002). Due to mixed data quality, the extended metric set is not published and this track is not used for headline claims.

> Data Quality Policy v1.0 — [`data_quality_policy/policy_v1.0.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/data_quality_policy/policy_v1.0.md)

---

## 🎛️ Cost model

### The cost model is dynamic with respect to notional volume per trade and calibrated to top‑tier ECN FX brokers. See details in the [Euro Macromechanica (EMM) Backtest — Overview and Methodology](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)

**Cost model (commission, spread, slippage) M5 EMM cost model v1.0** — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv).  
**Notional‑volume cost table (spread, slippage)** — [/docs/cost_model/eurusd_market_order_costs_ecn_round-turn_pips_v1.0](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/eurusd_market_order_costs_ecn_round-turn_pips_v1.0.csv).

### Commission modes
- **Institutional** — *main track*: average institutional commission **$5.5 per round‑turn per 1 standard lot (100 000 EUR)**; equivalent **≈0.55 pips on EURUSD** *(≈**$2.75** per side)* — benchmark for institutional conditions.
- **Retail Rebate** — average retail commission after cashback **$5 per round‑turn per 1 standard lot (100 000 EUR)**; equivalent **≈0.5 pips on EURUSD** *(≈**$2.5** per side)* (mode assumes use of a commission‑rebate service).
- **Retail Standard** — *illustration of typical retail commission*: **$7 per round‑turn per 1 standard lot (100 000 EUR)**; equivalent **≈0.7 pips on EURUSD** *(≈**$3.5** per side)*.

### Commission, spread, and slippage for each trade are included in PnL
The backtest engine accounts for costs **on every trade**.

---

## 📦 Structure and contents

> Example layout:  
> `core_baseline_2008-2025-08/retail_rebate/risk_1p00/fixed_start_100k/YYYY/(balance_YYYY.svg|summary_YYYY.csv|artifacts_YYYY.sha256)`

- `core_baseline_2008-2025-08` — backtest period per Data Quality Policy v1.0.
- `retail_rebate` — *profile*.
- `risk_1p00` — *risk* (risk size).
- `fixed_start_100k` — *mode* (capitalization mode).
- `YYYY` — year.
- `balance_YYYY.svg`, `summary_YYYY.csv`, `artifacts_YYYY.sha256` — backtest outputs and integrity manifest.

> Detailed trade reports (with timestamps and prices), as well as time‑tied balance/drawdown series, are **non‑public**.

Each `mode` includes a `README.ru.md` with an overview and **CSV metrics** under `metrics`.  

> Rounding for backtest outputs (`balance_YYYY.svg`/`summary_YYYY.csv`) is **set** to 5 decimals **only** at CSV generation; calculations inside the engine are performed without rounding.

### Available results

- **Core Baseline:**
  - *Institutional* — risk **1.0%** (capitalization modes: Notional 1M-5M, Notional 5M-15M, Notional 15M-30M).
  - *Retail Rebate* — risk **1.0%** (start capital $100 000 each year and compounding across the full period from a $100 000 start).
  - *Retail Standard* — risk **1.0%** (start capital $100 000 each year and compounding across the full period from a $100 000 start).

- **Extended Baseline:**
  - *Retail Rebate* — risk **1.0%** (start capital $100 000 each year and compounding across the full period from a $100 000 start).
  - *Retail Standard* — risk **1.0%** (start capital $100 000 each year and compounding across the full period from a $100 000 start).

- **Stress:**
  - *Retail Standard* — risk **1.0%** (start capital $100 000 each year).

- **Composite Baseline (Core + Extended):**
  - *Retail Rebate* — risk **1.5%** (compounding across the full period from a $100 000 start).

- **GBPUSD Cross Asset Test** - risk **1.0%** (start capital $100 000 each year and compounding across the full period from a $100 000 start).

**The Institutional profile uses capitalization modes tied to notional volume ranges at 1.0% risk**
  - Notional 1M-5M (starting balance $1 000 000, rebase to $1 000 000 when the threshold ≥ $1 500 000 is reached at year‑end close).
  - Notional 5M-15M (starting balance $3 400 000, rebase to $3 400 000 when the threshold ≥ $4 500 000 is reached at year‑end close).
  - Notional 15M-30M (starting balance $7 900 000, rebase to $7 900 000 when the threshold ≥ $9 000 000 is reached at year‑end close).
> Details on institutional modes are provided in [`Euro Macromechanica (EMM) Backtest — Overview and Methodology`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)

### Stress — data‑quality track (2001–2002)

Stress runs are published **minimally**: only `fixed_start_100k`.  
No **analysis** is published for Stress, because these years are excluded from the Baseline per the **Data Quality Policy**.

> One Stress track is fixed:  
> `.../retail_standard/risk_1p00/fixed_start_100k/`  
> (one run per year; the goal is illustration on a degraded **minute** quote stream (M1), not comparison of profiles).

---

## 📋 Metric calculation methodology (key points)

- **Time zone:** all timestamps and anchors are **UTC+0**.  
- **Results are based on realized P&L (closed balance).**
- **Monthly scale (EoM):** balance at **month‑end close**; if a month has no trades — `monthly_return = 0` (balance is carried forward, equivalent to 0%).  
- **Variances/σ:** sample (`ddof = 1`).  
- **Drawdowns:** `dd_t = eq_t / cummax(eq_t) - 1`; computed **separately** on the EoM grid and on the continuous **intramonth** path (raw balance − closed‑equity, realized‑only).  
- **Longest underwater (EoM):** the longest `dd < 0` sequence from peak to recovery (see TTR below); reported in **months**.  
- **TTR / Months since MaxDD trough:**  
  — **TTR (time‑to‑recover):** number of months to full peak recovery, **including** the month of recovery; if not recovered — `NaN`.  
  — **Months since MaxDD trough:** months since the MaxDD trough to the current end of period, **excluding** the trough month.  
- **DD quantiles:** computed on the **signed** drawdown depth (negative values). Therefore, as the percentile rises, values become **less negative** (closer to 0).  
- **Yearly metrics:** computed strictly **within the calendar year**; the DD peak **resets on January 1, 00:00 UTC+0**.  
- **Active months — share:**  
  — in `yearly_summary.csv`: `active_months_share = active_months_count / 12`.  
     *Note:* the divisor is **fixed (=12) even for the current partial year**. Actual coverage is reported separately in `months_in_year_available`. This is for cross‑year comparability; in a partial year the share may appear lower — this is not an error.  
  — in the **full period**: `active_months_share = active_months_count / months`.  
- **R‑metrics (normalized by actual per‑trade risk):** `R = pnl_pct / risk_per_trade`.  
  Classification: `win: R > +eps`, `loss: R < −eps`, `zero: |R| ≤ eps`. **PF by sums**; zeros are excluded from the PF denominator and from the win rate.  
  *Examples:* with `risk=1%` → `R = pnl_pct / 0.01`; with `risk=1.5%` → `R = pnl_pct / 0.015`.  
- **Sharpe/Sortino (monthly base → annualized):** computed on **monthly returns**, `rf = 0`. Annualization:  
  `Sharpe_ann = Sharpe_monthly × √12`, **Conditional Sortino** `(loss‑only sample LPSD; ddof=1; target=0% monthly; annualized ×√12)`.  
- **Calmar (EoM only):** `Calmar = CAGR / |MaxDD (EoM)|`.  
- **MAR = Calmar (Full period):** in the report, “MAR” means **Calmar over the full period** —  
  `Calmar (EoM; full period) = CAGR_full_period / |MaxDD_EoM_full_period|`.  
  For yearly rows (if shown), **Calmar (EoM; Year)** is used, computed from `CAGR_year` and `MaxDD_EoM_intra_year`. Calmar is not computed for “Active Months (Supp.)”.  
- **Signs:** **MaxDD** (EoM/intramonth) and **DD quantiles** are **negative**; these are depths.

### Conditional Sortino calculation (strict, loss-only)
**Definition.** Reports use a strict (conservative) Sortino variant focused on accurate risk assessment for trading strategies. Downside deviation is computed **only on losing months** (loss-only) relative to the target (T=0%) per month, with **statistical** standard deviation \(\mathrm{ddof}=1\). Annualization multiplies by \(\sqrt{12}\).

$$
\mathrm{Sortino}
= \frac{\bar r - T}{\sigma_{\text{down, loss-only}}}\cdot\thinspace\sqrt{12}
$$

$$
\sigma_{\text{down, loss-only}}
= \sqrt{\frac{\sum\limits_{i\thinspace\colon\thinspace r_i < T} (r_i - T)^2}{M - 1}}
$$


where \(M\) is the number of losing months in the window. If \(M<2\), the metric is not published (*insufficient negatives*).

**Difference from the “standard” Sortino.** In industry practice a variant based on LPM(2) is common, where the denominator uses **all observations** \(N\) (positive and zero months contribute zero), and \(\mathrm{ddof}=0\) is more typical:

$$
\sigma_{\text{down, std}}
= \sqrt{\frac{1}{N}\sum_{i=1}^{N}\big(\min(r_i - T, 0)\big)^2}
$$

This approach “dilutes” risk with non-negative months and typically yields **higher** Sortino values than the strict loss-only method.

**Comparability to industry.** For a rough conversion to the “standard” LPM variant you can use the rule of thumb:

$$
\mathrm{Sortino}_{\text{std}}
\approx
\mathrm{Sortino}_{\text{loss-only}}\cdot\thinspace\sqrt{\frac{N}{M-1}}
$$

where \(N\) is the window size (e.g., 12 or 36 months) and \(M\) is the number of losing months. With small \(M\), the standard Sortino will be substantially higher.

**Note on rolling windows.** On short windows (12 m), when the number of losing months \(M\) is small, the strict method often gives \(\mathrm{Sortino} < \mathrm{Sharpe}\) — an expected property of the formula, not a calculation error. Over longer horizons the effect persists but is weaker.

**Calculation parameters.**
- Target level: \(T = 0\%\) per month.
- Downside deviation: losing months only; \(\mathrm{ddof}=1\).
- Annualization: multiply by \(\sqrt{12}\).
- Publication policy: if \(M<2\), the metric is flagged as missing.

### OOS / Walk-Forward / Multiple Testing & Selection Bias

**Time-based OOS / Walk-Forward are not applied**, as a **single fixed M5 EMM logic** is used **without re-optimization or parameter tuning**. The purpose of time-based OOS is to detect overfitting after parameter adjustments, **which is not relevant here**.

**Multiple testing / selection bias are absent**, since **only one hypothesis / one parameter set** was tested, with no configuration sweeps and no cherry-picking.

**Cross-asset test on GBPUSD** serves as an independent **out-of-sample (OOS) by asset**: parameters optimized on EURUSD were applied **without any changes** to GBPUSD. Post-Brexit, the EURUSD–GBPUSD correlation decreased significantly, so successful application of the model on GBPUSD confirms:
- absence of overfitting,  
- the model’s ability to **generalize**,  
- stability and robustness of the performance profile on an independent asset.

**Strategy robustness is further supported by**:
- long historical data,  
- equity curve dynamics (files `balance_YYYY.csv`),  
- 12/36-month rolling periods,  
- drawdown quantiles,  
- BCa confidence intervals,  
- Monte Carlo (stationary bootstrap across all configurations).

Upon request, a **holdout period** can be prepared for independent auditing, without re-optimization or parameter changes.

> **Calmar — denominator protection:** an **ε‑guard** is applied to `|MaxDD|` to avoid division by near‑zero.  
> **Rolling‑window completeness:** rollings are computed only on **full windows**; incomplete windows yield `NaN` and the flag `insufficient_months=true`.

### Intramonth vs EoM (short)
- **EoM (end‑of‑month):** metrics on a monthly grid from the month‑end closing balance (UTC+0) — drawdowns/risk are “softer.”  
- **Intramonth (path‑level):** metrics on the continuous balance path (UTC+0) — MaxDD is typically deeper than EoM.

### Monte Carlo 
- **Method:** `stationary_bootstrap` (monthly blocks).
- **Horizons:** `12`, `36`, `full_period` (each computed separately).
- **Number of paths:** **10,000**.
- **RNG seed:** **42**.
- **Average block lengths:** **3–12 months** (iterate over **L ∈ {3,…,12}**; metrics are aggregated by the **median** across configurations).

> Note: final Monte Carlo figures are aggregated **by the median** across block-length configurations.

### Confidence Intervals (BCa)
- **Method:** `bootstrap_bca` (BCa — bias-corrected & accelerated).
- **Number of resamples:** **5,000**.
- **RNG seed:** **43**.
- **Accounting for temporal dependence:** `stationary_bootstrap`  
  — for **EoM metrics**: average block length **6 months**;  
  — for **intramonth metrics**: average block length **5 days**.  
- **Confidence level:** **90%**.

### Trade Bootstrap (EDR & losing streaks)
- **Input:** a series of per-trade `R` values (normalized by risk per trade).  
- **Method:** `stationary_bootstrap` over trades (preserving temporal dependence via block resampling).
- **Number of paths:** **5,000**.
- **Path horizon:** **100** trades.
- **RNG seed:** **44**.  
- **Average block length:** determined by the block-break probability **p = 0.2**, hence `E[L] = 1/p = 5` trades.  
  **Implementation:** at each step, with probability `p` start a **new block** (jump to a random index of the original series); otherwise continue the **current block** (advance the index with wrap-around).

> ### Note on the source of truth for medians and aggregates shown in overviews
> - The **source of truth** for medians, quantiles, shares, and other aggregates is the **metric CSVs** in the `metrics/` directory.  
> - All summary values in the overviews are **recomputed directly from the CSVs**.  
> - If a **discrepancy** is found in the overview text (vs. what follows from the CSVs), **the CSV‑based calculations are authoritative**.

### Full methodology and metric definitions
See [`docs/metrics_methodology/metrics_schema.json`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json) and [`docs/metrics_methodology/metrics_schema.md`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md).  
Duplicated **methodology** files and the calculator script are published in the [`metrics-toolkit`](https://github.com/euro-macromechanica-backtest/metrics-toolkit) repository for transparency.

---

## 🧾 Metric files

**Metric CSVs** are located under each *mode* in the `metrics` folder. Example path:  
`core_baseline_2008-2025-08/institutional/notional_1M-5M_1m/metrics/`

**Base metric set:**
```
metrics/
  monthly_returns.csv
  full_period_summary.csv
  rebasing_applied.csv
  yearly_summary.csv
  trades_full_period_summary.csv
```

**Extended metric set:**
```
metrics/
  confidence_intervals.csv
  dd_quantiles_full_period.csv
  monthly_returns.csv
  monte_carlo_summary.csv
  full_period_summary.csv
  rebasing_applied.csv
  rolling_12m.csv
  rolling_36m.csv
  trades_full_period_summary.csv
  yearly_summary.csv

```
> - `rebasing_applied.csv` — for modes with balance rebasing.

- The full extended metric set is published only for **Core Baseline** results.
- **Extended Baseline** and **Composite Baseline (Extended+Core)** are intentionally published **without extended metrics** — base set only.

---

## 🔗 Inputs binding

- Source of truth: **data‑hub** → [`INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md) (logic linking RAW/PREPARED/Economic Calendars).
- PIN in this repository: [`docs/INPUTS-PIN.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.md) (which input files were used).  
- Mirrors of SHA‑256/GPG/OTS roll‑up manifests listing SHA‑256 hashes of input files: `integrity/inputs/` (for convenience; the source of truth remains **data‑hub**).

---

## 🧾 Licenses and access
- Results and documentation in `results/**` — CC BY‑NC 4.0.
- Strategy source code, detailed trade reports (with timestamps and prices), and time‑tied balance/drawdown series are non‑public.

---

## 📎 Links

- **[Euro Macromechanica (EMM) Backtest — Overview and Methodology](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)**
- **Full methodology and metric definitions**: [`docs/metrics_methodology/metrics_schema.json`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json) / [`docs/metrics_methodology/metrics_schema.md`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md)
- **Cost model (commission, spread, slippage) M5 EMM cost model v1.0** — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv).
- **Notional‑volume cost table (spread, slippage)** — [/docs/cost_model/eurusd_market_order_costs_ecn_round-turn_pips_v1.0](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/eurusd_market_order_costs_ecn_round-turn_pips_v1.0.csv).
- **Audit guide** — [`docs/AUDIT.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.md)
- **SHA‑256, GPG and OTS verification guide** — [`docs/INTEGRITY.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INTEGRITY.md) 
- **PIN (inputs binding)** — [`docs/INPUTS-PIN.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.md)  
- **Data Hub (canonical source of input binding)** — [`INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md)
- **Duplicated methodology files and calculator scripts** — repository [`metrics-toolkit`](https://github.com/euro-macromechanica-backtest/metrics-toolkit)

