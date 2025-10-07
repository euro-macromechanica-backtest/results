# Full Audit & Transparency Verification Guide for the Euro Macromechanica (EMM) M5 Backtest

## Table of Contents
- [Before You Start the Deep-Dive Audit](#prep)
- [⓵ Step 1. Verify HistData Primary Data](#step-1)
- [⓶ Step 2. Verify Economic Calendars](#step-2)
- [③ Step 3. Verify Prepared Minute Data (yearly files and monthly files through August 2025)](#step-3)
- [⓸ Step 4. Verify Minute-Data Gap Analysis and the Data Quality Policy v1.0](#step-4)
- [⓹ Step 5. Verify Backtest Inputs and Results](#step-5)
- [➕ Metrics](#metrics)
- [Proprietary M5 EMM Strategy Engine Code](#engine)
- [📎 Key Links](#links)

<a id="prep"></a>
## ‼️ Before Starting the Deep-Dive Audit

- Read the **Euro Macromechanica (EMM) Backtest – Overview & Methodology** – root [`README.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md).
- Understand **SHA-256 hashes**, **GPG signatures**, **OTS anchors**, and **GPG-signed commits** (shown as *Verified* on GitHub), and know how to verify them. See [`docs/INTEGRITY.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INTEGRITY.md) for details and the related artifact types (`.sha256`, `.asc`, `.ots`).

---

<a id="step-1"></a>
## ⓵ Step 1 — Verify the HistData Raw Data

The backtest was run on HistData minute data.  
Raw **EUR/USD** minute data for **2001–2025-08** in **Generic ASCII** were obtained from [**HistData**](http://www.histdata.com/download-free-forex-data/?/ascii/1-minute-bar-quotes). Data format: **yearly** minute CSVs plus a minute-data integrity TXT report **for each year**; for **2025**, **monthly** CSVs and TXT reports from January through August 2025 inclusive.

For ease of verification and transparency, the data have been placed in the **data-hub** repository under [`source_data/raw/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/source_data/raw) *(all rights belong to the data providers)*. In case HistData requests removal of the raw downloads in the future, **roll-up manifests** listing **SHA-256 hashes** of all yearly minute CSVs and the TXT reports for **2001–2025-08** were generated; each such manifest has a published **GPG signature** and an **OTS anchor**.

### Proving the Integrity of the Raw Data Over Time

The combination of **SHA-256**, **GPG**, and **OTS** proves that the specific data were obtained by the author at a particular point in time, since the integrity of files hosted by HistData itself may change (e.g., added data that patch historical gaps, etc.).

### How to verify

Download the files from **data-hub** (folder [`source_data/raw/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/source_data/raw)), **compute hashes**, and compare them to the hashes published in the roll-up manifest at [`integrity/source_data/raw/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/integrity/source_data/raw); then verify the **GPG signature** and **OTS anchor** for the corresponding roll-up manifest. Repeat the same for the TXT reports.

---
<a id="step-2"></a>
## ⓶ Step 2. Verify Economic Calendars

**Economic calendars are not used for bit-for-bit matching because that is not meaningful. A roll-up manifest of hashes for the calendars normalized to UTC+0 is provided to confirm that these calendars were used as inputs to the backtest. The role of economic calendars and the detailed event list are described in the Euro Macromechanica (EMM) Backtest overview and methodology.**
- Each event record contains core columns: **impact**, **time in UTC+0**, **country/region**, and a **source link**. Actual indicator values are **not** included. For transparency there is also a column with the timestamp of when the data were obtained (UTC+0).
- Sources are **official websites**: national banks, national statistical agencies, etc.

All calendars are published in the **data-hub** repo at [`economic_calendars/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/economic_calendars)

Under `economic_calendars/raw/` you’ll find calendars with release times in each source’s local time — full years and split parts for convenient verification. Under `economic_calendars/prepared/` are the versions **normalized to UTC+0** with DST transitions accounted for using the IANA time zone database; **these are exactly the files used as inputs for the backtest**.  
For full transparency, the calendar normalizer script lives in **data-preparation-toolkit** – [`economic_calendar_normalizer`](https://github.com/euro-macromechanica-backtest/data-preparation-toolkit/tree/main/economic_calendar_normalizer). 

> The normalization script converts event times to UTC+0 using the IANA time zone database, so DST transitions are handled correctly.

### How to verify

For a meticulous verification, build your own calendars (or use your in-house calendars, if any) and compare them to the published ones — specifically the completeness of events, the key columns and their values: events, release times in UTC+0 with each source’s DST transitions, and the configured “impact” filter. The event descriptions and impact-filter settings are documented in the **Euro Macromechanica (EMM) Backtest – Overview & Methodology**.

### Bottom line

The presence of events in local time, the normalized UTC+0 calendars and the normalizer script that `economic_calendars/raw/` went through, plus the roll-up manifest listing the hashes of `economic_calendars/prepared/` — **jointly prove** the full reproducibility and transparency of the calendars used in the backtest.

---

<a id="step-3"></a>
## ③ Step 3. Verify Prepared Minute Data (yearly files and monthly files through August 2025)

**Source data format**
- Source: **HistData**.  
- Time format in raw CSVs: **fixed `UTC−5` without DST** (no daylight-saving transitions) — stated in the provider’s FAQ: http://www.histdata.com/f-a-q/.

The **normalized** minute data used as backtest inputs are in **data-hub** — [`source_data/prepared/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/source_data/prepared).  
The roll-up manifest listing the hashes of the normalized minute data is in **data-hub** — [`integrity/source_data/prepared/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/integrity/source_data/prepared).

### What normalization includes

Only conversion to **UTC+0**, sorting by UTC+0, and assigning column names — `datetime, open, high, low, close, volume`. The minute data themselves were **not altered or removed**.

As with the calendars, the full normalizer code is open for transparency in **data-preparation-toolkit** — [`minute_data_normalizer`](https://github.com/euro-macromechanica-backtest/data-preparation-toolkit/tree/main/minute_data_normalizer).

### How to verify

If you need a meticulous verification, use third-party tools to check the completeness and identity of minute data between `source_data/raw` and `source_data/prepared/`. Or run all `source_data/raw/` CSV minutes through the normalizer and compare the output hashes to those in the roll-up manifest.  
**Important:** ensure your Python version and dependencies match those specified in *requirements.lock(.txt)* and `README.md` in `minute_data_normalizer`.
> Note: the minute-data normalizer is configured to be **deterministic**. Given identical inputs, output file hashes will typically match — provided the Python and library versions are the same. If, against expectations, hashes don’t match (which is highly unlikely given the deterministic setup), verify CSV completeness with third-party tools. All files are published transparently.

### Bottom line

The availability of the raw minute data and their roll-up manifest, the normalized minute data and their roll-up manifest, plus a deterministic normalization script — **prove** full transparency and reproducibility of the minute data used in the backtest.

---

<a id="step-4"></a>
## ⓸ Step 4. Verify the Minute-Data Gap Analysis and the Data Quality Policy v1.0

### Verifying the minute-data analysis

The minute-data analysis classifies gaps that are multiples of 5-minute bars using the original TXT reports that accompany HistData minute CSVs.  
The analysis artifacts are in **data-hub** at [`data_quality_analysis/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/data_quality_analysis/).  
The analysis was performed with a script published for full reproducibility in **data-preparation-toolkit** – [`minute_data_analyzer`](https://github.com/euro-macromechanica-backtest/data-preparation-toolkit/tree/main/minute_data_analyzer).

To improve transparency, the script generates a manifest enumerating inputs and outputs. Those set manifests are published together with the analysis artifacts. Additionally, a roll-up manifest listing the hashes of those manifests is provided in [`integrity/data_quality_analysis/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/integrity/data_quality_analysis/). The roll-up manifest carries a **GPG signature** and an **OTS anchor**.

### How to verify

Same as Step 3: use the **data-preparation-toolkit** script [`minute_data_analyzer`](https://github.com/euro-macromechanica-backtest/data-preparation-toolkit/tree/main/minute_data_analyzer) and the TXT reports from **data-hub** [`source_data/raw/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/source_data/raw).

> The script is also deterministic (though that isn’t strictly necessary), because a meticulous verification can analyze the TXT reports with your own scripts or other tools.

### Verifying the Data Quality Policy v1.0

To verify the data-quality policy under which the full period was split into **Stress**, **Extended Baseline**, and **Core Baseline**, verify the **SHA-256 hash**, **GPG signature**, and **OTS anchor** for the policy CSV located in the **results** repo at:  
[`integrity/data_quality_policy/data_quality_table_2001-2025-08.csv.sha256(.asc/.ots)`](https://github.com/euro-macromechanica-backtest/results/blob/main/integrity/data_quality_policy).

The **OTS anchor** shows the actual time of anchoring these data.

### Rationale & skeptic Q&A

The rationale for the split by data quality — and answers to skeptical questions such as “What if the anchoring was staged on the timeline?” — are described in the **Euro Macromechanica (EMM) Backtest – Overview & Methodology**.

### Bottom line

The presence of the original TXT reports on minute-data quality, the analysis results, and their roll-up manifest of hashes **confirm the transparency and reproducibility** of the chosen Data Quality Policy v1.0 for the M5 EMM backtest and the split into Stress, Extended Baseline, and Core Baseline.

---

<a id="step-5"></a>
## ⓹ Step 5. Verify Backtest Inputs and Results

To verify the backtest result files, make sure the hashes of the files **used as inputs** from **data-hub** [`integrity/source_data/prepared/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/integrity/source_data/prepared) match the hashes **listed in the yearly manifests** of the results (`results/*data_quality_mode*/*cost_model*/*risk*/*mode*/YYYY/artifacts_YYYY.sha256`).

**More on input binding:**  
- the pinned **PIN** in the results — [`results/docs/INPUTS-PIN.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.md). It also links to the canonical mapping of inputs from **data-hub** — [`data-hub/INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md).  

**Inputs source:** all inputs are taken from **data-hub** — [`economic_calendars/prepared/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/economic_calendars/prepared) and [`source_data/prepared/`](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/source_data/prepared).

**What exactly to compare:** compare **SHA-256 hashes** of the files in those folders to the hashes listed in the yearly result manifests of the corresponding track — this provides detailed verification of input-to-output correspondence and reproducibility of the backtest outcomes. Also verify **GPG signatures** and **OTS** for the result manifests themselves.  
> For convenience, hashes of the input files — calendars and minute data — are listed in their own roll-up manifests, each with its own **GPG signature** and **OTS anchor**.

The backtest uses a **dynamic cost model** parameterized by **notional volume**.  
**Cost binding to mode and year.** Concrete cost values (spread, slippage) are defined **per profile/mode and calendar year** and are computed **from the average notional trade size** determined by the equity trajectory in that mode (given the set risk per trade).  
**Commission (profile-based).** Commission is defined by the profile (Institutional / Retail Rebate / Retail Standard) as a fixed rate per **round-turn** per 1 standard lot (100,000 EUR) and applied **proportionally to the actual trade size**. Commission, spread, and slippage are included in P&L.

Example profile rates:  
- Institutional — $5.5 per round-turn / standard lot  
- Retail Rebate — $5 per round-turn / standard lot  
- Retail Standard — $7 per round-turn / standard lot

> Unlike **spread/slippage**, commission is profile-based (a fixed round-turn rate per standard lot) and is charged **pro-rata to each trade’s actual notional volume**; it is **not derived from the table’s average notional volume** and **does not depend on the calendar year** (it only changes when the **profile** changes).  
> Details on the cost-model values are in the [`Euro Macromechanica (EMM) Backtest – Overview & Methodology`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md).

- Cost-model table (commission/spread/slippage) for all tracks — [`results/docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/cost_model/m5_emm_cost_model_v1.0.csv).  
- Table of average market costs by notional volume (benchmark: top-tier ECN FX brokers; filtered by M5 EMM trading settings) — [`results/docs/cost_model/eurusd_market_order_costs_ecn_round-turn_pips_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/cost_model/eurusd_market_order_costs_ecn_round-turn_pips_v1.0.csv).  
- **SHA-256 hashes**, **GPG signatures**, and **OTS anchors** — [`results/integrity/cost_model/`](https://github.com/euro-macromechanica-backtest/results/tree/main/integrity/cost_model/).

**Cryptographic pinning of parameters.** All cost values (commission, spread, slippage) used in the backtest are pinned in the SHA-256 manifest of the CSV table `m5_emm_cost_model_v1.0.csv`; the manifest is **GPG-signed** and **time-anchored** via **OpenTimestamps**. This guarantees that the parameters correspond to the specific profiles/modes and calendar years of the backtest and confirms that the published yearly run results and metrics were computed using exactly these values.

### Note on live runs

Live runs were performed on randomly selected years across different tracks to demonstrate **determinism**. The goal is to confirm the **immutability of the strategy logic** and **reproducibility** of the entire pipeline across the whole backtest horizon when using the cost values from `m5_emm_cost_model_v1.0.csv`. The same fixed archive version of the strategy is used for all periods; yearly runs vary only in the cost parameters, initial balance, and risk per trade.

Each video demonstrates that:
- before the run, the **SHA-256 hash** of the deterministic strategy archive matches the pre-published value;
- **SHA-256 hashes** of all input files are verified;
- a **GPG-signed commit** of the file [`docs/cost_model/m5_emm_cost_model_v1.0.csv`], marked as **Verified**, is shown; the parameters used in the run are cross-checked: **initial balance**, **per-trade risk**, and **costs** — for the corresponding **track** and **calendar year**;
- the run is executed on a fixed code/configuration, with no manual intervention;
- after completion, **SHA-256 hashes** of all output files are verified.

> The M5 EMM engine is configured to be **deterministic**. With fixed Python and dependency versions, an unchanged configuration, and **identical input files** (bit-for-bit; matching SHA-256), run results **match bit-for-bit**.

Links to the live-run videos (and the corresponding packages with **SHA-256 / GPG / OTS**) are provided in the [`Euro Macromechanica (EMM) Backtest – Overview & Methodology`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md).

### Bottom line

The cryptographic linkage of the backtest result manifests, the cost-model table, the published SHA-256 / GPG / OTS artifacts, and the live runs **jointly confirm** that the reported results were obtained and are reproducible with fixed cost parameters, and that the strategy logic remains **unchanged (no re-tuning)** across the entire backtest horizon and for all profiles / modes / calendar years.

---

<a id="metrics"></a>
## Metrics

The result summaries and metrics are computed from **non-public files**—detailed trade reports (with timestamps and prices) and time-aligned equity/drawdown series. Separate **SHA/GPG/OTS artifacts** are not provided for the metrics because they are reproducible from these underlying result files; **for verification**, it is sufficient to validate the integrity of the result files themselves (see the yearly manifests) and recompute the metrics with the **open-source** script. For transparency, the **metrics calculation methodology** and the calculator script are published in [**metrics-toolkit**](https://github.com/euro-macromechanica-backtest/metrics-toolkit). The methodology is also mirrored in this repo’s `docs/` directory for convenience. Metrics for each track are published under the corresponding **capitalization mode**.

### Full methodology and metric definitions
See [`docs/metrics_schema.json`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_schema.json) and [`docs/metrics_schema.md`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_schema.md).  
Duplicated **methodology** files and the calculator script are published in the [`metrics-toolkit`](https://github.com/euro-macromechanica-backtest/metrics-toolkit) repository for transparency.

<a id="engine"></a>
## Proprietary M5 EMM Strategy Engine Code

A [**SHA-256, GPG signature, and OTS anchor**](https://github.com/euro-macromechanica-backtest/results/tree/main/strategy_existence_evidence) of a deterministic archive of the backtest strategy engine code are provided to **verify** the existence of the exact logic implemented in code. In commercial engagements or other cases where attestation is required, the original strategy engine used for the entire M5 EMM backtest project can be **easily verified** via this **cryptographic combination**.

---

<a id="links"></a>
## 📎 Key Links

- [**Euro Macromechanica (EMM) Backtest — Overview & Methodology**](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md) 
- [**Verifying SHA-256, GPG signatures, and OTS**](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INTEGRITY.md)
- [**data-hub**](https://github.com/euro-macromechanica-backtest/data-hub/)  
- [**data-preparation-toolkit**](https://github.com/euro-macromechanica-backtest/data-preparation-toolkit)  
- [**metrics-toolkit**](https://github.com/euro-macromechanica-backtest/metrics-toolkit)     
- [**backtest results**](https://github.com/euro-macromechanica-backtest/results/tree/main/results)
- [**SHA-256, GPG signature, and OTS anchor** of a deterministic archive of the backtest strategy engine code](https://github.com/euro-macromechanica-backtest/results/tree/main/strategy_existence_evidence)
- **Commercial engagements and collaborations** — [COMMERCIAL.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/COMMERCIAL.md)
