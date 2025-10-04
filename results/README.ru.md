# Euro Macromechanica (EMM) M5 Engine – Backtest Results

## ‼️ Обязательно к прочтению — [Обзор и Методология Euro Macromechanica (EMM) Backtest](https://github.com/euro-macromechanica-backtest/results/blob/main/README.ru.md)

---

## 📚 Периоды бэктеста по политике качества данных v1.0

- **Core Baseline** 2008–2025-08 — **`core_baseline_2008-2025-08/`** — основной интервал бэктеста по качеству **минутных** данных (17 лет 8 месяцев)
- **Extended Baseline** 2003–2007 — **`extended_baseline_2003-2007/`** — интервал с более низким качеством **минутных** данных (5 лет)
- **Stress** 2001–2002 — **`stress_2001-2002/`** — стресс-период (иллюстративно на деградированном **минутном** потоке котировок (M1); вне базового интервала *baseline*)
- **Composite Baseline (Extended + Core)** 2003–2025-08 — **`composite-baseline_extended-core_2003-2025-08/`** — иллюстративный интервал на данных смешанного качества (mixed quality) — справочный *pooled view*, публикуется как *reference*-показатель на длинном историческом горизонте. Покрывает практически весь период «retail-broker trading era» по евро и почти весь срок существования евро как единой валюты ЕС (с начала обращения наличных в 2002 г.). Из-за смешанного качества данных расширенный набор метрик не публикуется и для headline-утверждений трек не используется.

