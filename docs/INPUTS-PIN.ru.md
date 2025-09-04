# 📌 PIN — привязка входных данных к результатам (Euro Macromechanica / EMM)

Этот файл фиксирует, **какими именно входами** пользовались годовые прогоны в этом репозитории **results**,  
и отсылает к каноничным артефактам происхождения/целостности в репозитории **data-hub**.

---

## 🧭 Что привязано

**Входы для headline/stress‑прогонов** берутся из репозитория данных:  
**data-hub**.

Минимальный набор для воспроизводимости:
- **Prepared M1 (UTC, очищенные минутки)** — архив‑снимок (SHA‑256/GPG/OTS).  
- **Economic calendars (UTC публикаций)** — архив‑снимок (SHA‑256/GPG/OTS).  

> **Каноничное описание связки входов см. в *Data Hub*: [`INPUT-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md). Ру версия [`INPUT-PROVENANCE.ru.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.ru.md).**

---

## 🧾 Где лежат артефакты в этом репозитории results

Зеркальные копии хэшей/подписей/OTS для входных **архивов‑снимков**:
```
integrity/
  inputs/
    eurusd_prepared_2001-2025-07.tar.gz.sha256
    eurusd_prepared_2001-2025-07.tar.gz.sha256.asc
    eurusd_prepared_2001-2025-07.tar.gz.sha256.ots
    calendars_2001-2025-07.tar.gz.sha256
    calendars_2001-2025-07.tar.gz.sha256.asc
    calendars_2001-2025-07.tar.gz.sha256.ots
keys/
  emm_pub_key.asc        # публичный ключ для GPG‑проверки
```
> Публичный ключ и его отпечаток приведены также в **docs/`INTEGRITY.ru.md`** этого репозитория.

---

### Mirrored anchors (for convenience) — canonical in data-hub

**Prepared minute data (archive snapshot)**  
- SHA-256: `e81f26072b48e62eb37dcd4da9919608e05da871b7264d3718e01244a2658d5f`  
- GPG: `integrity/prepared/eurusd_prepared_2001-2025-07.tar.gz.sha256.asc`  
- OTS:  `integrity/prepared/eurusd_prepared_2001-2025-07.tar.gz..sha256.ots`  
- Bitcoin: txid `<3646771ecc29d275b1b80ead9d5866a3b8867e6488df8d42145e2824a06e2a71>`, block `<910536>`, time (UTC) `2025-08-18T02:06:26Z`  

**Economic calendars (archive snapshot)**  
- SHA-256: `0bd28458c49affc3b052116279827a8c840edf2dc4295d57a0b7e2a7b0b2e045`  
- GPG: `integrity/economic_calendars/calendars_2001-2025-07.tar.gz..sha256.asc`  
- OTS:  `integrity/economic_calendars/calendars_2001-2025-07.tar.gz..sha256.ots`  
- Bitcoin: txid `a95e00f437575dad58c5e074352b6f4c9637ace705eccb05aa936e1cb99c6ffb`, block `910177`, time (UTC) `2025-08-15T16:31:10Z`

---

## ✅ Быстрая проверка (60 секунд)

1) **SHA‑256:**  
   Скачайте архивы входов из data-hub **или** используйте локальные копии, затем:
   ```bash
   sha256sum <archive>     # macOS: shasum -a 256 <archive>
   # сравните с содержимым integrity/inputs/<name>.sha256
   ```

2) **GPG:**  
   ```bash
   gpg --import keys/emm_pub_key.asc
   gpg --verify integrity/inputs/<name>.sha256.asc integrity/inputs/<name>.sha256
   ```

3) **OpenTimestamps (OTS):**  
   ```bash
   ots verify --no-bitcoin integrity/inputs/<name>.sha256.ots integrity/inputs/<name>.sha256
   ots info                integrity/inputs/<name>.sha256.ots   # высота блока / txid
   ```
> Момент времени фиксируйте в **UTC**: некоторые обозреватели показывают локальную зону.

---

## 🔗 Ссылки

- Каноничная схема привязки входов **dat-hub**:  
  [`data-hub/INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md). Ру перевеод – [`data-hub/INPUTS-PROVENANCE.ru.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.ru.md)
- Гайд по проверке целостности **results**:  
  [docs/`INTEGRITY.ru.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INTEGRITY.ru.md) (заметка про абсолютные пути в манифестах)
- Годовые манифесты результатов:  
  `results/**/artifacts_YYYY.sha256` (+ `.asc`, `.ots` для headline)

---

## 📝 Примечания

- Этот **PIN** — краткая «скрепка» между **results** и **data-hub**.  
  Каноничными для входов считаются артефакты в *data-hub*; здесь хранится их зеркальная копия для удобства.
- Годовые манифесты результатов не изменяются под этот PIN.  
  Они уже перечисляют **точные файлы входов** (prepared минутки, календари), по которым проходит верификация.
- Если в манифестах встречаются **абсолютные пути**, это **не влияет** на корректность хэшей.  
  Для локальной проверки можно «перебазировать» пути на относительные — см. **docs/`INTEGRITY.ru.md`**.
