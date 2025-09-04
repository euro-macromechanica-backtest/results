# 📦 Results

> **Important note:** the implementation **covers ~10–15% of the full logic** and only trading on the **M5 timeframe** of the full quant strategy **Euro Macromechanica (EMM)**. Even with that coverage, the results demonstrate **stable profitability over a long horizon with percent risk sized off equity at order entry** on minute data that includes **gaps**. See the root `README.md` for details.
>
> - **Cost model (included in all results):** three result variants are available:  
>     - **Institutional** *main track* — *$3 per round‑turn per 1 standard lot (100,000 EUR)*; equivalent to **≈0.3 pip on EURUSD** (≈$1.5 per side). A reference for institutional conditions; the final net assessment is always performed on the counterparty side using their cost model (per‑million per side, markup, slippage) and their data (LP/TCA).
>     - **Retail Advanced** *retail‑ECN mode using a commission cashback service.* **$4.5 per round‑turn per 1 standard lot (100,000 EUR).**
>     - **Retail Baseline Stress** *illustrative assessment under typical retail cost levels* — **$7 per round‑turn per 1 standard lot (100,000 EUR)**, a typical retail‑ECN level; equivalent to **≈0.7 pip on EURUSD** (≈$3.5 per side). A conservative “worst‑case” with the highest retail‑ECN costs; used as a **stress benchmark**. 
> - Order execution mechanics — the public summary (*M5 with M1 micro‑confirmation*) — see `strategy_proof/README.md`.
> - Rounding is set to 5 decimals **only** when generating CSV; all data inside the strategy engine are computed without rounding. Commission is deducted from each trade **after** it closes. Equity after closing a trade = profit/loss + commission from the last trade. The next trade is opened with the configured percent risk off equity — clarification for result accuracy.

**Available results by cost model:**
- *Institutional* — risk 1.0% and 1.5% (start capital 100,000 USD each year and compounding mode over the full period starting at 100,000 USD)
- *Retail Advanced* — risk 1.0% (start capital 100,000 USD each year and compounding mode over the full period starting at 100,000 USD)
- *Retail Baseline Stress* — risk 1.0% (start capital 100,000 USD each year and compounding mode over the full period starting at 100,000 USD)

⭕️ **Conservative showcase. The metrics shown in the results and reports are more likely a lower‑bound estimate. ~10–15% of the full logic is implemented and only 5‑minute trading with a minimally implemented set of quant factors, without institutional data (tick tape, official calendars, precise TCA). These constraints systematically depress the outcome: under the same institutional cost and identical implemented logic, results are likely to be better or, at the very least, not worse.** 

---

## 📚 What’s inside `results/`

- **`headline_2009-2025-07/`** — the main corpus (16 years 7 months headline).  
- **`stress_2001-2008/`** — stress period (illustrative on a degraded feed; outside headline).  
- In each *risk* track:
  - `README.md` for **sub‑tracks** with an **overview of results and metrics**
- In each sub‑track (e.g., *cost_model* `commission_7usd_per_lot/`, then *risk* `risk_1p00/`, `risk_1p50/`, then *mode* `fixed_start_100k/` or `compounding_eoy_soy_base_100k/`) you’ll find:
  - yearly **summary CSV** and **SVG**,
  - the **yearly manifest** `artifacts_YYYY.sha256` (*hashes only, no paths*),
  - GPG signatures/OTS of manifests for the headline,
  - the metrics explainer **metrics_schema.json**,
  - **metrics CSVs** (Sharpe/Sortino/MAR, etc.).
- In

> ℹ️ **The SVG equity is plotted starting from the moment of the first closed trade**. The year’s starting capital is **not** shown as an initial point. This is a visualization script limitation for report generation and does not affect aggregates.

> Detailed per‑trade reports (with timestamps and prices), as well as time series of equity/drawdown tied to timestamps — are not public.

---

## 🎛️ Cost model, risk modes, and capitalization

