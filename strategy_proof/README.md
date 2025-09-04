# 🛡️ Strategy Proofs & Public‑Safe Mechanics  
*(this folder contains integrity proofs for a proprietary archive; the strategy itself is not published)*

---

## What this folder includes
- **Purpose:** proofs of existence and integrity for a closed strategy archive.  
- **Files:** `*.sha256` (hash manifests), `*.asc` (GPG signatures), `*.ots` (OpenTimestamps).  
- **Important:** the strategy code/parameters are **not** published here.

### Keys
- **Public key:** `keys/emm_pub_key.asc` (ASCII‑armored)  
- **GPG fingerprint:** `4E53 DA03 D1A1 644E 6479  3C47 EEEC 0AFA 4D8A F0A7`

---

## ✅ Quick check

### Linux / macOS / Windows PowerShell
```bash

# Verify the manifest signature
gpg --verify artifacts.sha256.asc artifacts.sha256

# Verify the time anchor (Bitcoin)
ots verify artifacts.sha256.ots artifacts.sha256
# optionally upgrade the .ots to a confirmed Bitcoin block:
# ots upgrade artifacts.sha256.ots
# or
ots info artifacts.sha256.ots # - check bitcoin block mannually
```

---

## ⚙️ Entry/Exit Logic (M5 with M1 micro‑confirmation) — public simplified summary

**Idea.** For each M5 bar the engine deterministically computes direction‑specific **service levels**:
- **Trigger** — initiates entry;  
- **Guard** — protective stop;  
- **Objective** — target.

**Entries are micro‑confirmed** on M1 within the current M5 bar’s window.

**Per‑bar sequence**
1) **Window:** all M1 minutes inside the current M5 bar.  
2) **Pre‑filters:** macroeconomic events, session masks, data‑hygiene heuristics (thin bars, post‑gap warm‑up); temporary post‑anomaly blocks.
   - **Data‑quality caveat:** when a year exhibits **many 5–10‑minute gaps**, the reliability of **Trigger/Guard/Objective** computations degrades; such years are **excluded from headline** per the Data Quality Policy. **Longer gaps** (>10 min) also matter, but are **far less material overall** for an M5 engine than frequent 5–10‑minute gaps.  
3) **Readiness:** both **Guard** and **Objective** must be present; otherwise no entry.  
4) **Entry trigger (via M1):** a touch of **Trigger** activates the entry; if multiple triggers are eligible, pick the most stringent per the internal order.  
5) **Position management:**  
   **SL:** fixed at **Guard** (no trailing);  
   **TP:** at **Objective**, recomputed each new M5 bar. Fills are checked on M1; if SL and TP collide within the same minute, **SL takes precedence**.  
6) **Force exit:** **only at the very end of the dataset** — if data ends and a position is still open, it is closed on the **last available minute**. No other forced exits are used.

### ⛓️ Gaps & exits (concise)
- **Positions are not auto‑closed across gaps.** Over discontinuities the trade remains open and continues on the next available bar.  
- **Exit evaluation on M1** inside the next M5 bar:  
  - **LONG:** TP at `high ≥ Objective`, SL at `low ≤ Guard`;  
  - **SHORT:** mirror (TP at `low ≤ Objective`, SL at `high ≥ Guard`).  
- **Fill price** is the **exact computed level** (Objective/Guard); the **timestamp** is the first M1 minute where reachability is met. M1 high/low are used for **detection**, not as the trade price.  
- **Same‑minute conflict:** if both target and stop conditions are met, **SL takes precedence**.  
- The **“anomalous‑gap” detector** only **blocks new entries** for a configured period; it **does not** close an open position.

### 💼 Sizing, accounting & public cost model
- **Position size** = a fraction of equity at entry; risk derives from the distance to **Guard**.  
- Trades store `pnl_abs` and `pnl_pct`; equity is built at **exit** timestamps.  
- **Cost model: Commission is configurable at run time. Default is fixed $7 round-turn per 1 standard lot (100,000 EUR) — typical retail ECN level; ≈ $3.50 per side, ≈ 0.7 pip on EURUSD. Applied uniformly in the backtest.**

> This is a public summary. **Numeric thresholds, exact formulas, and time windows are intentionally omitted.**  
> Headline/stress inclusion is governed by the **Data Quality Policy** (primary gate: frequency of **5–10‑minute** gaps).

---

## 📜 Licensing & access

- **This folder:** *CC BY‑NC 4.0* (integrity artifacts & documentation).  
- **Detailed per-trade reports (timestamps/prices) and time-indexed equity/drawdown series** are **not provided**.
- **Strategy code:** Proprietary / All Rights Reserved. Not included.
See [`STRATEGY-CODE-NOTICE.txt`](https://github.com/euro-macromechanica-backtest/results/blob/main/STRATEGY-CODE-NOTICE.txt) and [`COMMERCIAL.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/COMMERCIAL.md).

**Contact:** GitHub **@rleydev (thelaziestcat)** · email **thelaziestcat@proton.me** / **thelazyazzcat@gmail.com**
