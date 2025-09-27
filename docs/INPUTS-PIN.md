# 📌 PIN - Binding Inputs to Results for the Euro Macromechanica (EMM) Backtest 

This note specifies **exactly which inputs** the annual runs in the **results** repository rely on
and points to the **canonical provenance & integrity artifacts** kept in **data-hub**.

---

## 🧭 What this binds

**Inputs for headline and stress runs** come from the **data-hub** repository.

Minimum set for reproducibility:
- **Prepared M1 (UTC‑normalized minute data)** — point‑in‑time snapshot (SHA‑256 / GPG / OTS).  
- **Economic calendars (UTC publication timestamps)** — point‑in‑time snapshot (SHA‑256 / GPG / OTS).  

> **For the canonical description of how inputs are bound, see *Data Hub*: [`INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md)**

---

## 🧾 Integrity Artifacts Stored in This Repository

Mirrored copies of the input roll-up manifests (SHA-256 lists of all relevant files), together with their GPG signatures and OpenTimestamps proofs:
```
integrity/
  inputs/
    eurusd_prepared_2001-2025-08.sha256
    eurusd_prepared_2001-2025-08.sha256.asc
    eurusd_prepared_2001-2025-08.sha256.ots
    economic_calendars_prepared_2001-2025-08.sha256
    economic_calendars_prepared_2001-2025-08.sha256.asc
    economic_calendars_prepared_2001-2025-08.sha256.ots
keys/
  emm_pub_key.asc        # public key for GPG verification
```

---

## 🔗 Links

- Canonical input‑binding scheme in **data‑hub**:  
  [`data-hub/INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md)
- Integrity verification guide for **results**:  
  [`docs/INTEGRITY.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INTEGRITY.md)
- Full audit guide:  
  [`docs/AUDIT.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.md)
- Yearly result manifests:  
  `results/**/artifacts_YYYY.sha256` (+ `.asc`, `.ots`)

---

## 📝 Notes

- This **PIN** is a short “bridge” between **results** and **data‑hub**.  
  The *canonical* input artifacts live in **data‑hub**; this repo keeps a mirror copy for convenience.
- Yearly result manifests are **not** altered by this PIN.  
  They already enumerate the **exact input files** (prepared minutes, calendars) used for verification.