> Политика качества данных v1.0 — [`data_quality_policy/policy_v1.0.ru.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/data_quality_policy/policy_v1.0.ru.md)

---

## 🎛️ Модель издержек

### Модель издержек является динамической относительно notional volume на сделку и ориентирована на лучших ECN FX-брокеров. Подробности о модели издержек читать в [Обзор и Методология Euro Macromechanica (EMM) Backtest](https://github.com/euro-macromechanica-backtest/results/blob/main/README.ru.md)
**Модель издержек (commission, spread, slippage) M5 EMM cost model v1.0** — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv).
**Таблица издержек (spread, slippage) по notional volume** - [/docs/cost_model/eurusd_market_order_costs_ecn_round-turn_pips_v1.0](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/eurusd_market_order_costs_ecn_round-turn_pips_v1.0.csv).

### Режимы комиссий
- **Institutional** — *главный трек*: средняя институциональная комиссия **$5.5 за round-turn на 1 стандартный лот (100 000 EUR)**; эквивалент **≈0.55 pips на EURUSD** *(≈**$2.75** за сторону)* — ориентир для институциональных условий.
- **Retail Rebate** — усреднённая комиссия ритейл-счёта после кэшбэка **$5 за round-turn на 1 стандартный лот (100 000 EUR)**; эквивалент **≈0.5 pips на EURUSD** *(≈**$2.5** за сторону)* (режим с использованием сервиса кэшбэка на комиссии).
- **Retail Standard** — *иллюстрация обычной ритейл-комиссии*: **$7 за round-turn на 1 стандартный лот (100 000 EUR)**; эквивалент **≈0.7 pips на EURUSD** *(≈**$3.5** за сторону)*.

### Комиссия, spread и slippage каждой сделки учтены в PnL
Движок бэктеста учитывает издержки **на каждую сделку**.

---

## 📦 Структура и содержание

> Пример структуры:
> `core_baseline_2008-2025-08/retail_rebate/risk_1p00/fixed_start_100k/YYYY/(balance_YYYY.svg|summary_YYYY.csv|artifacts_YYYY.sha256)`

- `core_baseline_2008-2025-08` — период бэктеста по политике качества данных v1.0.
- `retail_rebate` — *profile* (профиль).
- `risk_1p00` — *risk* (размер риска).
- `fixed_start_100k` — *mode* (режим капитала).
- `YYYY` — год.
- `balance_YYYY.svg`, `summary_YYYY.csv`, `artifacts_YYYY.sha256` — результаты бэктеста и манифест целостности.

> Детальные отчёты по сделкам (с метками времени и ценами), а также временные ряды капитала/просадки, привязанные ко времени, — **непубличны**.

В каждом режиме `mode` — файл `README.ru.md` с обзором и **CSV-метрики** в папке `metrics`.  

> Округление результатов бэктеста (`balance_YYYY.svg`/`summary_YYYY.csv`) **установлено** до 5 знаков **только** при генерации CSV; расчёты внутри движка выполняются без округления.

### Доступные результаты

- **Core Baseline:**
  - *Institutional* - риск **1.0%** (режимы капитала: Notional 1M-5M, Notional 5M-15M, Notional 15M-30M).
  - *Retail Rebate* — риск **1.0%** (старт капитала $100 000 каждый год и режим компаундинга на весь период со старта $100 000).
  - *Retail Standard* — риск **1.0%** (старт капитала $100 000 каждый год и режим компаундинга на весь период со старта $100 000).

- **Extended Baseline:**
  - *Retail Rebate* — риск **1.0%** (старт капитала $100 000 каждый год и режим компаундинга на весь период со старта $100 000).
  - *Retail Standard* — риск **1.0%** (старт капитала $100 000 каждый год и режим компаундинга на весь период со старта $100 000).

- **Stress:**
  - *Retail Standard* — риск **1.0%** (старт капитала $100 000 каждый год).

- **Composite Baseline (Core + Extended):**
  - *Retail Rebate* — риск **1.5%** (режим компаундинга на весь период со старта $100 000).

**Institutional профиль имеет режимы капитализации относительно диапазонов notional volume с риском 1.0%**
  - Notional 1M-5M (старт баланса $1 000 000, ребейз к $1 000 000 при достижении порога >= $1 500 000 по закрытию года).
  - Notional 5M-15M (старт баланса $3 400 000, ребейз к $3 400 000 при достижении порога >= $4 500 000 по закрытию года).
  - Notional 15M-30M (старт баланса $7 900 000, ребейз к $7 900 000 при достижении порога >= $9 000 000 по закрытию года).
> Подробности об институциональных режимах описаны в [`Обзор и Методология Euro Macromechanica (EMM) Backtest`](https://github.com/euro-macromechanica-backtest/results/blob/main/README.ru.md)

### Stress — трек качества данных (2001–2002)

Stress-прогоны публикуются **минимально**: только `fixed_start_100k`.  
Анализ **не публикуется для Stress**, поскольку эти годы исключены из Baseline согласно **политике качества данных**.

> Для Stress зафиксирован один трек:  
> `.../retail_standard/risk_1p00/fixed_start_100k/`  
> (по одному запуску на год; цель — иллюстрация на деградированном **минутном** потоке котировок (M1), не сравнение профилей).

---

## 📋 Методология расчёта метрик (основное)

- **Часовой пояс:** все временные метки и якоря — **UTC+0**.  
- **Результаты на основе реализованного P&L (закрытого баланса).**
- **Месячная шкала (EoM):** balance по **закрытию месяца**; если месяц без сделок — `monthly_return = 0` (balance тянется вперёд, что эквивалентно 0%).  
- **Дисперсии/σ:** выборочные (`ddof = 1`).  
- **Просадки:** `dd_t = eq_t / cummax(eq_t) - 1`; считаются **раздельно** на EoM-сетке и по непрерывному **intramonth** пути (raw balance - closed-equity, realized-only).  
- **Longest underwater (EoM):** самая длинная последовательность `dd < 0` от пика до восстановления (см. TTR ниже); результат в **месяцах**.  
- **TTR / Months since MaxDD trough:**  
  — **TTR (time-to-recover):** число месяцев до полного восстановления пика, **включая** месяц восстановления; если не восстановилось — `NaN`.  
  — **Months since MaxDD trough:** месяцы с момента MaxDD-дна до текущего конца периода, **исключая** месяц дна.  
- **DD-квантили:** считаются по **подписанной** глубине просадки (отрицательные значения). Поэтому при росте перцентиля значения становятся **менее отрицательными** (ближе к 0).  
- **Yearly-метрики:** считаются строго **внутри календарного года**; пик для DD **сбрасывается 1 января 00:00 UTC+0**.  
- **Active months — доля:**  
  — в `yearly_summary.csv`: `active_months_share = active_months_count / 12`.  
     *Примечание:* делитель **фиксирован (=12) даже для неполного текущего года**. Фактическое покрытие выводится отдельно в `months_in_year_available`. Это сделано для сопоставимости между годами; в неполном году доля может казаться ниже — это не ошибка.  
  — в **full-period**: `active_months_share = active_months_count / months`.  
- **R-метрики (нормировка на фактический риск per trade):** `R = pnl_pct / risk_per_trade`.  
  Классификация: `win: R > +eps`, `loss: R < −eps`, `zero: |R| ≤ eps`. **PF по суммам**; нули исключаются из знаменателя PF и из win-rate.  
  *Примеры:* при `risk=1%` → `R = pnl_pct / 0.01`; при `risk=1.5%` → `R = pnl_pct / 0.015`.  
- **Sharpe/Sortino (месячная база → годовая нормировка):** расчёт по **месячным доходностям**, `rf = 0`. Годовая нормировка:  
  `Sharpe_ann = Sharpe_monthly × √12`, **Conditional Sortino** `(loss-only sample LPSD; ddof=1; target=0% monthly; annualized ×√12)`.  
- **Calmar (только EoM):** `Calmar = CAGR / |MaxDD (EoM)|`.  
- **MAR = Calmar (Full period):** в отчёте под «MAR» понимается **Calmar за весь период** —  
  `Calmar (EoM; full period) = CAGR_full_period / |MaxDD_EoM_full_period|`.  
  Для годовых строк (если выводятся) используется обозначение **Calmar (EoM; Year)**, который считается от `CAGR_year` и `MaxDD_EoM_intra_year`. Calmar не считается для «Active Months (Supp.)».  
- **Знаки:** **MaxDD** (EoM/intramonth) и **DD-квантили** — **отрицательные**; это величины «глубины».

### Методика расчёта Conditional Sortino (строгая, loss-only)
**Определение.** В отчётах используется **строгая (консервативная)** версия Sortino, ориентированная на точную оценку риска торговых стратегий. Downside-отклонение рассчитывается **только по убыточным месяцам** (loss-only) относительно целевого уровня \(T=0\%\) в месяц, со **статистическим** стандартным отклонением \(\mathrm{ddof}=1\). Годовая нормализация — умножение на \(\sqrt{12}\).

$$
\mathrm{Sortino}
= \frac{\bar r - T}{\sigma_{\text{down, loss-only}}}\cdot\thinspace\sqrt{12}
$$

$$
\sigma_{\text{down, loss-only}}
= \sqrt{\frac{\sum\limits_{i\thinspace\colon\thinspace r_i < T} (r_i - T)^2}{M - 1}}
$$

где \(M\) — число убыточных месяцев в окне. Если \(M<2\), показатель не публикуется (*insufficient negatives*).

**Отличия от «стандартного» Sortino.** В отраслевой практике распространён вариант на основе LPM(2), где в знаменателе используются **все наблюдения** \(N\) (положительные и нулевые месяцы дают нулевой вклад), а также чаще применяется \(\mathrm{ddof}=0\):

$$
\sigma_{\text{down, std}}
= \sqrt{\frac{1}{N}\sum_{i=1}^{N}\big(\min(r_i - T, 0)\big)^2}
$$

Такой подход «разбавляет» риск неотрицательными месяцами и обычно даёт **более высокие** значения Sortino по сравнению со строгим loss-only.

**Мотивация выбора.** Loss-only с \(\mathrm{ddof}=1\) исключает размывание риска «плоскими» и положительными месяцами и даёт **более консервативную и точную** оценку чувствительности к убыточным эпизодам — что критично для внутримесячных/интрадей-профилей и риск-менеджмента.

**Сопоставимость с отраслью.** Для грубой конвертации к «стандартному» LPM-варианту можно использовать оценочное соотношение

$$
\mathrm{Sortino}_{\text{std}}
\approx
\mathrm{Sortino}_{\text{loss-only}}\cdot\thinspace\sqrt{\frac{N}{M-1}}
$$

где \(N\) — размер окна (например, 12 или 36 мес.), \(M\) — число убыточных месяцев. При малом \(M\) стандартный Sortino будет существенно выше.

**Замечание по скользящим окнам.** На коротких окнах (12м) при малом числе убыточных месяцев \((M)\) строгая методика даёт **Sortino < Sharpe** — ожидаемое свойство формулы, а не ошибка расчёта. На длинных горизонтах эффект сохраняется, но выражен слабее.

**Параметры расчёта.**
- Целевой уровень: \(T = 0\%\) в месяц.
- Downside-отклонение: только по убыточным месяцам; \(\mathrm{ddof}=1\).
- Годовая нормализация: множитель \(\sqrt{12}\).
- Политика публикации: при \(M<2\) метрика помечается как отсутствующая.

### OOS / Walk-Forward / Multiple Testing & Selection Bias
**OOS/Walk-Forward не применяются**: используется **одна фиксированная логика M5 EMM** **без переоптимизации/подгонки**; назначение OOS — выявлять переобучение после настройки параметров, **чего здесь нет**. **Multiple testing bias отсутствует**: тестировалась **одна гипотеза/стратегия без перебора параметров** (selection bias минимален). **Устойчивость профиля подтверждается**: **длинной историей**, **роллингами 12/36м**, **квантилями просадок**, **BCa-интервалами** и **Monte Carlo** (stationary bootstrap **по всем заявленным конфигурациям**). **По запросу для аудита** может быть подготовлен **holdout** без переобучения и без изменения параметров для независимой проверки.

> **Calmar — защита знаменателя:** при вычислении Calmar применяется **ε-guard** к `|MaxDD|`, чтобы избежать деления на почти ноль.  
> **Полнота окон в роллингах:** роллинги считаются только по **полным окнам**; неполные окна дают `NaN` и флаг `insufficient_months=true`.

### Intramonth vs EoM (кратко)
- **EoM (end-of-month):** метрики на месячном гриде по balance закрытия месяца (UTC+0) — просадки/риск «мягче».  
- **Intramonth (path-level):** метрики по непрерывному пути balance (UTC+0) — MaxDD обычно глубже, чем EoM.

### Monte Carlo (по месячным доходностям)
- **Метод:** `stationary_bootstrap` (месячные блоки).
- **Горизонты:** `12`, `36`, `full_period` (каждый считается отдельно).
- **Число путей:** **10 000**.
- **RNG seed:** **42**.
- **Средние длины блоков:** **3–12 месяцев** (перебор по набору L; агрегирование показателей — по медиане между конфигурациями).

> Примечание: итоговые цифры в Monte Carlo агрегируются **по медианам** между конфигурациями блоков.

### Доверительные интервалы (BCa)
- **Метод:** `bootstrap_bca` (BCa — bias‑corrected & accelerated).
- **Число ресэмплов:** **5 000**.
- **RNG seed:** **43**.
- **Корреляции во времени:** `stationary_bootstrap`  
  — для **EoM‑метрик**: средняя длина блока **6 месяцев**;  
  — для **intramonth‑метрик**: средняя длина блока **5 дней**.
- **Уровень доверия:** **90%**.

### Трейд-бутстрэп (EDR & серии убыточных сделок)
- **Вход:** ряд сделочных значений `R` (нормированных на риск per trade).  
- **Метод:** `stationary_bootstrap` по сделкам (сохранение временной зависимости посредством блочного ресэмплирования).
- **Число путей:** **5 000**.
- **Горизонт пути:** **100** сделок.
- **RNG seed:** **44**.  
- **Средняя длина блока:** определяется вероятностью разрыва блока **p = 0.2**, следовательно `E[L] = 1/p = 5` сделок.  
  Реализация: на каждом шаге с вероятностью `p` начинается **новый блок** (прыжок на случайный индекс исходного ряда), иначе продолжается **текущий блок** (циклическое смещение индекса).

> ### Заметка об источнике истины для медиан и агрегатов приведенных в обзорах
> - **Источник истины** для медиан, квантилей, долей и прочих агрегатов — **данные из файлов метрик (CSV)** в каталоге `metrics/`.
> - Все сводные значения в обзорах **пересчитаны напрямую из CSV**.
> - Если в тексте обзора обнаружена **погрешность** (расхождение с тем, что следует из CSV), **истинными считаются расчёты по CSV**.

### Полная методология и определения метрик
См. [`docs/metrics_schema.json`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_schema.json) и [`docs/metrics_schema.md`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_schema.md).  
Продублированные файлы **методологии** и скрипт калькулятора для прозрачности опубликованы в репозитории [`metrics-toolkit`](https://github.com/euro-macromechanica-backtest/metrics-toolkit).

---

## 🧾 Файлы метрик

**Метрики CSV** расположены в *mode* в папке `metrics`. Пример пути:  
`core_baseline_2008-2025-08/institutional/notional_1M-5M_1m/metrics/`

**Базовый набор метрик:**
```
metrics/
  monthly_returns.csv
  full_period_summary.csv
  rebasing_applied.csv
  yearly_summary.csv
  trades_full_period_summary.csv
```

**Расширенный набор метрик:**
```
metrics/
  confidence_intervals.csv
  dd_quantiles_full_period.csv
  monthly_returns.csv
  monte_carlo_summary.csv
  full_period_summary.csv
  rebasing_applied.csv
  rolling_12m.csv
  rolling_36m.csv
  trades_full_period_summary.csv
  yearly_summary.csv

```
> - `rebasing_applied.csv` - для режимов с ребейзом баланса.

- Полный расширенный набор метрик публикуется только для **Core Baseline** результатов.
- **Extended Baseline** и **Composite Baseline (Extended+Core)** намеренно публикуются **без расширенных метрик** — только базовый набор.

---

## 🔗 Привязка входных данных (inputs binding)

- Источник истины: **data-hub** → [`INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md) (логика связки RAW/PREPARED/Economic Calendars). RU-версия — [`INPUTS-PROVENANCE.ru.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.ru.md).
- PIN в этом репозитории: [`docs/INPUTS-PIN.ru.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.ru.md) (какие входные файлы использованы).  
- Зеркала SHA-256/GPG/OTS roll-up манифестов, перечисляющих SHA-256-хэши входных файлов: `integrity/inputs/` (для удобства; источником истины остаётся **data-hub**).

