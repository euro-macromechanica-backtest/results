# Full Audit & Transparency Verification Guide for the Euro Macromechanica (EMM) M5 Backtest

## Table of Contents

- [‼️ Before you dive in](#before-you-dive-in)
- [⓵ Step 1. Verifying the original HistData inputs](#step-1)
- [⓶ Step 2. Verifying the economic calendars](#step-2)
- [③ Step 3. Verifying prepared minute data and gap analysis outputs](#step-3)
- [⓸ Step 4. Verifying the Data Quality Policy v1.0](#step-4)
- [⓹ Step 5. Verifying inputs and backtest results](#step-5)
- [➕ Appendix](#appendix)
- [📎 Key links](#key-links)

---

## ‼️ Before you dive in
<a id="before-you-dive-in"></a>

- Read the root [`README.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md).
- Make sure you understand **SHA‑256 (hash)**, **GPG signatures** (`.sig`/`.asc`), and **OpenTimestamps (OTS, `.ots` files)** and how to verify them. Details: [`docs/INTEGRITY.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INTEGRITY.md).
- It’s **strongly recommended** to have access to ChatGPT **(advanced modes — e.g., model 5 — Thinking/Pro)**. It significantly simplifies the audit: **rebuilding calendars, normalizing minute data, and running gap analysis**. The **economic‑calendar‑builder** and **minute‑data‑analyzer** were designed for flexible compilation workflows — **especially helpful if you’d rather avoid manual tweaks**. *Bonus:* this also helps when checking **SHA‑256**, **GPG signatures**, and **OTS anchors**.

> If you don’t want to deal with **absolute→relative path conversions** and other **SHA‑256** nuances, see the relevant part of `docs/INTEGRITY.md`.

> **Note.** When unzipping raw minute data from HistData, prepared minute data, and calendars, the inner folder for 2025 is named like `2025_jan-jul`. In the repo the same set sits under `2025-07` (machine‑readable). The contents are identical — archive hashes, `.asc`, and `.ots` all validate. The rename in the repo does **not** change archive hashes.

### To save time, based on Data Quality Policy v1.0 — **verify only the headline period (2009–2025-07)**

---

## ⓵ Step 1. Verifying the original HistData inputs
<a id="step-1"></a>

Raw **EUR/USD** M1 data for **2001–2025‑07** in **Generic ASCII** format were taken from [**HistData**](http://www.histdata.com/download-free-forex-data/?/ascii/1-minute-bar-quotes) at project start — early **August 2025**. Data format: **yearly** minute CSVs plus a TXT report per year; for **2025** — **monthly** CSVs and TXT reports from January through July 2025.

For transparency and ease of audit, copies are hosted in **data-hub** under [`source_data/...`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/source_data) *(all rights remain with the original data providers)*. In case HistData ever asks to remove mirrors, **SHA‑256** and **GPG** for the yearly files — and an **OTS anchor** for the full archive of all yearly CSVs for **2001–2025‑07** — were generated as proof. *(If removal occurs, the verification files will be provided on request.)*

### Evidence of temporal integrity

The combination of **SHA‑256**, **GPG**, and **OTS** proves that the data were obtained at a specific time. This matters because the downloadable files at HistData can change (e.g., they might later fill old gaps).

### **How to verify**

Download files from **data-hub** ([`source_data/...`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/source_data)), compute hashes, and compare them with those published under [`integrity/source_data/...`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/integrity/source_data). Verify signatures for all files and the **OTS** for the archive. If you wish, unpack the archive and match its internal file hashes to the yearly files in the repo. Also verify the archive’s own hash, signature, and **OTS**.

---

## ⓶ Step 2. Verifying the economic calendars
<a id="step-2"></a>

This section explains how to check the calendars used in the backtest — and why you verify **key fields** rather than require **bit‑for‑bit** identity.

### What to check (4 key columns)

1. **Release time** in fixed **UTC+0** (with correct DST adjustments).
2. **Country/region** of the event.
3. **Event name** (the news item).
4. **Impact** = **high**.

> Additionally verify **completeness** (no missing high‑impact events).

See the details on **which events/countries/impacts are included and how** in [`data-hub/economic_calendars/README.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/economic_calendars/README.md).

> If those four columns and the full set of rows match the calendars under [`data-hub/economic_calendars/YYYY/calendar_YYYY.csv`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/economic_calendars), the verification passes.

### Role of calendars in the implemented strategy

- In this demo logic, calendars are of **limited importance**.
- Only events with **impact = high** are taken into account.
- The calendar is an **instrument for macro‑structuring price action and for light filtering**, **not** a signal engine. Signals come from price mechanics.

### Sources and build process

- Calendars were compiled using [`economic-calendar-builder/core_builder`](https://github.com/euro-macromechanica-backtest/economic-calendar-builder) from the **economic-calendar-builder** repo.
- Each event record includes: **impact**, **UTC+0 time**, **country/region**, a **source link**, and a *note*. Actual indicator values are **not** included.
- Sources are **official**: national central banks, national statistics offices, etc.

### Important build nuances

The builder is designed to compile in ChatGPT **(advanced modes — model 5 — Thinking/Pro)**, so the code is intentionally flexible:

- There is no hard‑coded local→**UTC+0** conversion with **DST** — you set that in the compile environment/scripts.
- There are optional build variants for some **EU** countries that are **not used** in the final calendars.
- This design prioritizes **flexibility and speed**.

If you are rebuilding:
- Use **ChatGPT (model 5 — Thinking/Pro)**; build step‑by‑step (e.g., for 2018: build **US** first, then **Euro Area**, etc.).
- Thanks to **manifest‑based versioning**, you can build in parts and then merge for the period (deduping if needed).

### Why SHA‑256 hashes won’t match a fresh rebuild

Bit‑for‑bit identity with the published files is **not guaranteed** for several reasons:

1. **Non‑standardized event names** (the same item may appear as *NFP*, *Unemployment Change — NFP*, *Non‑Farm Payrolls*, etc.).
2. **Links** may be blank (for speed), generic, or precise — **not unified**. Some official sites have migrated; URLs change.
3. **CSV delimiters** weren’t fully standardized across all early builds.
4. Official sources sometimes **change paths**.

> Building **economic-calendar-builder** to enforce **bit‑for‑bit** calendar identity is **not an efficient** approach.

> The links column was added to **quickly validate** that the data came from official sources during development, not as part of the audit surface.

### Audit note: Jackson Hole 2017–2024

Starting in 2017 the *Jackson Hole Economic Symposium* was **excluded** by design. The calendar is used for **macro‑structuring** of price action and **light filtering** of **regular statistical releases**. One‑off conference slots (1–3 in late August each year) are not treated as signal drivers and are left out.

**Methodological rationale.** For the implemented M5 EMM logic, these slots do not form signals and have **no material impact** on aggregated metrics over **16 years 7 months**; the time share is **negligible**.

**Sanity check (retrospective; not used to decide).** In the reference window **2009–2016**, the “event hour” occurs once or twice: private `trades_YYYY.csv` show **2 trades** total (**1** in 2015 — **not** news‑filtered; **1** in 2016 — **with** a filter). This suggests the effect is **statistical noise** on multi‑year aggregation.

**Conservatism.** Since *Jackson Hole* is absent for 2017–2024, the news filter may **not have triggered** in those slots; a few trades might have stopped out during an “event hour.” That bias is toward **slightly understating** results, making the evaluation **conservative** versus fully filtering those slots.

**Non‑Material Impact statement.** Excluding *Jackson Hole* from 2017 onward **does not change conclusions** and **does not materially affect** headline period aggregates.

### How to verify calendars when rebuilding

1. Read [`data-hub/economic_calendars/README.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/economic_calendars/README.md) — it lists **all filters** (countries, events, impact levels, and rules) used by the author.
2. For verification, **only build** the countries and events specified there **with impact = high**.
   - To keep it simple, **skip** **impact = medium** — the demo engine doesn’t use them.
   - You may include Jackson Hole if you like (already discussed).
   - Items **promoted to impact = high** are also listed in `README.md`.
3. For convenience, **download** calendars from [**data-hub/economic_calendars**](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/economic_calendars) and **filter to impact = high** to compare.

### Critical: correct time conversion

**Convert local event times** for **`impact = high`** to **UTC+0** **with DST applied correctly** for the specific country/region.
The calendar is **not** a signal engine; it provides **macro context**. Regular high‑impact releases (as captured by the author) are a **light entry filter**. If you misconvert high‑impact times to UTC+0, especially ignoring DST, **results will be biased downward**.

- Explicitly encode this logic in ChatGPT or implement it locally; the source code **does not** hard‑code this conversion.

> Bottom line: verify four columns — **event**, **country/region**, **impact = high**, **time (UTC+0)**. If they match, the calendar verification passes.

**Add‑on.** If the calendars published in **data‑hub** (`economic_calendars/YYYY/...`) contain any incorrect conversions for `impact = high`, it implies the **backtest results are likely understated** (depending on the share of misconverted high‑impact events).

### SHA‑256 for calendars — what it’s for

Calendar **SHA‑256** artifacts are **not** for bit‑level identity across rebuilds; they are for **verifying the exact input files** used in the backtest. Bit‑for‑bit identity is **unrealistic** (URLs evolve, etc.). Given the demo’s **flexibility/speed** goals and the **minor role** of calendars, **verifying the four columns** (time, name, country/region, impact) is **sufficient**.

**⚠️ Reminder.** After verification, use the published [**data‑hub/economic_calendars**](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/economic_calendars) calendars for later audit steps — their hashes are chained into the rest of the verification.

### Links
- [`data-hub/economic_calendars/README.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/economic_calendars/README.md) — filtering rules, countries, and build nuances.
- [`economic-calendar-builder/core_builder`](https://github.com/euro-macromechanica-backtest/economic-calendar-builder/tree/main/core_builder) — the calendar builder.

---

## ③ Step 3. Verifying prepared minute data (quarterly files + July 2025) and gap analysis outputs
<a id="step-3"></a>

*(dedupe/clean, normalize delimiter to `;`, convert timestamps to fixed `UTC+0`, run gap analysis, generate reports).*

### Original data format

- Source: **HistData**.
- Time format in source CSVs: fixed **UTC-5 without DST** (no seasonal time changes).

### Notes on the analysis reports

- The analysis is tuned for **M5 backtesting suitability**; hence SVGs are named `EURUSD_YYYY_anomalies_M5.svg`, even though they **display all gap durations**.
- The key takeaway: **counts of “abnormal” 5–10 minute gaps** are the most critical (why — see Step 4).
- Other report elements are for **context** and a general view of **minute‑data quality**.

### Two‑stage normalization & analysis

Normalization and analysis were performed with [**minute‑data‑analyzer/core_analyzer**](https://github.com/euro-macromechanica-backtest/minute-data-analyzer/tree/main/core_analyzer).

**1) Normalization**
- Remove **duplicates and broken rows**.
- Normalize delimiter to **`;`**.
- Convert times **from `UTC-5` (no DST) to fixed `UTC+0`**.
- Split the data into **quarterly** files and a **monthly** file for July 2025.

**2) Analysis**
- Analyze **gaps** using:
  - **Economic calendars** (*impact = high*),
  - **FX weekends/holidays**,
  - Detection of **abnormal gaps**.
- Generate **reports** and **SVG graphics**.

### How to repeat normalization & analysis

- Use **ChatGPT (advanced modes — model 5 — Thinking/Pro)** — the code targets that environment for flexibility and speed. **Feed the compilation guide below into ChatGPT to configure and run the pipeline.**
- You can run locally if you adapt the analyzer to the **compilation guide**.
- Process **one year at a time**, following the two stages above.

### ⚠️ Important

Follow the compilation guide exactly: it describes the raw analyzer pipeline, ensures determinism for prepared minute files (`*.csv.gz`), and configures report generation.

**data‑hub links:**
- Raw minute data: [`source_data/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/source_data/)
- Calendars: [economic_calendars/](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/economic_calendars/)
- Integrity artifacts: [integrity/](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/integrity/)
- Analysis README → [`analysis/README.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/analysis/README.md)
- **Compilation guide**: [`analysis/COMPILATION-GUIDE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/analysis/COMPILATION-GUIDE.md)

### Verification

Stage 1 (normalization) produces deterministic `*.csv.gz` outputs.

1. Verify **SHA‑256** and **GPG signatures** of `.csv.gz.sha256` and `.csv.gz.sha256.asc` under `integrity/prepared/YYYY/csv.gz_sha256/...` against the `prepared/YYYY/...` `*.csv.gz` files in **data‑hub**.
2. Do the same for the unzipped `.csv` files (`prepared/YYYY/...`) vs. `integrity/prepared/YYYY/csv_sha256/...`.
3. Optionally, unzip the `*.csv.gz` you downloaded and match hashes against `prepared/YYYY/...`.
4. Download the rollup archive of all **prepared** data — `eurusd_prepared_2001-2025-07.tar.gz` under `prepared/` — and verify the hashes of its contents.
5. Verify the hash, GPG signature, and OTS of the archive itself (`integrity/prepared/`).
6. Cross‑check hashes of all inputs (minutes and calendars) and **reports** (`.svg.gz`) against the analysis manifest under `analysis/YYYY/output_bundle_report/`.

### Important: a report/manifest line nuance

Be aware of a **specific line nuance** in `annual_report_YYYY.md` when generating a manifest. See
[`data-hub/analysis/ANALYSIS-INTEGRITY.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/analysis/ANALYSIS-INTEGRITY.md)
for details on verifying inputs/outputs published in **data‑hub** that result from normalization and analysis.

### After repeating normalization & analysis with minute‑data‑analyzer…

Compare your `*.csv.gz` hashes to those under `integrity/prepared/YYYY/csv.gz_sha256/...` in **data‑hub**. Cryptographic checks for analysis reports, SVGs, and manifests are **not** required: a visual check of 5–10 minute gaps by year is sufficient. (Hashes will not match unless you reproduced the analyzer’s determinism **and** used calendars from **data‑hub**.)

---

## ⓸ Step 4. Verifying the Data Quality Policy v1.0
<a id="step-4"></a>

To validate the policy that splits the history into **stress** and **headline**, verify the **hashes**, **signatures**, and **OTS anchors** of the data‑quality CSV table in the **results** repo:
[`integrity/data_quality_policy/data_quality_table.csv.sha256(.asc/.ots)`](https://github.com/euro-macromechanica-backtest/results/blob/main/integrity/data_quality_policy).

**OTS** shows when these artifacts were anchored.

### “Was the anchoring timeline staged?”

**If you’re skeptical:** look at the **minute‑data analysis reports and SVGs** and compare the number of gaps in **2001–2008** vs. **2009–2025‑07**.

The threshold was set at **120 abnormal 5–10 minute gaps per year** — broad enough to include more of the sample (e.g., keep **2010** in the headline period) while still **avoiding biased results** from poor‑quality data.

**Why not start at 2008?**
- The difference in 5–10 minute gaps between **2008** and **2009** is almost **≈×3**: **2008 — 164**, **2009 — 58**.
- Between **2009** and **2010** it’s **just under ×2**: **2009 — 58**, **2010 — 107**.
- **2001–2007** are much worse and not worth debating.

Ideally, such gaps should be **minimal** or **absent**; same for other gaps. But we worked with data we could obtain. The implemented minimal engine logic focuses on **M5**, so the primary data‑quality filter targets **5–10 minute gaps**.

All of this **supports** the **soundness and verifiability** of the chosen **Data Quality Policy v1.0**.

---

## ⓹ Step 5. Verifying inputs and backtest results
<a id="step-5"></a>

To verify inputs and results, ensure that the hashes of the actual input files **used** (`data‑hub/integrity/economic_calendars/YYYY/...` and `data‑hub/integrity/prepared/YYYY/...`) match the input hashes **listed in the yearly manifests** of the corresponding results track (`results/headline_2009-2025-07/*cost_model*/*risk*/*mode*/YYYY/artifacts_YYYY.sha256`).

**For pinning and provenance:**
- See the pinned doc in the results — [`results/docs/INPUTS-PIN.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.md). It references the provenance canon — [`data‑hub/INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md).

**Input source:** all inputs came from the [`data‑hub`](https://github.com/euro-macromechanica-backtest/data-hub) repo under `economic_calendars/...` and `prepared/...`.

**What to compare exactly:** match **SHA‑256** hashes for the files in those folders with the hashes listed in the yearly manifests of the corresponding results track. This ensures input fidelity and reproducibility. Also verify **GPG** and **OTS**. Additionally, verify the hashes, signatures, and OTS of the results **rollup manifests** for each “mode”, located under `results/integrity/results/...`.

---

## ➕ Appendix
<a id="appendix"></a>

### Result metrics

Performance overviews and metrics are based on **non‑public files** — detailed trade logs (with timestamps and prices) and time‑aligned equity/drawdown series. There are no separate SHA/GPG/OTS artifacts for metrics since they are reproducible from public inputs. All artifact hashes are enumerated in the yearly manifests; metrics are auditable via the integrity of the results files.

**Aggregation methodology (monthly basis)**

**NAV (Net Asset Value):** portfolio value at time *t*. In the backtest:
`NAV_t = starting capital + cumulative net PnL after fees`
(+ mark‑to‑market of open positions, if any).

**Base series:** end‑of‑month (**EoM**) **NAV** at each calendar month‑end (UTC+0).
**Monthly return:** `ret_m = NAV_t / NAV_{t-1} − 1` (months without trades → `ret_m = 0`).

**Why monthly:** **clearer and more robust** — a single calendar step, less intra‑month noise/artifacts, easier to audit and reproduce, consistent annualization.

**Portfolio‑level metrics** (computed on the full monthly series):
CAGR, MaxDD, MAR, Sharpe/Sortino (annualized ×12), Ulcer/Martin, UW (longest/time), % positive months, best/worst month.

**Yearly figures** — aggregated from `ret_m`: `∏(1+ret_m) − 1` for the year; intra‑year DD is taken from the monthly NAV curve within the year.

**Trade metrics** (hit rate, PF, payoff, expectancy in R) — computed over all trades; **independent** of monthly aggregation and nominal risk.

**Tracks:** *compounding* carries NAV across years; *fixed‑start* rebases each year’s start to 100k — formulas for the metrics are unaffected.

> **Detailed metric methodology and result overviews** are in each track’s `README.md` (`results/headline_2009-2025-07/*cost_model*/*risk*/README.md`) and in each mode’s `metrics_schema.json` — `fixed_start_100k` and `compounding_eoy_soy_base_100k`.

### Proprietary EMM M5 engine

**SHA‑256, GPG signatures, and OTS** for the backtest engine code serve as **proof of existence** of the implemented logic. For commercial engagements or when additional assurance is needed, the original engine used for the entire EMM backtest can be **easily verified** via this **cryptographic combination**.

### Note on live runs

Links to live run videos (and bundles with SHA‑256/GPG/OTS) are placed in the main README: see **“Live Backtest Runs (video)”** in the root `README.md`.

## 📎 Key links
<a id="key-links"></a>

- [**Root README.md**](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)
- [**Verifying SHA‑256, GPG signatures, OTS**](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INTEGRITY.md)
- [**data‑hub**](https://github.com/euro-macromechanica-backtest/data-hub/)
- [**minute‑data‑analyzer**](https://github.com/euro-macromechanica-backtest/minute-data-analyzer)
- [**economic‑calendar‑builder**](https://github.com/euro-macromechanica-backtest/economic-calendar-builder)
- [**Backtest results**](https://github.com/euro-macromechanica-backtest/results/tree/main/results)
- **Commercial** — [COMMERCIAL.md](https://github.com/euro-macromechanica-backtest/results/blob/main/COMMERCIAL.md)
