<p align="center">Balance Curve — Compounding EoY-SoY Base 100k Mode (Risk 1.5%, $5 round-turn per standard lot, M5 EMM cost model v1.0) 2003–2025-08</p>

<p align="center"><img src="../../../../../docs/assets/2003-2025-08_5usd_balance_curve_comp.png" alt="EMM M5 — Retail Rebate — Composite Baseline 5 USD/lot — Balance Curve" width="100%"></p>

# Euro Macromechanica (EMM) M5 Engine — Composite Baseline (2003–2025-08) — Retail Rebate (5 USD/lot, risk 1.5%) – Compounding EoY-SoY Base 100k

## 🧾 Описание трека

Этот трек фиксирует результаты бэктеста стратегии M5 EMM при **Retail Rebate** комиссионных издержках: **5 USD за round-turn на 1 стандартный лот (100 000 EUR)**, эквивалент **≈0.5 pips** на EURUSD, **динамическая модель издержек (spread & slippage) M5 EMM cost model v1.0**. Режим капитализации — **компаундинг на всём периоде (EoY→SoY)**. Капитал переходит из года в год. **Ending balance → initial balance** следующего года. Старт initial balance в начале периода — 100k. Риск на сделку — **1.5% от баланса на момент входа**.

- Диапазон данных: **Composite Baseline 2003-01 – 2025-08** (покрытие: **272 мес. без дыр = 22 года 8 мес.**)
- Инструмент/TF: **EURUSD**, сигнальная логика на **M5**
- **Часовой пояс бэктеста:** **UTC+0** (все временные метки в UTC+0)
- Модель издержек: комиссия, spread и slippage **включены** в PnL

---

## 📈 Баланс по закрытию года `compounding_eoy_soy_base_100k`

| Год | баланс на момент закрытия года (UTC+0) | процент на момент закрытия года (округление — 5 знаков после запятой) |
|---|---:|---:|
| 2003 | 112857.03606 | +12.85704% |
| 2004 | 126465.25193 | +12.05792% |
| 2005 | 119621.50165 | −5.41157% |
| 2006 | 125626.14681 | +5.01970% |
| 2007 | 126135.19998 | +0.40521% |
| 2008 | 154332.59106 | +22.35489% |
| 2009 | 197051.03761 | +27.67947% |
| 2010 | 288369.84478 | +46.34272% |
| 2011 | 323116.99399 | +12.04951% |
| 2012 | 321829.00907 | −0.39861% |
| 2013 | 334672.75517 | +3.99086% |
| 2014 | 336412.99798 | +0.51998% |
| 2015 | 444812.66836 | +32.22220% |
| 2016 | 524943.73053 | +18.01456% |
| 2017 | 568592.87943 | +8.31501% |
| 2018 | 588717.24109 | +3.53933% |
| 2019 | 595879.32993 | +1.21656% |
| 2020 | 700406.21482 | +17.54162% |
| 2021 | 711730.35138 | +1.61680% |
| 2022 | 766232.55264 | +7.65770% |
| 2023 | 729611.64143 | −4.77935% |
| 2024 | 750123.38267 | +2.81132% |
| 2025-08 | 772451.30878 | +2.97657% |

### Результат за 22 года 8 мес. ~ +672451.31 USD / +672.45%

---

## 🧾 Модель издержек

- **Комиссия:** 5 USD за round-turn на 1 стандартный лот (100k EUR)  
- **Модель издержек (commission, spread, slippage) M5 EMM cost model v1.0** — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv).
- Все издержки **включены** в PnL.

> Подробности о динамической модели издержек описаны в [`Обзор и Методология Euro Macromechanica (EMM) Backtest`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.ru.md)

---

## 📊 Краткий обзор — Retail Rebate 5 USD/lot, `compounding_eoy_soy_base_100k`, risk 1.5%

### Full period summary
- **CAGR 9.44%** при годовой волатильности **7.86%**; соотношение риск/доход — **Sharpe 1.19**, **Sortino 2.40**, **MAR (Full period Calmar) 1.24**.
- Просадки (по непрерывной кривой): **EoM MaxDD −7.64%**; время восстановления — **26 мес.**; внутримесячно глубже (**−12.78%**), **TTR — 2 мес.**. Длительность под водой: **EoM 39 мес.**, **Intramonth 42 мес.**.
- Месячная премия: средний/медианный месяц **0.78% / 0.42%**.
- Календарная устойчивость: лучший год **2010 (46.34%)**, худший **2005 (−5.41%)**; «нулевых» месяцев **41**.
- Объём выборки: покрыто **22 года 8 месяцев** (2003‑01—2025‑08), **272** мес.; количество сделок: **1785**.
- Стресс‑ориентиры: **EoM MaxDD ≈ −7.64%**, **Intramonth MaxDD ≈ −12.78%**; ориентир ожиданий — **средний месяц ≈ 0.78%**.
> **Итог:** устойчивый, «ровно‑растущий» профиль с умеренной волатильностью и ограниченными EoM‑просадками; внутримесячные колебания глубже, но быстро отыгрываются. Премия по месяцу невелика, однако стабильна на длинной выборке; устойчивость формируется за счёт частоты положительных периодов и дисциплины риск‑менеджмента.