---

## 🧾 Лицензии и доступ
- Результаты и документация в `results/**` — CC BY-NC 4.0.
- Исходный код стратегии, детальные отчёты по сделкам (с метками времени и ценами), а также временные ряды баланса/просадки, привязанные ко времени, — непубличны.

---

## 📎 Ссылки

- **[Обзор и методология Euro Macromechanica (EMM) Backtest](https://github.com/euro-macromechanica-backtest/results/blob/main/README.ru.md)**
- **Полная методология и определения метрик**: [`docs/metrics_schema.json`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_schema.json) / [`docs/metrics_schema.md`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/metrics_schema.md)
- **Модель издержек (commission, spread, slippage) M5 EMM cost model v1.0** — [`docs/cost_model/m5_emm_cost_model_v1.0.csv`](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/m5_emm_cost_model_v1.0.csv).
- **Таблица издержек (spread, slippage) по notional volume** - [/docs/cost_model/eurusd_market_order_costs_ecn_round-turn_pips_v1.0](https://github.com/euro-macromechanica-backtest/results/tree/main/docs/cost_model/eurusd_market_order_costs_ecn_round-turn_pips_v1.0.csv).
- **Инструкция по аудиту** — [`docs/AUDIT.ru.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/AUDIT.ru.md)
- **Инструкция по проверке SHA-256, GPG и OTS** — [`docs/INTEGRITY.ru.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INTEGRITY.ru.md) 
- **PIN (привязка входных данных)** — [`docs/INPUTS-PIN.ru.md`](https://github.com/euro-macromechanica-backtest/results/blob/main/docs/INPUTS-PIN.ru.md)  
- **Data Hub (канонический источник привязки входных данных)** — [`INPUTS-PROVENANCE.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.md) и RU-версия [`INPUTS-PROVENANCE.ru.md`](https://github.com/euro-macromechanica-backtest/data-hub/blob/main/INPUTS-PROVENANCE.ru.md)
- **Продублированные файлы методологии и скрипт калькулятора** — репозиторий [`metrics-toolkit`](https://github.com/euro-macromechanica-backtest/metrics-toolkit)