- **Cost model** — the magnitude of the commission considered in backtest results (retail baseline stress — 7 USD per lot, retail advanced — 4.5 USD per lot, institutional — 3 USD per lot).
- **Risk size** (e.g., `risk_1p00`, `risk_1p50`) is the **fraction of equity at order entry**, **not** of the year’s starting balance.  
- **fixed_start_100k/** — each year starts at $100,000 regardless of the outcome of the previous year.  
- **compounding_eoy_soy_base_100k/** — equity **carries over** year to year (*End‑of‑Year → Start‑of‑Next*), starting from $100,000.

---

## 🧪 Stress (2001–2008)

Stress runs are published **minimally**: only yearly summary CSVs and yearly SVGs.  
Per‑trade reports, minute/bar‑level equity/drawdown series, and **extended analysis** (Sharpe, Sortino, MAR, etc.) are **not published for stress**, because these years are excluded from the headline set by the **data quality policy**.

> For stress, only **one track** is fixed: **`/commission_7usd_per_lot/risk_1p50/fixed_start_100k/`** (a single run per year; purpose — illustration, not risk‑profile comparison).

---

## 🧾 Where to find metrics and reports

- In **each *risk* track** there is its own `README.md` with an **overview of results** and a **summary of metrics** (Sharpe, Sortino, MAR, MaxDD%, summaries, etc.) for the **headline** in the *mode* sub‑tracks — **metrics CSV and metrics_schema.json** in which the calculation methodology is described.
- Full extended analysis and metrics are provided only for the **institutional** and **retail advanced** modes.
- **Stress** intentionally **has no extended metrics** — only summary CSV + SVG (see the “Stress” section above).

---

## 🔗 Inputs binding

- Canon: **data-hub** → [`INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md) (logic linking RAW/PREPARED/Calendars).
- Pin in this repo: `docs/INPUTS-PIN.md` (which input archives were used).  
- SHA‑256/GPG/OTS mirrors of input archives: `integrity/inputs/` (for convenience; **data-hub** remains the source of truth).

---

## 🔐 Integrity verification

### 1) Verify the **yearly manifest** (list of a year’s inputs/outputs)
- **Linux**
  ```bash
  sha256sum -c results/<cost_model>/<risk>/<mode>/YYYY/artifacts_YYYY.sha256
  ```
- **macOS**
  ```bash
  shasum -a 256 -c results/cost_model/<risk>/<mode>/YYYY/artifacts_YYYY.sha256
  ```
- **Windows (PowerShell)** — there is no direct equivalent of `-c`; verify line‑by‑line from the manifest:
  ```powershell
  Get-Content results\cost_model\<risk>\<mode>\YYYY\artifacts_YYYY.sha256 |
    ForEach-Object {
      $parts = $_ -split '\s+'
      $hash  = $parts[0]
      $file  = $parts[-1]
      (Get-FileHash $file -Algorithm SHA256).Hash.ToLower() -eq $hash
    }
  ```

> The yearly `artifacts_YYYY.sha256` files contain **hashes only (64‑char SHA‑256)** with no paths — this improves portability and avoids false mismatches due to directory differences.

### 2) Roll‑up manifest of “manifests” (per track)
For audit convenience, tracks have **roll‑up manifests** (e.g.,  
`integrity/results/headline_2009-2025-07/commission_7usd_per_lot/risk_1p00/fixed_start_100k_manifest/artifacts_2009-2025-07.sha256`)  
— a list of **SHA‑256 of the yearly files** `artifacts_YYYY.sha256` with **relative paths**.

**Verifying that a given year is included in the roll‑up:**  
- **Linux**
  ```bash
  # compute the hash of the yearly manifest file
  sha256sum results/<track>/<mode>/YYYY/artifacts_YYYY.sha256

  # match it against the first column in the roll‑up
  grep -F "<OUTPUT_HASH>  results/<cost_model>/<risk>/<mode>/YYYY/artifacts_YYYY.sha256"     integrity/results/<…>/artifacts_2009-2025-07.sha256
  ```
- **macOS**
  ```bash
  shasum -a 256 results/<track>/<mode>/YYYY/artifacts_YYYY.sha256
  grep -F "<OUTPUT_HASH>  results/<cost_model>/<risk>/<mode>/YYYY/artifacts_YYYY.sha256"     integrity/results/<…>/artifacts_2009-2025-07.sha256
  ```
- **Windows (PowerShell)**
  ```powershell
  $h = (Get-FileHash "results\<cost_model>\<risk>\<mode>\YYYYartifacts_YYYY.sha256" -Algorithm SHA256).Hash.ToLower()
  Select-String -Path "integrity\results\<…>artifacts_2009-2025-07.sha256" -SimpleMatch "$h  results\<cost_model>\<risk>\<mode>\YYYYartifacts_YYYY.sha256"
  ```

> Roll‑up manifests contain **relative paths**. Therefore, verify them **from the repo root** or adjust your paths to match those in the file.

---

## 🖋️ Methodology and data honesty

**Methodology.** Monthly EoM scale (NAV at end of month, UTC+0). Standard deviations are sample (`ddof=1`). **Sortino**: downside σ only on months with `r<0` (target=0). **MAR** (CAGR/|MaxDD|) and **Calmar (36m)** are separated.  

**Slippage** is not modeled, news are not traded (trading starts only after a certain material price move from the time of a high‑impact event — when liquidity normalizes — under specific quant conditions), UTC+0 with DST, zero months are included.

### Metrics based on monthly returns (institutional‑standard)

**Scale and data.**  
- Use **monthly returns `r_m`** from **EoM NAV** (last value at month‑end), **UTC+0**.  
- Before the first point — anchor NAV = **100,000 USD** (`t₀−ε`); missing EoM values are **ffill**.  
- **Months with no trades are included** (their `ret_m = 0`).  
- Standard deviations are sample (`ddof = 1`). Returns are **arithmetic**, not log.

**Annualized metrics (by `r_m`):**  
- `vol_ann = stdev(r_m) · √12`.  
- `Sharpe_ann = (mean(r_m − rf_m) / stdev(r_m − rf_m)) · √12`, default `rf = 0`.  
- `Sortino_ann = (mean(r_m) / stdev(r_m[r_m < 0])) · √12` (downside σ **only on negative months**, target = 0).  
- `CAGR = (∏(1 + r_m))^(12/N) − 1`, where `N` is the number of months.

**Drawdowns and derivatives:**  
- Monthly curve: `eq_t = ∏(1 + r_m)`.  
- Drawdown: `dd_t = eq_t / cummax(eq) − 1` (negative).  
- **MaxDD** — minimum of `dd_t`.  
- **Longest Underwater** — length of the longest run of months with `dd_t < 0` (the recovery month with `dd = 0` **is not included**).  
- **Time Underwater** — share of months with `dd_t < 0`.  
- **MAR (mar_full)** = `CAGR / |MaxDD|`.

**Rolling metrics:**  
- **Rolling‑12m**: `roll_12m_return = ∏(1+r_m_window) − 1`, `roll_12m_sharpe = (mean/σ)·√12` within the window (rf=0).  
- **Rolling‑36m (Calmar)**:  
  `ret_36m_ann = (∏(1+r_m_window))^(12/36) − 1`,  
  `maxdd_36m` — MaxDD on the same window curve,  
  `calmar_36m = ret_36m_ann / |maxdd_36m|`.  
  > The final report publishes **MAR (full)**; **Calmar 36m** is in the rolling file.

**Additional indicators (if computed):**  
- **Ulcer Index**: `sqrt(mean( max(0, −dd_t)^2 ))`.  
- **Martin Ratio**: `(mean(r_m)·12) / UlcerIndex`.  
- **Skewness / Kurtosis_excess** — on `r_m` (Fisher).

> All these metrics depend **only on monthly returns**, not on R.  
> The **R‑metrics block (hit‑rate, PF, payoff, expectancy, Kelly)** is computed **separately** from trades. 

**R‑metrics (STRICT — for all 1% and 1.5% tracks)**  
- Normalization base:
  - **Risk 1%:** `1R = 1.0%` of equity at entry.  
    • if `pnl_pct` is in fractions (e.g., `0.012` = +1.2%) → `R = pnl_pct / 0.01`;  
    • if `pnl_%` is in percent units (e.g., `1.2` = +1.2%) → `R = pnl_% / 1.0`.  
  - **Risk 1.5%:** `1R = 1.5%` of equity at entry.  
    • if `pnl_pct` is in fractions → `R = pnl_pct / 0.015`;  
    • if `pnl_%` is in percent units → `R = pnl_% / 1.5`.  
  - If `trades` contains `pnl_r`/`r`, use them **only** if they already represent R at the corresponding risk.

- Epsilon for stable comparisons: `eps = 1e-12`.

- **Trade classification (STRICT):**  
  — **win:** `R > +eps`; **loss:** `R < −eps`; zeros are excluded from win/loss groups.

- **Metrics (by R):**  
  — `hit_rate = share(R > +eps)` (zeros are not wins);  
  — `profit_factor = sum(R[R>+eps]) / abs(sum(R[R<−eps]))` (**on sums**, zeros excluded);  
  — `avg_win_r = mean(R[R>+eps])`, `avg_loss_r = mean(R[R<−eps])`;  
  — `payoff_ratio = avg_win_r / |avg_loss_r|`;  
  — `expectancy_r_mean/median`, `std_r`, `min_r`, `max_r` — over **all** R as is (zeros included).

*Note:* in historical data, exact zero R values are virtually absent; STRICT is introduced for uniformity and to guard against rounding artifacts.

**Data with gaps.** The quote source is documented; NAV gaps are closed with `ffill` to each EoM. Integrity verified: **199 months** (2009‑01…2025‑07) without “holes”; months with no trades are **left in** (`ret_m = 0`).

**Behavioral risk.** Long underwater periods (**Longest UW (Underwater — time below the high‑water mark) ≈ 41–42 months**) are a property of the demo model, not a bug. The showcase implements only ~**10–15%** of the full strategy logic and applies **minimal** quant filters, demonstrating **stable long‑term profitability** without aggressive “tuning.” This lengthens UW while keeping drawdowns moderate in depth.

**For more on the calculation methodology see `README.md` and the `metrics_schema.json` of each *risk* track.**

---

## 🔎 Data quality policy (brief)

- **Gating rule**: 2001–2008 → `DROP` (stress only); 2009+ → `OK` if the yearly counter of **5–10 minute** gaps is `< 120` (±10% — “BORDERLINE” tag only).  
- Full text: `data_quality_policy/policy_v1.0.md`, table: `data_quality_policy/data_quality_table.csv`.  
- Time/integrity proofs: see artifacts in `integrity/data_quality_policy/`.

---

## 🧾 Licenses and access
- Results and documentation in results/**: CC BY‑NC 4.0.
- Strategy source code, detailed per‑trade reports (with timestamps and prices), and time‑tied equity/drawdown series are not public. Commercial access/deployment: see `COMMERCIAL.md`.

---

## 🗺️ Quick links

- **Full audit guide** — [`docs/AUDIT.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.md)
- **PIN (inputs binding)**: [`docs/INPUTS-PIN.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.md)
- **euro-macromechanica-backtest-data – Data Hub (inputs canon):** [`INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md) in the data repository
- **Integrity (guide to verifying SHA‑256, GPG, and OTS):** [`docs/INTEGRITY.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INTEGRITY.md) in this repository  
- **Headline**: `results/headline_2009-2025-07/…`  
- **Stress**: `results/stress_2001-2008/…`
