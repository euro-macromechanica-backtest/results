# 📊 Data Quality Policy (M1→M5, EMM intraday strategy on 5-minute bars)

Version: **v1.0**

This policy puts **5–15-minute gaps** in the M1 stream **front and center**.  
Other anomaly types are reported **for transparency** but are **not gating criteria**.

---

## 🧭 Definitions & anti–cherry-picking

- **Core Baseline (2008–…):** the primary, quality-consistent period **after a structural shift** in the feed (sharp drop in 5–15-minute gaps).
- **Extended Baseline (2003–2007):** earlier years added for historical breadth; **data quality differs from Core** (higher frequency of 5–15-minute gaps). Metrics are **reported separately**.
- **Stress (2001–2002):** years with systematically high gap levels; used **only** for stress illustration.

### Inclusion rules (fixed before publishing results)

- **Core rule (strict):** the baseline starts at the first year `t` such that  
  1) annual **5–15-minute gaps < 1000**, and  
  2) years `t+1` and `t+2` also have **< 1000**.  
  *The start rule is fixed **ex ante** (relative to PnL): the first year followed by two more years each with <1000 gaps. In today’s data that is **2008**; it’s a consequence of the rule, **not** a post-hoc choice.*

- **Extended rule (broad):** a year is in Extended Baseline if annual **5–15-minute gaps < 2500**.  
  *This covers 2003–2007, showing the strategy over a wider window **with lower data quality** than Core (more 5–15-minute gaps).*

> 🔒 Rules and windows are **cryptographically anchored** (SHA-256 / GPG / OpenTimestamps) **before** publishing results — see “Transparency & Precommitment.”

---

## 📈 Data quality — 5–15-minute gaps by year (EMM backtest on M5)

### Gap table for **EUR/USD M1 (source: HistData)** — by year

| Year    | Gaps 5–15 min | All anomalies (any length) | Track              |
|---------|----------------|-----------------------------|--------------------|
| 2001    | 3719           | 59870                       | Stress             |
| 2002    | 3009           | 51025                       | Stress             |
| 2003    | 1542           | 36445                       | Extended Baseline  |
| 2004    | 1062           | 36942                       | Extended Baseline  |
| 2005    | 1226           | 48494                       | Extended Baseline  |
| 2006    | 1923           | 57051                       | Extended Baseline  |
| 2007    | 2347           | 62222                       | Extended Baseline  |
| 2008    | 488            | 29410                       | **Core Baseline**  |
| 2009    | 277            | 22018                       | **Core Baseline**  |
| 2010    | 359            | 26829                       | **Core Baseline**  |
| 2011    | 202            | 20047                       | **Core Baseline**  |
| 2012    | 5              | 1855                        | **Core Baseline**  |
| 2013    | 3              | 2785                        | **Core Baseline**  |
| 2014    | 13             | 9259                        | **Core Baseline**  |
| 2015    | 2              | 1423                        | **Core Baseline**  |
| 2016    | 4              | 1580                        | **Core Baseline**  |
| 2017    | 3              | 1954                        | **Core Baseline**  |
| 2018    | 16             | 1995                        | **Core Baseline**  |
| 2019    | 1              | 484                         | **Core Baseline**  |
| 2020    | 26             | 1791                        | **Core Baseline**  |
| 2021    | 40             | 5670                        | **Core Baseline**  |
| 2022    | 20             | 1891                        | **Core Baseline**  |
| 2023    | 55             | 2753                        | **Core Baseline**  |
| 2024    | 42             | 3051                        | **Core Baseline**  |
| 2025-08 | 1              | 884                         | **Core Baseline**  |

*Note.* **Stress** is a **data-quality** track (2001–2002) and is **unrelated** to cost modes.

---

## 🧐 Why 5–15-minute gaps get priority

For M5 indicators, **frequent 5–15-minute gaps** break bar construction and entry/exit triggers.  
Therefore the **annual count of 5–15-minute gaps** is the key determinant of a year’s suitability for the baseline.  
Other anomalies are documented but **not** used as cutoffs.

---

## 📏 Reporting policy

- **Core and Extended metrics are computed separately** and **not pooled** into one set.  
- **Headline metrics are published only for Core Baseline.**  
- A *pooled view* is allowed as a **reference indicator over a long historical window** and must be labeled **mixed quality**; it is **not used for headline claims**.

---

## 📂 Current classification

- **⭐ Core Baseline:** 2008–2025-08  
- **📦 Extended Baseline:** 2003–2007  
- **🧪 Stress:** 2001–2002

Detailed table: `data_quality_policy/data_quality_table_2001-2025-08.csv`

---

## 🗂️ Reporting tracks

- ⚠️ **Stress (2001–2002)** — no metrics computed; illustrates behavior on low-quality data.
- ⭐ **Core Baseline (2008–2025-08)** — headline.
- 📦 **Extended Baseline (2003–2007)** — secondary; computed separately.
- 🧩 **Composite (Extended+Core, 2003–2025-08)** — a **reference pooled view over a long historical window** (mixed quality); not used for headline claims.

### Cost modes

Detailed cost models (commission, spread and slippage) for each regime are described in the [`results/results/README.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/results/README.md).

Regimes are named by their cost model:
- **Retail-Standard** — standard retail conditions;
- **Retail-Rebate** — retail with a commission rebate/cashback applied;
- **Institutional** — institutional conditions.

**All cost components for each regime are included in the PnL calculation.**

> Precommitment applies to the data-quality table only. Any future changes to costs will be released as separate results with a new cost-model identifier.

---

## 🔎 Where to find detailed gap-report analysis

See the data repo: `data-hub` → [data_quality_analysis/...](https://github.com/euro-macromechanica-backtest/data-hub/tree/main/data_quality_analysis)  
(summary tables of 5–15-minute gaps, by-date reports, SVG visualizations, integrity manifests)

---

## 🧾 Transparency & precommitment (provenance)

The primary artifact for **v1.0** is `data_quality_table_2001-2025-08.csv`, listing 5–15-minute gaps and the per-year track assignment.  
The table was **fixed and anchored** prior to publishing results. Artifacts live under `integrity/data_quality_policy/...`: **SHA-256 manifest**, **GPG signature**, **OpenTimestamps (OTS)**.

- **MANIFEST_SHA256:** `a0ed0dbdc1bd8031d6810942b59d3127a1c216ab1648489055a3a4a83ba90080` *(SHA-256 of `data_quality_table_2001-2025-08.csv.sha256`)*  
- **GPG signature:** `data_quality_table_2001-2025-08.csv.sha256.asc`  
- **OpenTimestamps:** `data_quality_table_2001-2025-08.csv.sha256.ots`  
- **Anchored (Bitcoin):** block **914872**,  
  **txid** `a81a68850e88125f22614a9777be32953710a2d6064c3d23f29e978457ef7ba5`,  
  **UTC timestamp** `2025-09-15T22:33:19Z`.

> Verification instructions for **SHA-256, GPG signatures, and OTS proofs**: see [`docs/INTEGRITY.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INTEGRITY.md).
> Detailed audit guide: see [`docs/AUDIT.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.md)