### Trades summary
- Объём выборки: **1785** сделок; win rate **73.17%**.
- Качество профиля: **Profit factor 1.39**, **Payoff 0.51** (avg win/|avg loss|).
- Ожидание на сделку: **mean 0.08 R**, **median 0.33 R**.
- Распределение R: **σ ≈ 0.55 R**, **min -1.02 R**, **max 0.59 R**.
- Средние результаты: **avg win 0.38 R**, **avg loss -0.74 R**.
> **Итог:** положительная ожидаемая доходность при высокой доле прибыльных сделок и payoff ниже 1 — профиль «частые небольшие выигрыши, более редкие по частоте, но крупнее по размеру убытки»; устойчивость обеспечивается строгим контролем риска.

### Yearly summary
- Покрытие: **22 года 8 месяцев** (2003‑01—2025‑08; календарно охвачены **2003–2025**, 2025 — неполный). Средний/медианный календарный год: **9.94% / 5.02%**.
- Лучший год: **2010 (46.34%)**; худший год: **2005 (-5.41%)**.
- Просадки (внутри года, от пика): **EoM -7.12% → 0.00%**, **Intramonth -12.78% → -0.34%**.
- Торговая активность: всего сделок за годы **1785**; средние по годам — win rate **71.41%**, PF **1.68**.
> **Итог:** по годам динамика остаётся ровной: положительное среднее без крайностей; внутригодовые просадки ограничены. Торговые показатели по годам подтверждают устойчивость профиля.

### Monthly returns
- Покрытие: **272** месяцев (2003‑01—2025‑08). Средний/медианный месяц: **0.78% / 0.42%** (P10/P90: **−1.55% / 3.21%**).
- Симметрия: положительных месяцев **161**, отрицательных **70**, нулевых **41**.
- Экстремальные значения: лучший месяц **2010-05 (13.17%)**, худший месяц **2008-09 (−5.62%)**.
- Серии по месяцам: максимальная серия выигрышей — **12** месяцев подряд, максимальная серия убытков — **4** месяца подряд; месяцы с нулевой доходностью прерывают любую серию.
> **Итог:** небольшая и стабильная месячная «премия»; высокая доля положительных месяцев без длительных убыточных серий.

### Вывод
Трек специально собран как «длинная витрина» стабильности: на горизонте 22 года 8 месяцев, при смешанном качестве данных и фиксированном риске ~1.5% на сделку кривая капитала идёт ровно вверх с умеренной волатильностью; EoM-просадки остаются неглубокими, внутримесячные колебания заметнее, но, как правило, быстро отыгрываются, а длительные периоды «под водой» ограничены по глубине. Месячная премия невелика, зато повторяема, что отражается и в годовой динамике без крайних всплесков и провалов. Профиль сделок с высокой долей попаданий и payoff ниже единицы делает ставку на частоту небольших выигрышей при строгом контроле потерь — за счёт этого формируется устойчивое положительное ожидание и предсказуемое соотношение риск/доход даже в «грязной» смеси рыночных режимов. В сумме это демонстрирует именно то, ради чего собран трек: устойчивость поведения на очень длинном интервале при умеренно повышенном риске на сделку.

### Полная методология и определения метрик в [`docs/metrics_methodology/metrics_schema.json`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json) / [`docs/metrics_methodology/metrics_schema.md`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md).

### Файлы метрик

```
metrics/
  monthly_returns.csv
  full_period_summary.csv
  yearly_summary.csv
  trades_full_period_summary.csv
```

### Метрики рассчитывались на основе непубличных файлов `trades_YYYY.csv` и `balance_YYYY.csv`

---

## 📎 Ссылки

- **Обзор и Методология Euro Macromechanica (EMM) Backtest**: корневой **[README.ru.md](https://github.com/euro-macromechanica-backtest/results/blob/main/README.ru.md)**
- Общая информация о содержимом в `results`: **[results/README.ru.md](https://github.com/euro-macromechanica-backtest/results/blob/main/results/README.ru.md)**
- Модель издержек (commission, spread, slippage) M5 EMM cost model v1.0 — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv)
- Входные данные и происхождение: **[INPUTS-PIN.ru.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.ru.md)** / **[INPUTS-PROVENANCE.ru.md](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.ru.md)**
- Полная инструкция по аудиту: **[docs/AUDIT.ru.md](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.ru.md)**
- Политика качества данных: **[data_quality_policy/policy_v1.0.ru.md](https://github.com/euro-macromechanica-backtest/results/blob/main/data_quality_policy/policy_v1.0.ru.md)**
- Методология расчёта метрик: **[docs/metrics_methodology/metrics_schema.md](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.md)** / **[docs/metrics_methodology/metrics_schema.json](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_methodology/metrics_schema.json)**