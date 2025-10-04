<p align="center">Balance Curve — Fixed Start 100k Mode (Risk 1%, $7 round-turn per standard lot, M5 EMM cost model v1.0) 2003–2007</p>

<p align="center"><img src="../../../../../docs/assets/2003-2007_7usd_balance_curve_fix.png" alt="EMM M5 — Retail Standard — Extended Baseline 7 USD/lot — Balance Curve" width="100%"></p>

# Euro Macromechanica (EMM) M5 Engine — Extended Baseline (2003–2007) — Retail Standard (7 USD/lot, risk 1%) – Fixed Start 100k

## 🧾 Описание трека

Этот трек фиксирует результаты бэктеста стратегии M5 EMM при **Retail Standard** комиссионных издержках: **7 USD за round-turn на 1 стандартный лот (100 000 EUR)**, эквивалент **≈0.7 pips** на EURUSD, **динамическая модель издержек (spread & slippage) M5 EMM cost model v1.0**. Режим капитализации — **ежегодный ресет к 100 000 USD**. Риск на сделку — **1% от баланса на момент входа**.

- Диапазон данных: **Extended Baseline 2003-01 – 2007-12** (покрытие: **60 мес. без дыр = 5 лет**)
- Инструмент/TF: **EURUSD**, сигнальная логика на **M5**
- **Часовой пояс бэктеста:** **UTC+0** (все временные метки в UTC+0)
- Модель издержек: комиссия, spread и slippage **включены** в PnL
- Базовый NAV для ребейза: **100 000 USD** (`fixed_start_100k` — ежегодный ресет к 100k)

---

## 📈 Баланс по закрытию года `fixed_start_100k`

| Год | баланс на момент закрытия года (UTC+0) | процент на момент закрытия года (округление — 5 знаков после запятой) |
|---|---:|---:|
| 2003 | 107707.83975 | +7.70784% |
| 2004 | 107140.76118 | +7.14076% |
| 2005 | 96006.44707 | −3.99355% |
| 2006 | 103070.43943 | +3.07044% |
| 2007 | 100169.26886 | +0.16927% |

### Результат за 5 лет ~ +14094.76 USD / +14.09%

---

## 🧾 Модель издержек

- **Комиссия:** 7 USD за round-turn на 1 стандартный лот (100k EUR)  
- **Модель издержек (commission, spread, slippage) M5 EMM cost model v1.0** — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv).
- Все издержки **включены** в PnL.

> Подробности о динамической модели издержек описаны в [`Обзор и Методология Euro Macromechanica (EMM) Backtest`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.ru.md)

---

## 📊 Краткий обзор — Retail Standard 7 USD/lot, `fixed_start_100k`, risk 1%

### Full period summary
- **CAGR 2.72%** при годовой волатильности **3.79%**; соотношение риск/доход — **Sharpe 0.73**, **Sortino 1.24**, **MAR (Full period Calmar) 0.49**.
- Просадки (по непрерывной кривой): **EoM MaxDD −5.54%**; время восстановления — **не восстановилось (n/a)**; внутримесячно глубже (**−7.50%**), **TTR — не восстановилось (n/a)**. Длительность под водой: **EoM 38 мес.**, **Intramonth 38 мес.**.
- Месячная премия: средний/медианный месяц **0.23% / 0.21%**.
- Календарная устойчивость: лучший год **2003 (7.71%)**, худший **2005 (−3.99%)**; «нулевых» месяцев **4**.
- Объём выборки: покрыто **5** лет, **60** мес.; количество сделок: **342**.
- Стресс-ориентиры риск-менеджмента: **EoM MaxDD ≈ −5.54%**, **Intramonth MaxDD ≈ −7.50%**; ориентир по ожиданиям — **средний месяц ≈ 0.23%**.
> **Итог:** умеренный рост при низкой волатильности. Просадки неглубокие, но максимум не восстановлен в пределах окна наблюдений; периоды «под водой» могут тянуться до ~3 лет. Внутримесячные колебания заметно глубже EoM, но остаются в однозначных процентах. Доход формируется за счёт устойчивых небольших месячных приростов без экстремальных всплесков.

