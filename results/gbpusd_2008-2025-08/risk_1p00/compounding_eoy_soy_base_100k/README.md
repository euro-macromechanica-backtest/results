# Euro Macromechanica (EMM) M5 Engine — GBPUSD Cross Asset Test (2008–2025-08) — (risk 1%, no cost model) – Compounding EoY-SoY Base 100k

<p align="center">GBPUSD Cross Asset Test – Balance Curve — Compounding EoY-SoY Base 100k Mode (Risk 1%, no cost model) 2008–2025-08</p>

<p align="center"><img src="../../../../docs/assets/GBPUSD_2008-2025-08.png" alt="EMM M5 — GBPUSD Cross Asset Test 2008-2025-08 — Balance Curve" width="100%"></p>

## 🧾 Track Description

This track records the backtest results of the **M5 EMM strategy** on **GBPUSD**, **without cost modeling** and **without any parameter changes**. Capitalization mode — **compounding over the whole period (EoY→SoY)**. Balance carries over year to year. **Ending balance → initial balance** of the next year. Initial balance at the start of the period — 100k. Per‑trade risk — **1% of balance at entry**.

- Data range: **Core Baseline 2008-01 – 2025-08** (coverage: **212 months without gaps = 17 years 8 months**)
- Instrument/TF: **GBPUSD**, signal logic on **M5**
- **Backtest time zone:** **UTC+0** (all timestamps in UTC+0)
- Cost model: not applied

---

## ✅ Objective

### Cross-Asset Test on GBPUSD

To demonstrate model robustness and the absence of overfitting, a cross-asset test was performed: parameters optimized on EURUSD were applied to GBPUSD *without any adjustments for UK-specific economic data or volatility patterns*.

Historically, EURUSD and GBPUSD were highly correlated, but after **Brexit (2016)** this correlation dropped significantly. This makes GBPUSD an **independent out-of-sample** test: the asset was not involved in optimization and has a materially different post-Brexit market structure.

Stable performance on GBPUSD confirms:
- absence of overfitting,
- no multiple testing or selection bias,
- the model’s ability to generalize without cherry-picking.

---

## 📎 Links

- **Euro Macromechanica (EMM) Backtest — Overview and Methodology**: repository root **[README.md](https://github.com/euro-macromechanica-backtest/results/blob/main/README.md)**
- Full audit guide: **[docs/AUDIT.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.md)**