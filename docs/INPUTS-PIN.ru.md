# 📌 PIN — привязка входных данных к результатам Euro Macromechanica (EMM) Backtest

Этот файл фиксирует, **какими именно входами** пользовались годовые прогоны в репозитории **results**,  
и отсылает к каноничным артефактам происхождения/целостности в репозитории **data-hub**.

---

## 🧭 Привязка входных данных — охват и источники

**Входы для headline/stress-прогонов** берутся из репозитория данных **data-hub**.

Минимальный набор для воспроизводимости:
- **Prepared M1 (UTC-нормализованные минутные данные)** — архив-снимок (SHA-256/GPG/OTS).  
- **Economic calendars (UTC-метки публикаций)** — архив-снимок (SHA-256/GPG/OTS).  

> **Каноничное описание связки входов см. в *Data Hub*: [`INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md). Русская версия — [`INPUTS-PROVENANCE.ru.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.ru.md).**

---

## 🧾 Артефакты целостности в репозитории *results*

Зеркальные **roll-up-манифесты** входов (SHA-256-списки хэшей) плюс их **GPG-подписи** и **OTS-доказательства**:

```
integrity/
  inputs/
    eurusd_prepared_2001-2025-08.sha256
    eurusd_prepared_2001-2025-08.sha256.asc
    eurusd_prepared_2001-2025-08.sha256.ots
    economic_calendars_prepared_2001-2025-08.sha256
    economic_calendars_prepared_2001-2025-08.sha256.asc
    economic_calendars_prepared_2001-2025-08.sha256.ots
    gbpusd_prepared_2008-2025-08.sha256
    gbpusd_prepared_2008-2025-08.sha256.asc
    gbpusd_prepared_2008-2025-08.sha256.ots

keys/
  emm_pub_key.asc        # публичный ключ для GPG-проверки
```

---

## 🔗 Ссылки

- Каноничная схема привязки входов **data-hub**:  
  [`data-hub/INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md). Русский перевод — [`data-hub/INPUTS-PROVENANCE.ru.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.ru.md)
- Гайд по проверке целостности **results**:  
  [`docs/INTEGRITY.ru.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INTEGRITY.ru.md)
- Полный гайд по аудиту:  
  [`docs/AUDIT.ru.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.ru.md)
- Годовые манифесты результатов:  
  `results/**/artifacts_YYYY.sha256` (+ `.asc`, `.ots`)

---

## 📝 Примечания

- Этот **PIN** — краткая «скрепка» между **results** и **data-hub**.  
  Каноничными для входов считаются артефакты в *data-hub*; здесь хранится их зеркальная копия для удобства.
- Годовые манифесты результатов не изменяются под этот PIN.  
  Они уже перечисляют **точные файлы входов** (prepared-минутки, календари), по которым проходит верификация.