### Trades summary
- Объём выборки: **342** сделок; win rate **69.88%**.
- Качество профиля: **Profit factor 1.19**, **Payoff 0.51** (avg win/|avg loss|).
- Ожидание на сделку: **mean 0.04 R**, **median 0.28 R**.
- Распределение R: **σ ≈ 0.56 R**, **min -1.03 R**, **max 0.57 R**.
- Средние результаты: **avg win 0.37 R**, **avg loss -0.72 R**.
> **Итог:** положительное матожидание достигается за счёт высокой доли прибыльных сделок при **payoff < 1** (средняя прибыль меньше средней потери). Профиль близок к «часто-маленькие выигрыши vs реже-более крупные убытки»; устойчивость поддерживается частотой попаданий и контролем риска на сделку.

### Yearly summary
- Покрытие: **5** лет (2003–2007). Средний/медианный календарный год: **2.82% / 3.07%**.
- Лучший год: **2003 (7.71%)**; худший год: **2005 (-3.99%)**.
- Просадки (внутри года, от пика): **EoM -4.70% → -1.08%**, **Intramonth -5.54% → -2.23%**.
- Торговая активность: всего сделок за годы **342**; средние по годам — win rate **69.55%**, PF **1.19**.
> **Итог:** на годовом горизонте результаты умеренные и ровные: средний календарный год ~**2.82%** с ограниченными внутригодовыми просадками (обычно однозначные проценты). Лучшие и худшие годы остаются в умеренных пределах; удержание положительного среднего достигается сочетанием приемлемой частоты прибыльных месяцев и контролируемой глубины просадок.

### Monthly returns
- Покрытие: **60** месяцев (2003–2007). Средний/медианный месяц: **0.23% / 0.21%** (P10/P90: **-1.09% / 1.64%**).
- Симметрия: положительных месяцев **37**, отрицательных **19**, нулевых **4**.
- Экстремальные значения: лучший месяц **2004-02 (2.92%)**, худший месяц **2005-07 (-2.92%)**.
- Серии по месяцам: максимальная серия выигрышей — **10** месяцев подряд, максимальная серия убытков — **4** месяца подряд; месяцы с нулевой доходностью прерывают любую серию.
> **Итог:** поведение месячных доходностей соответствует профилю с умеренной премией и ограниченными хвостами (P10/P90 в узком диапазоне). Устойчивость поддерживается частотой положительных месяцев и отсутствием затяжных убыточных серий.

### Cash flows (USD)
- Покрытие: событий **4** за годы 2003–2006.
- Денежные потоки: **внесения (cash-in)** 3,993.55 (1 событие), **снятия (cash-out)** 17,919.04 (3 события)
- **Прибыль за последний год** 169.27.
> **Итог:** чистая прибыль ~ **14094.76**.

### Вывод
В целом трек показывает умеренный, ровный рост при низкой изменчивости: просадки неглубокие, хотя восстановление пиков может растягиваться; месячная премия небольшая, но стабильная; годовая динамика без крайностей. Результат держится за счёт высокой доли прибыльных периодов при скромном **payoff**, что требует дисциплины риск-менеджмента. Внутримесячные колебания глубже, но остаются контролируемыми.

### Полная методология и определения метрик в [`docs/metrics_methodology/metrics_schema.json`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json) / [`docs/metrics_methodology/metrics_schema.md`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md).

### Файлы метрик

```
metrics/
  monthly_returns.csv
  full_period_summary.csv
  rebasing_applied.csv
  yearly_summary.csv
  trades_full_period_summary.csv
```

### Метрики рассчитывались на основе непубличных файлов `trades_YYYY.csv` и `balance_YYYY.csv`

---

## 📎 Ссылки

- **Обзор и Методология Euro Macromechanica (EMM) Backtest**: корневой **[README.ru.md](https://github.com/euro-macromechanica-backtest/results/blob/main/README.ru.md)**
- Модель издержек (commission, spread, slippage) M5 EMM cost model v1.0 — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv)
- Общая информация о содержимом в `results`: **[results/README.ru.md](https://github.com/euro-macromechanica-backtest/results/blob/main/results/README.ru.md)**
- Входные данные и происхождение: **[INPUTS-PIN.ru.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.ru.md)** / **[INPUTS-PROVENANCE.ru.md](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.ru.md)**
- Полная инструкция по аудиту: **[docs/AUDIT.ru.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.ru.md)**
- Политика качества данных: **[data_quality_policy/policy_v1.0.ru.md](https://github.com/euro-macromechanica-backtest/results/blob/main/data_quality_policy/policy_v1.0.ru.md)**
- Методология расчёта метрик: **[docs/metrics_methodology/metrics_schema.md](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md)** / **[docs/metrics_methodology/metrics_schema.json](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json)**