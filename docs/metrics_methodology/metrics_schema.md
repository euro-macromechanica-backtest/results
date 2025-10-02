# Metrics Schema

_Generated from `metrics_schema.json`.

## Contents
- [about](#about)
- [data_ingestion](#data_ingestion)
- [rebase_policy](#rebase_policy)
- [csv_schemas](#csv_schemas)
- [definitions](#definitions)
- [metadata](#metadata)
- [methodology](#methodology)
- [benchmark_policy](#benchmark_policy)
- [confidence_intervals_policy](#confidence_intervals_policy)
- [month_counting_policy](#month_counting_policy)
- [underwater_anchor_policy](#underwater_anchor_policy)
- [epsilons](#epsilons)
- [minimum_sample_policy](#minimum_sample_policy)
- [monte_carlo_policy](#monte_carlo_policy)
- [naming_conventions](#naming_conventions)
- [nan_inf_display](#nan_inf_display)
- [nan_inf_policy](#nan_inf_policy)
- [newey_west_policy](#newey_west_policy)
- [no_oos_policy](#no_oos_policy)
- [rebasing](#rebasing)
- [risk_parametrization](#risk_parametrization)
- [rolling_windows](#rolling_windows)
- [sharpe_rf](#sharpe_rf)
- [sortino_target](#sortino_target)
- [sortino_target_policy](#sortino_target_policy)
- [streaks_policy](#streaks_policy)
- [supplementary_active_only_metrics](#supplementary_active_only_metrics)
- [ulcer_martin_policy](#ulcer_martin_policy)
- [variance](#variance)
- [visualization_policy](#visualization_policy)
- [yearly_annualization_policy](#yearly_annualization_policy)
- [yearly_metrics_policy](#yearly_metrics_policy)
- [parameters](#parameters)
- [profiles](#profiles)
- [rounding_policy](#rounding_policy)
- [runtime](#runtime)
- [schema_id](#schema_id)
- [schema_title](#schema_title)
- [units](#units)
- [version](#version)


# about

This schema is a full metrics schema set. It includes the base metrics core specification and expanded with additional analytics. The metrics methodology covers calculations for both compounding modes (with and without annual cash-in/cash-outs) and for the fixed-start mode with an annual reset. It is used to compute the M5 EMM backtest results. Disambiguation conventions: 'Annualized, Full Period' != 'Calendar Year, Annualized'. Schema ID: metrics_schema.

# data_ingestion


- **time_key**: time_utc

## deduplication

- **enabled**: true
- **keep**: last

## eom_selection

- **resample**: M
- **take**: last

## inactive_months_policy

- **notes**: Month-end NAV is forward-filled across calendar months; if a month has no observations or no NAV change, its EoM NAV inherits the last observed EoM and monthly_return=0. This policy does not fabricate returns and does not bias volatility or drawdown metrics. In this dataset the monthly coverage is complete; the forward-fill is a no-op (no missing months).
- **forward_fill_nav**: true


# rebase_policy


- **boundary_flow_detection**: At the calendar-year boundary (Dec→Jan), detect opening cash flow as CF_Jan = NAV_SoY − NAV_EoY_prev. If |CF_Jan| ≤ tolerance_ccy, treat as 0.
- **check_boundary**: calendar_year
- **enabled**: true
- **log_emission**: When rebase logging is enabled, 'rebasing_applied.csv' is always written. If no Dec→Jan rebases were detected, the file is emitted with header only (no rows).
- **log_filename**: rebasing_applied.csv
- **sign_convention**: CF > 0 means deposit (cash-in), CF < 0 means withdrawal (cash-out). Persist opening_cashflow_ccy per year; totals feed total_cash_in_ccy / total_cash_out_ccy and flow-aware fields. Both total_cash_in_ccy and total_cash_out_ccy feed net_flows_ccy and the flow-aware fields via NAV + cash_out − cash_in.
- **tolerance_ccy**: 500


# csv_schemas


## rebasing_applied.csv

Audit log of applied year-boundary chain-links between the last closed-balance snapshot of the prior period and the first closed-balance snapshot of the new period (one row per applied boundary; empty header-only file if none).

### Columns

| name | type | decimals | desc |
| --- | --- | --- | --- |
| prev_eom_month | string |  | Month (YYYY-MM) of the last closed-balance snapshot immediately before the boundary (typically December, but may be any last EoM if observations are irregular). |
| prev_eom_nav | currency | 2 | Observed NAV at the last closed-balance snapshot before the boundary (raw, before rebase). |
| next_first_month | string |  | Month (YYYY-MM) of the first closed-balance snapshot immediately after the boundary (typically January, but may be the first available month of the new period). |
| next_first_nav | currency | 2 | Observed NAV at the first closed-balance snapshot after the boundary (raw, before rebase). |


## dd_quantiles_full_period.csv

Full-period drawdown depth quantiles computed on the EoM (month-end) underwater series dd_t = eq_t/cummax(eq_t) - 1, using only months with dd_t<0. Drawdown values dd_t are reported as negative decimals. Durations are computed on CLOSED episodes only and reported in months; if no closed episodes exist, duration quantiles are NaN.

### Columns

| name | label | type | units | desc |
| --- | --- | --- | --- | --- |
| period_start_month | Period Start (Month, UTC) | date | YYYY-MM (UTC) | First month in dataset (YYYY-MM-01 UTC) used for the quantile computation. |
| period_end_month | Period End (Month, UTC) | date | YYYY-MM (UTC) | Last month in dataset (YYYY-MM-01 UTC) used for the quantile computation. |
| dd_observations_count | Drawdown Months — Count | int | count | Number of months with dd_t<0 included in the quantile computation. |
| dd_episodes_count | Drawdown Episodes — Count | int | count | Number of closed underwater episodes (from new high to recovery). |
| drawdown_p90_full_period | Drawdown P90 (EoM, Full Period) | float | decimal | 90th percentile of drawdown depth dd_t over dd_t<0; negative decimal. |
| drawdown_p95_full_period | Drawdown P95 (EoM, Full Period) | float | decimal | 95th percentile of drawdown depth dd_t over dd_t<0; negative decimal. |
| drawdown_p99_full_period | Drawdown P99 (EoM, Full Period) | float | decimal | 99th percentile of drawdown depth dd_t over dd_t<0; negative decimal. |
| underwater_duration_p90_full_period | Underwater Duration P90 (EoM, Months, Full Period) | float | months | 90th percentile of closed underwater episode durations, rounded half-up to months. If dd_episodes_count=0, NaN. |
| underwater_duration_p95_full_period | Underwater Duration P95 (EoM, Months, Full Period) | float | months | 95th percentile of closed underwater episode durations, rounded half-up to months. If dd_episodes_count=0, NaN. |
| drawdown_p90_full_period_xrisk | Drawdown P90 (EoM, Full Period, ×Risk) | float | R-multiple | 90th percentile of drawdown magnitude in R-multiples: \|drawdown_p90_full_period\| / risk_per_trade_pct; positive. |
| drawdown_p95_full_period_xrisk | Drawdown P95 (EoM, Full Period, ×Risk) | float | R-multiple | 95th percentile of drawdown magnitude in R-multiples: \|drawdown_p95_full_period\| / risk_per_trade_pct; positive. |
| drawdown_p99_full_period_xrisk | Drawdown P99 (EoM, Full Period, ×Risk) | float | R-multiple | 99th percentile of drawdown magnitude in R-multiples: \|drawdown_p99_full_period\| / risk_per_trade_pct; positive. |


## full_period_summary.csv

### Columns

| name | label | type | units | desc |
| --- | --- | --- | --- | --- |
| months |  | int | months | Number of months in series (full period length). |
| cagr_full_period | CAGR (Full Period) | float | decimal | Full-period CAGR. |
| volatility_annualized_full_period | Volatility (Annualized, Full Period) | float | decimal | Annualized volatility from monthly returns. |
| sharpe_ratio_annualized_full_period | Sharpe (Annualized, Full Period) | float |  | Annualized Sharpe; rf=0 unless specified. |
| sortino_ratio_annualized_full_period | Sortino (Annualized, Full Period) | float |  | Sortino Ratio (annualized): (mean(r_m − t_m) / downside_std_m) * √12; default t_m=0. If an annual target t_ann is supplied (MAR or rf), use t_m=(1+t_ann)^(1/12)−1; undefined (NaN) if <2 negative months. |
| calmar_ratio_full_period | Calmar Ratio (EoM, Full Period) | float |  | cagr_full_period / \|eom_max_drawdown_full_period\| over the full period. |
| eom_max_drawdown_full_period | Max Drawdown (EoM, Full Period) | float | decimal (0.012 = 1.2%) | Maximum drawdown on end-of-month (EoM) monthly equity: min(dd_t) where dd_t = eq_t / cummax(eq_t) - 1 ≤ 0; reported as a negative decimal. |
| eom_longest_underwater_months | Longest Underwater (EoM, months) | int | months | Longest underwater spell on the EoM monthly equity measured from the last peak (dd=0) to the first recovery (dd>=0), inclusive of the recovery month; if no recovery occurs, measure to the last negative month. Reported as integer months. |
| eom_time_to_recover_months | Time To Recover MaxDD (EoM, months) | float | months | Time to recover for the MaxDD on the EoM monthly equity: number of months from the trough to the first dd>=0, inclusive of the recovery month; if never recovered by period end, report NaN. |
| eom_months_since_maxdd_trough | Months Since MaxDD Trough (EoM) | int | months | Months since the MaxDD trough on the EoM monthly equity: exclude the trough month and include the end month (last available month). |
| intramonth_max_drawdown_full_period | Max Drawdown (Intramonth, Full Period) | float | decimal (0.012 = 1.2%) | Maximum drawdown on the Intramonth (path-level) equity: min(dd_t) along the path (UTC ordering); reported as a negative decimal. |
| intramonth_longest_underwater_months | Longest Underwater (Intramonth, months) | int | months | Longest underwater spell on the Intramonth (path-level) equity measured from the last peak (dd=0) to the first recovery (dd>=0), inclusive of the recovery month; if no recovery, measure to the last negative observation. Duration in months = months_between(peak_ts, recovery_ts) + 1 when recovered; otherwise months_between(peak_ts, last_negative_ts). |
| intramonth_time_to_recover_months | Time To Recover MaxDD (Intramonth, months) | float | months | Time to recover for the MaxDD on the Intramonth (path-level) equity: months_between(trough_ts, recovery_ts) + 1; if never recovered by period end, report NaN. |
| intramonth_months_since_maxdd_trough | Months Since MaxDD Trough (Intramonth) | int | months | Months since the MaxDD trough on the Intramonth (path-level) equity: months_between(trough_ts, last_ts). The trough month is excluded; the end month is included. |
| underwater_months_share_full_period | Underwater Months — Share (Full Period) | float | decimal | Share of months with dd_t<0 over the full period = dd_observations_count / months. |
| ulcer_index_full_period | Ulcer Index (EoM, Full Period) | float | decimal | Root-mean-square of dd_t over months with dd_t<0 on the EoM monthly grid; reported as a positive decimal. |
| martin_ratio_full_period | Martin Ratio (EoM, Full Period) | float |  | CAGR (Full Period) / Ulcer Index (Full Period); NaN/Inf policy applies. |
| pain_index_full_period | Pain Index (EoM, Full Period) | float | decimal | Pain Index on EoM grid: mean(\|dd_t\|) over months with dd_t<0; positive decimal. |
| pain_ratio_full_period | Pain Ratio (EoM, Full Period) | float |  | Pain Ratio = CAGR (Full Period) / Pain Index (EoM, Full Period); NaN/Inf policy applies. |
| skewness_full_period | Skewness (Full Period) | float | dimensionless | Skewness of monthly returns over the full period (sample stdev with ddof=1 in standardization). |
| kurtosis_excess_full_period | Excess Kurtosis (Full Period) | float | dimensionless | Excess kurtosis (kurtosis − 3) of monthly returns over the full period (sample stdev, ddof=1). |
| eom_max_drawdown_full_period_xrisk | Max Drawdown (EoM, Full Period, ×Risk) | float | R-multiple | Maximum drawdown on the EoM monthly equity expressed in R-multiples: \|eom_max_drawdown_full_period\| / risk_per_trade_pct; positive magnitude. |
| intramonth_max_drawdown_full_period_xrisk | Max Drawdown (Intramonth, Full Period, ×Risk) | float | R-multiple | Maximum drawdown on the Intramonth (path-level) equity expressed in R-multiples: \|intramonth_max_drawdown_full_period\| / risk_per_trade_pct; positive magnitude. |
| ulcer_index_full_period_xrisk | Ulcer Index (EoM, Full Period, ×Risk) | float | R-multiple | Ulcer Index on the EoM grid expressed in R-multiples: ulcer_index_full_period / risk_per_trade_pct; positive magnitude. |
| monthly_var_95_full_period_xrisk | Monthly VaR 95% (Full Period, ×Risk) | float | R-multiple | Monthly historical VaR 95% over the full period expressed in R-multiples: \|monthly_var_95_full_period\| / risk_per_trade_pct. |
| monthly_es_95_full_period_xrisk | Monthly ES 95% (Full Period, ×Risk) | float | R-multiple | Monthly historical ES 95% over the full period expressed in R-multiples: \|monthly_es_95_full_period\| / risk_per_trade_pct. |
| monthly_var_99_full_period_xrisk | Monthly VaR 99% (Full Period, ×Risk) | float | R-multiple | Monthly historical VaR 99% over the full period expressed in R-multiples: \|monthly_var_99_full_period\| / risk_per_trade_pct. |
| monthly_es_99_full_period_xrisk | Monthly ES 99% (Full Period, ×Risk) | float | R-multiple | Monthly historical ES 99% over the full period expressed in R-multiples: \|monthly_es_99_full_period\| / risk_per_trade_pct. |
| worst_month_return_full_period_xrisk | Worst Month (Full Period, ×Risk) | float | R-multiple | Worst single monthly return over the full period expressed in R-multiples: \|worst_month_return_full_period\| / risk_per_trade_pct. |
| best_month_return_full_period_xrisk | Best Month (Full Period, ×Risk) | float | R-multiple | Best single monthly return over the full period expressed in R-multiples: \|best_month_return_full_period\| / risk_per_trade_pct. |
| active_months_count | Active Months — Count | int | months | Number of active months over the full period. |
| active_months_share | Active Months — Share | float | decimal | Active months share = active_months_count / months. |
| volatility_active_annualized | Volatility (Annualized, Active Months — Supp.) | float | decimal (0.012 = 1.2%) | Annualized vol on active months (ddof=1)*sqrt(12). |
| sharpe_active_annualized | Sharpe (Annualized, Active Months — Supp.) | float |  | Sharpe on active months; sqrt(12). |
| sortino_active_annualized | Sortino (Annualized, Active Months — Supp.) | float |  | Sortino on active months; default target 0; sqrt(12). |
| cagr_active | CAGR (Active Months — Supp.) | float | decimal (0.012 = 1.2%) | Active-only CAGR: (Π(1+r_active))^(12/N_active)-1. |
| negative_months_count_full_period | Negative Months — Count (Full Period) | int | months | Number of months with monthly_return<0 over the full period. |
| total_return_full_period | Total Return (Full Period) | float | decimal (0.012 = 1.2%) | Π(1 + monthly_return) − 1 over the full compounding series. |
| wealth_multiple_full_period | Wealth Multiple (Full Period) | float |  | 1 + total_return_full_period. |
| ending_nav_full_period | Ending NAV (Full Period) | float | USD | NAV0 * Π(1 + monthly_return) with NAV0=starting_nav_ccy |
| period_start_month | Period Start (Month, UTC) | date | YYYY-MM (UTC) | First month in the dataset (YYYY-MM UTC). |
| period_end_month | Period End (Month, UTC) | date | YYYY-MM (UTC) | Last month in the dataset (YYYY-MM UTC). |
| positive_months_count_full_period | Positive Months — Count (Full Period) | int | months | Months with monthly_return>0. |
| best_month_return_full_period | Best Month Return (Full Period) | float | decimal (0.012 = 1.2%) |  |
| worst_month_return_full_period | Worst Month Return (Full Period) | float | decimal (0.012 = 1.2%) |  |
| mean_monthly_return_full_period | Mean Monthly Return (Full Period) | float | decimal (0.012 = 1.2%) |  |
| median_monthly_return_full_period | Median Monthly Return (Full Period) | float | decimal (0.012 = 1.2%) |  |
| zero_months_count_full_period | Zero Months — Count (Full Period) | int | months |  |
| years_covered | Years — Covered (Full Period) | int | years |  |
| best_year_return_calendar_full_period | Best Calendar-Year Return (Full Period) | float | decimal (0.012 = 1.2%) |  |
| best_year | Best Year (YYYY) | int | YYYY |  |
| worst_year_return_calendar_full_period | Worst Calendar-Year Return (Full Period) | float | decimal (0.012 = 1.2%) |  |
| worst_year | Worst Year (YYYY) | int | YYYY |  |
| downside_deviation_annualized_full_period | Downside Deviation (Annualized, Full Period) | float | decimal (0.012 = 1.2%) | Annualized downside deviation using monthly returns and target=0% annual converted to monthly; ddof=1; ×√12. |
| newey_west_tstat_mean_monthly_return | Newey–West t-stat (Mean Monthly Return) | float | dimensionless (t-statistic) | HAC-robust t-statistic for mean monthly return using Newey–West standard error with Bartlett kernel and lag q (runtime parameter). |
| newey_west_p_value_mean_monthly_return | p-value (Newey–West, Two-sided) | float | decimal (0.012 = 1.2%) | Two-sided p-value for the HAC-robust t-statistic under asymptotic normal approximation. |
| max_consecutive_up_months_full_period | Max Consecutive Up Months (Full Period) | int | months | Longest run of months with monthly_return>0. Months with monthly_return==0 break runs (do not count). |
| max_consecutive_down_months_full_period | Max Consecutive Down Months (Full Period) | int | months | Longest run of months with monthly_return<0. Months with monthly_return==0 break runs (do not count). |
| omega_ratio_full_period_target_0m | Omega (τ=0, Full Period) | float |  | Omega ratio at monthly threshold τ=0 over the full period. |
| monthly_var_95_full_period | Monthly VaR 95% (Full Period) | float | decimal | Historical monthly VaR at 95% over the full period (5th percentile of r). Negative decimal return. |
| monthly_es_95_full_period | Monthly ES 95% (Full Period) | float | decimal | Historical monthly ES at 95% over the full period (mean of r ≤ VaR95); reported as a negative decimal monthly return. |
| monthly_var_99_full_period | Monthly VaR 99% (Full Period) | float | decimal | Historical monthly VaR at 99% over the full period (1st percentile of r); reported as a negative decimal monthly return. |
| monthly_es_99_full_period | Monthly ES 99% (Full Period) | float | decimal | Historical monthly ES at 99% over the full period (mean of r ≤ VaR99); reported as a negative decimal monthly return. |
| gain_to_pain_ratio_monthly_full_period | Gain-to-Pain (Monthly, Full Period) | float |  | Gain-to-Pain on monthly returns: sum(r_m>0) / \|sum(r_m<0)\| over the full period; zeros excluded from both sums. NaN/Inf policy per generic_ratio. |
| tail_ratio_p95_p5_full_period | Tail Ratio P95/\|P5\| (Full Period) | float |  | Tail Ratio = Q95(r) / \|Q5(r)\| on monthly returns over the full period. Default scope: all calendar months (not active-only). |
| trade_count_full_period |  | int | count | Total number of trades over the full period. |
| observed_last_eom_nav_ccy | Observed Last EoM NAV | float | USD | Observed NAV at the last end-of-month snapshot on the compounding track. |
| total_cash_in_ccy | Total Cash-In (Full Period) | float | USD | Sum of all cash-in flows over the full period (absolute USD). |
| total_cash_out_ccy | Total Cash-Out (Full Period) | float | USD | Sum of all cash-out flows over the full period (absolute USD). |
| net_flows_ccy | Net Flows (Full Period) | float | USD | total_cash_in_ccy - total_cash_out_ccy over the full period (USD). |
| flow_aware_ending_value_ccy | Flow-Aware Ending Value (Full Period) | float | USD | observed_last_eom_nav_ccy + total_cash_out_ccy − total_cash_in_ccy (USD). |
| flow_aware_wealth_multiple | Flow-Aware Wealth Multiple | float |  | flow_aware_ending_value_ccy / starting_nav_ccy. |


## monte_carlo_summary.csv

Monte Carlo bootstrap results on the full-period monthly return series. Each row is one configuration (method, block_mean_length_months L, horizon). Monthly resampling (stationary_bootstrap by default). CAGR annualization via (1+ret)^(12/H)−1. MaxDD magnitudes computed on the EoM grid. Includes risk-normalized (×Risk) percentiles for MaxDD and conditional depths. Time-to-breach (months) and (no-)breach probabilities are not in ×Risk units; they are computed at thresholds defined relative to risk (k×risk).

Policy-aware mode (Cap NAV). If runtime.policy.enabled=true, each simulated NAV path is post-processed by a Cap NAV rule after monthly compounding. At a configured cadence ('yearly' in month=mc_policy_start_month or 'monthly'), if the end-of-month NAV exceeds (mc_cap_nav_ccy + mc_cap_tolerance_ccy), the excess is withdrawn so NAV is brought down to mc_cap_nav_ccy. Withdrawals accumulate as cash-out events. The following columns are populated in this mode without renaming:

- mc_policy, mc_cap_nav_ccy, mc_policy_apply, mc_policy_start_month, mc_cap_tolerance_ccy
- policy_total_cash_out_ccy_p05, policy_total_cash_out_ccy_p50, policy_total_cash_out_ccy_p95
- policy_ending_nav_ccy_p05, policy_ending_nav_ccy_p50, policy_ending_nav_ccy_p95
- policy_flow_aware_ending_value_ccy_p05, policy_flow_aware_ending_value_ccy_p50, policy_flow_aware_ending_value_ccy_p95
- policy_flow_aware_multiple_p05, policy_flow_aware_multiple_p50, policy_flow_aware_multiple_p95
- policy_prob_any_cashout, policy_avg_cashout_events

When the policy is disabled, mc_policy='none' and all policy_* fields are set to NaN (non-applicable).

### Columns

| name | label | type | units | desc |
| --- | --- | --- | --- | --- |
| period_start_month | Period Start (Month, UTC) | date | YYYY-MM (UTC) | First month used for sampling (full period start). |
| period_end_month | Period End (Month, UTC) | date | YYYY-MM (UTC) | Last month used for sampling (full period end; YTD inclusive). |
| method | Bootstrap Method | string |  | stationary_bootstrap (Politis–Romano) or moving_block_bootstrap (MBB). Default: stationary_bootstrap. |
| block_mean_length_months | Block Mean Length (Months) | int | months | Expected block length (L) in months; for stationary bootstrap L=1/p. Institutional default: 6. |
| n_paths | Paths | int |  | Number of bootstrap paths (e.g., 10000). |
| horizon_months | Horizon (Months) | int | months | Simulation horizon in months (e.g., 12, 36, full_period). In output, 'full_period' is resolved to the dataset length in months. |
| seed | Seed | int |  | Random seed for reproducibility (optional). |
| cagr_annualized_p05 | CAGR (Annualized) — P05 | float | decimal (0.012 = 1.2%) | 5th percentile of annualized CAGR across paths over the horizon. |
| cagr_annualized_p50 | CAGR (Annualized) — Median | float | decimal (0.012 = 1.2%) | 50th percentile (median) of annualized CAGR across paths over the horizon. |
| cagr_annualized_p95 | CAGR (Annualized) — P95 | float | decimal (0.012 = 1.2%) | 95th percentile of annualized CAGR across paths over the horizon. |
| max_drawdown_magnitude_p50 | Max Drawdown (EoM) — Magnitude P50 | float | decimal (0.012 = 1.2%) | 50th percentile of maximum drawdown magnitude (\|dd\|) on the EoM monthly grid across simulated paths; positive decimal. |
| max_drawdown_magnitude_p95 | Max Drawdown (EoM) — Magnitude P95 | float | decimal (0.012 = 1.2%) | 95th percentile of maximum drawdown magnitude (\|dd\|) computed on the EoM monthly grid across simulated paths; reported as a positive decimal. |
| max_drawdown_magnitude_p99 | Max Drawdown (EoM) — Magnitude P99 | float | decimal (0.012 = 1.2%) | 99th percentile of maximum drawdown magnitude (\|dd\|) computed on the EoM monthly grid across simulated paths; reported as a positive decimal. |
| prob_negative_horizon_return | Pr[Horizon Return < 0] | float | decimal | Probability (share of paths) that cumulative return over the horizon is < 0 (decimal). |
| wealth_multiple_p05 | Wealth Multiple — P05 | float |  | 5th percentile of wealth multiple over the horizon (Π(1+r)). |
| wealth_multiple_p50 | Wealth Multiple — Median | float |  | Median of wealth multiple over the horizon (Π(1+r)). |
| wealth_multiple_p95 | Wealth Multiple — P95 | float |  | 95th percentile of wealth multiple over the horizon (Π(1+r)). |
| ending_nav_p05 | Ending NAV — P05 (USD) | float | USD | Percentile of Ending NAV over the horizon given NAV0=starting_nav_ccy (USD). |
| ending_nav_p50 | Ending NAV — Median (USD) | float | USD | Percentile of Ending NAV over the horizon given NAV0=starting_nav_ccy (USD). |
| ending_nav_p95 | Ending NAV — P95 (USD) | float | USD | Percentile of Ending NAV over the horizon given NAV0=starting_nav_ccy (USD). |
| risk_per_trade_pct | Risk per Trade (Decimal) | float |  | Risk per trade used in the simulation (decimal, e.g., 0.01 or 0.015). |
| prob_maxdd_ge_5x_risk_eom | Pr[EoM MaxDD ≥ 5×Risk] | float | decimal | Share of simulated paths with EoM MaxDD magnitude ≥ 5×risk_per_trade_pct over the horizon. |
| prob_maxdd_ge_7x_risk_eom | Pr[EoM MaxDD ≥ 7×Risk] | float | decimal | Share of simulated paths with EoM MaxDD magnitude ≥ 7×risk_per_trade_pct over the horizon. |
| prob_maxdd_ge_10x_risk_eom | Pr[EoM MaxDD ≥ 10×Risk] | float | decimal | Share of simulated paths with EoM MaxDD magnitude ≥ 10×risk_per_trade_pct over the horizon. |
| prob_maxdd_ge_5pc_eom | Pr[EoM MaxDD ≥ 5%] | float | decimal | Share of simulated paths with EoM MaxDD magnitude ≥ 5% over the horizon. |
| prob_maxdd_ge_7pc_eom | Pr[EoM MaxDD ≥ 7%] | float | decimal | Share of simulated paths with EoM MaxDD magnitude ≥ 7% over the horizon. |
| prob_maxdd_ge_10pc_eom | Pr[EoM MaxDD ≥ 10%] | float | decimal | Share of simulated paths with EoM MaxDD magnitude ≥ 10% over the horizon. |
| mc_maxdd_xrisk_p50 | Max Drawdown (EoM) — ×Risk P50 | float | R-multiple | Median of EoM MaxDD magnitude across paths expressed in R-multiples: max_drawdown_magnitude_p50 / risk_per_trade_pct. |
| mc_maxdd_xrisk_p95 | Max Drawdown (EoM) — ×Risk P95 | float | R-multiple | 95th percentile of EoM MaxDD magnitude across paths expressed in R-multiples: max_drawdown_magnitude_p95 / risk_per_trade_pct. |
| mc_maxdd_xrisk_p99 | Max Drawdown (EoM) — ×Risk P99 | float | R-multiple | 99th percentile of EoM MaxDD magnitude across paths expressed in R-multiples: max_drawdown_magnitude_p99 / risk_per_trade_pct. |
| mc_ttb_ge_5x_risk_p50 | Time to Breach ≥5×Risk — Median (Months) | float |  | Median months to first breach of \|dd\| ≥ 5×risk among paths where breach occurs; end-anchored months. |
| mc_ttb_ge_7x_risk_p50 | Time to Breach ≥7×Risk — Median (Months) | float |  | Median months to first breach of \|dd\| ≥ 7×risk among paths where breach occurs; end-anchored months. |
| mc_ttb_ge_10x_risk_p50 | Time to Breach ≥10×Risk — Median (Months) | float |  | Median months to first breach of \|dd\| ≥ 10×risk among paths where breach occurs; end-anchored months. |
| prob_no_breach_ge_5x_risk_eom | Pr[No Breach ≥5×Risk] | float | decimal | Complement probability: 1 − Pr[EoM MaxDD ≥ 5×risk] over the horizon. |
| prob_no_breach_ge_7x_risk_eom | Pr[No Breach ≥7×Risk] | float | decimal | Complement probability: 1 − Pr[EoM MaxDD ≥ 7×risk] over the horizon. |
| prob_no_breach_ge_10x_risk_eom | Pr[No Breach ≥10×Risk] | float | decimal | Complement probability: 1 − Pr[EoM MaxDD ≥ 10×risk] over the horizon. |
| cond_es_maxdd_ge_5x_risk | Cond. Depth \| Breach ≥5×Risk (×Risk) | float | R-multiple | Conditional expected MaxDD depth in R-multiples given breach ≥ 5×risk: E[ MaxDD_xRisk \| breach ≥ 5×risk ]. |
| cond_es_maxdd_ge_7x_risk | Cond. Depth \| Breach ≥7×Risk (×Risk) | float | R-multiple | Conditional expected MaxDD depth in R-multiples given breach ≥ 7×risk: E[ MaxDD_xRisk \| breach ≥ 7×risk ]. |
| cond_es_maxdd_ge_10x_risk | Cond. Depth \| Breach ≥10×Risk (×Risk) | float | R-multiple | Conditional expected MaxDD depth in R-multiples given breach ≥ 10×risk: E[ MaxDD_xRisk \| breach ≥ 10×risk ]. |
| prob_maxdd_ge_20pc_eom | Pr[EoM MaxDD ≥ 20%] | float | decimal | Share of simulated paths with EoM MaxDD magnitude ≥ 20% over the horizon. |
| prob_maxdd_ge_30pc_eom | Pr[EoM MaxDD ≥ 30%] | float | decimal | Share of simulated paths with EoM MaxDD magnitude ≥ 30% over the horizon. |
| mc_policy | MC Policy | string |  | Monte-Carlo policy applied (e.g., cap_nav) or none. |
| mc_cap_nav_ccy | Cap NAV (Policy) | float | USD | Cap level for NAV in policy-aware MC (currency). |
| mc_policy_apply | Policy Apply Mode | string |  | Policy check cadence: yearly or monthly. |
| mc_policy_start_month | Policy Start Month | int |  | Month number (1..12) used for yearly policy checks. |
| mc_cap_tolerance_ccy | Cap Tolerance | float | USD | Absolute tolerance for trimming (currency). |
| policy_total_cash_out_ccy_p05 | Total Cash-Out P05 | float | USD | P5 of total cash-out across policy-aware MC paths. |
| policy_total_cash_out_ccy_p50 | Total Cash-Out P50 | float | USD | Median of total cash-out across policy-aware MC paths. |
| policy_total_cash_out_ccy_p95 | Total Cash-Out P95 | float | USD | P95 of total cash-out across policy-aware MC paths. |
| policy_ending_nav_ccy_p05 | Ending NAV (Policy) P05 | float | USD | P5 of ending NAV after applying the policy. |
| policy_ending_nav_ccy_p50 | Ending NAV (Policy) P50 | float | USD | Median of ending NAV after applying the policy. |
| policy_ending_nav_ccy_p95 | Ending NAV (Policy) P95 | float | USD | P95 of ending NAV after applying the policy. |
| policy_flow_aware_ending_value_ccy_p05 | Flow-Aware Ending Value P05 | float | USD | P5 of (ending NAV + total cash-out). |
| policy_flow_aware_ending_value_ccy_p50 | Flow-Aware Ending Value P50 | float | USD | Median of (ending NAV + total cash-out). |
| policy_flow_aware_ending_value_ccy_p95 | Flow-Aware Ending Value P95 | float | USD | P95 of (ending NAV + total cash-out). |
| policy_flow_aware_multiple_p05 | Flow-Aware Multiple P05 | float |  | P5 of (flow-aware ending value / starting NAV). |
| policy_flow_aware_multiple_p50 | Flow-Aware Multiple P50 | float |  | Median of (flow-aware ending value / starting NAV). |
| policy_flow_aware_multiple_p95 | Flow-Aware Multiple P95 | float |  | P95 of (flow-aware ending value / starting NAV). |
| policy_prob_any_cashout | Prob(Any Cash-Out) | float |  | Share of paths with ≥1 cash-out event under the policy. |
| policy_avg_cashout_events | Avg Cash-Out Events | float |  | Average number of cash-out events per path under the policy. |


## monthly_returns.csv

### Columns

| name | label | type | units | desc |
| --- | --- | --- | --- | --- |
| year |  | int | YYYY | Calendar year (YYYY). |
| month |  | int | 1–12 | 1–12. |
| monthly_return | Monthly Return | float | decimal | Monthly return from EoM NAV; includes months with 0. (Compounding/realized-PnL: include all months; if there were no trades and NAV did not change ⇒ 0; if there were trades but net monthly PnL is exactly 0 (NAV unchanged) ⇒ 0.). For months with an opening cash flow (Jan after rebase), compute monthly_return(m) = (NAV_EoM(m) − CF_m) / NAV_EoM(m−1) − 1, where CF_m = opening_cashflow_ccy for that year, else 0. |
| active_month | Active Month | bool |  | True if month has at least one trade (trade_count>0). |


## rolling_12m.csv

Rolling 12-month window metrics (end-anchored by 'month'; UTC month anchors). Values are computed only when at least 12 months are available; otherwise set to NaN and flag insufficient_months.

### Columns

| name | label | type | units | desc |
| --- | --- | --- | --- | --- |
| month | Month (Window End, UTC) | date | YYYY-MM | Month-anchored date YYYY-MM UTC for the end of the 12M window. |
| window_months | Window — Months | int | months | Size of the rolling window (12). |
| window_start_month | Window Start (UTC) | date | YYYY-MM | Start month of the 12M window (inclusive). |
| window_end_month | Window End (UTC) | date | YYYY-MM | End month of the 12M window (inclusive); equals 'month'. |
| rolling_return_12m | Rolling 12M Return | float | decimal | Product over last 12 months: Π(1+r_m) − 1. NaN if insufficient months. |
| rolling_volatility_annualized_12m | Volatility (Annualized, Rolling 12M) | float | decimal (0.012 = 1.2%) | Annualized volatility on last 12 months: stdev(r_m, ddof=1)*√12. NaN if insufficient months. |
| rolling_sharpe_annualized_12m | Sharpe (Annualized, Rolling 12M) | float |  | Annualized Sharpe on last 12 months: mean(r_m)/stdev(r_m, ddof=1)*√12 (rf=0 by default). NaN if insufficient months. |
| insufficient_months | Insufficient Months | bool |  | True if fewer than 12 months available as of the window end. |
| rolling_max_drawdown_12m | Rolling 12M Max Drawdown (EoM) | float | decimal (0.012 = 1.2%) | Maximum drawdown on the 12-month EoM sub-curve: min(dd_t); reported as a negative return. |
| rolling_calmar_12m | Rolling 12M Calmar (EoM) | float |  | Rolling 12M Calmar: rolling_return_12m (as annualized equals same for 12M) divided by \|rolling_max_drawdown_12m\|; NaN/Inf policy applies. |
| rolling_sortino_annualized_12m | Sortino (Annualized, Rolling 12M) | float |  | Annualized Sortino on last 12 months; downside via min(r_m − t_m, 0) with institutional target policy; ddof=1; ×√12. |
| active_months_count_12m | Active Months — Count (12M) | int | months | Number of months with active_month=True inside the 12M window. |
| active_months_share_12m | Active Months — Share (12M) | float | decimal | active_months_count_12m / 12; set to NaN when insufficient_months=true. |
| positive_months_count_12m | Positive Months — Count (12M) | int | months | Number of months with monthly_return>0 inside the 12M window. |
| negative_months_count_12m | Negative Months — Count (12M) | int | months | Number of months with monthly_return<0 inside the 12M window. |
| insufficient_negative_months | Insufficient Negatives (12M) | bool |  | True if fewer than 2 months with r_m below target within the 12M window (Sortino undefined). |
| positive_months_share_12m | Positive Months — Share (12M) | float | decimal | positive_months_count_12m / 12; set to NaN when insufficient_months=true. |
| negative_months_share_12m | Negative Months — Share (12M) | float | decimal | negative_months_count_12m / 12; set to NaN when insufficient_months=true. |
| rolling_omega_12m_target_0m | Rolling Omega (τ=0, 12M) | float |  | Omega(τ=0) on the last 12 months. |
| rolling_var_95_12m | Rolling Monthly VaR 95% (12M) | float | decimal | Historical monthly VaR95 within the 12M window. |
| rolling_es_95_12m | Rolling Monthly ES 95% (12M) | float | decimal | Historical monthly ES95 within the 12M window. |
| rolling_tail_ratio_p95_p5_12m | Rolling Tail Ratio P95/\|P5\| (12M) | float |  | Tail Ratio = Q95/\|Q5\| within the 12M window. Default scope: all calendar months (not active-only) |
| rolling_max_drawdown_12m_xrisk | Rolling 12M Max Drawdown (×Risk) | float | R-multiple | Rolling 12M maximum drawdown expressed in R-multiples: \|rolling_max_drawdown_12m\| / risk_per_trade_pct. |
| rolling_var_95_12m_xrisk | Rolling Monthly VaR 95% (12M, ×Risk) | float | R-multiple | Rolling monthly VaR 95% expressed in R-multiples: \|rolling_var_95_12m\| / risk_per_trade_pct. |
| rolling_es_95_12m_xrisk | Rolling Monthly ES 95% (12M, ×Risk) | float | R-multiple | Rolling monthly ES 95% expressed in R-multiples: \|rolling_es_95_12m\| / risk_per_trade_pct. |


## rolling_36m.csv

Rolling 36-month window metrics (end-anchored by 'month'; UTC month anchors). Values are computed only when at least 36 months are available; otherwise set to NaN and flag insufficient_months.

### Columns

| name | label | type | units | desc |
| --- | --- | --- | --- | --- |
| month | Month (Window End, UTC) | date | YYYY-MM | Month-anchored date YYYY-MM UTC for the end of the 36M window. |
| window_months | Window — Months | int | months | Size of the rolling window (36). |
| window_start_month | Window Start (UTC) | date | YYYY-MM | Start month of the 36M window (inclusive). |
| window_end_month | Window End (UTC) | date | YYYY-MM | End month of the 36M window (inclusive); equals 'month'. |
| rolling_return_annualized_36m | Rolling 36M Return (Annualized) | float | decimal | Annualized return over last 36 months: (Π(1+r_m))^(12/N) − 1 with N=36. NaN if insufficient months. |
| rolling_max_drawdown_36m | Rolling 36M Max Drawdown (EoM) | float | decimal (0.012 = 1.2%) | Maximum drawdown on the 36-month EoM sub-curve: min(dd_t); reported as a negative return. |
| rolling_calmar_36m | Rolling 36M Calmar (EoM) | float |  | Rolling 36M Calmar: rolling_return_annualized_36m / \|rolling_max_drawdown_36m\| with NaN/Inf policy. |
| insufficient_months | Insufficient Months | bool |  | True if fewer than 36 months available as of the window end. |
| rolling_volatility_annualized_36m | Volatility (Annualized, Rolling 36M) | float | decimal (0.012 = 1.2%) | Annualized volatility on last 36 months: stdev(r_m, ddof=1)*√12; NaN if insufficient months. |
| rolling_sharpe_annualized_36m | Sharpe (Annualized, Rolling 36M) | float |  | Annualized Sharpe on last 36 months: mean(r_m)/stdev(r_m, ddof=1)*√12 (rf=0 by default). NaN if insufficient months. |
| rolling_sortino_annualized_36m | Sortino (Annualized, Rolling 36M) | float |  | Annualized Sortino on last 36 months; downside via min(r_m − t_m, 0) with institutional target policy; ddof=1; ×√12. |
| active_months_count_36m | Active Months — Count (36M) | int | months | Number of months with active_month=True inside the 36M window. |
| active_months_share_36m | Active Months — Share (36M) | float | decimal | active_months_count_36m / 36; set to NaN when insufficient_months=true. |
| positive_months_count_36m | Positive Months — Count (36M) | int | months | Number of months with monthly_return>0 inside the 36M window. |
| negative_months_count_36m | Negative Months — Count (36M) | int | months | Number of months with monthly_return<0 inside the 36M window. |
| insufficient_negative_months | Insufficient Negatives (36M) | bool |  | True if fewer than 2 months with r_m below target within the 36M window (Sortino undefined). |
| positive_months_share_36m | Positive Months — Share (36M) | float | decimal | positive_months_count_36m / 36; set to NaN when insufficient_months=true. |
| negative_months_share_36m | Negative Months — Share (36M) | float | decimal | negative_months_count_36m / 36; set to NaN when insufficient_months=true. |
| rolling_omega_36m_target_0m | Rolling Omega (τ=0, 36M) | float |  | Omega(τ=0) on the last 36 months. |
| rolling_var_95_36m | Rolling Monthly VaR 95% (36M) | float | decimal | Historical monthly VaR95 within the 36M window. |
| rolling_es_95_36m | Rolling Monthly ES 95% (36M) | float | decimal | Historical monthly ES95 within the 36M window. |
| rolling_tail_ratio_p95_p5_36m | Rolling Tail Ratio P95/\|P5\| (36M) | float |  | Tail Ratio = Q95/\|Q5\| within the 36M window. Default scope: all calendar months (not active-only) |
| rolling_max_drawdown_36m_xrisk | Rolling 36M Max Drawdown (×Risk) | float | R-multiple | Rolling 36M maximum drawdown expressed in R-multiples: \|rolling_max_drawdown_36m\| / risk_per_trade_pct. |
| rolling_var_95_36m_xrisk | Rolling Monthly VaR 95% (36M, ×Risk) | float | R-multiple | Rolling monthly VaR 95% expressed in R-multiples: \|rolling_var_95_36m\| / risk_per_trade_pct. |
| rolling_es_95_36m_xrisk | Rolling Monthly ES 95% (36M, ×Risk) | float | R-multiple | Rolling monthly ES 95% expressed in R-multiples: \|rolling_es_95_36m\| / risk_per_trade_pct. |


## confidence_intervals.csv

Confidence intervals for metrics computed on 'full_period' or 'calendar_year' scopes using bootstrap only (no Monte Carlo, no trade-bootstrap).

### Columns

| name | label | type | units | desc |
| --- | --- | --- | --- | --- |
| period_start_month | Period Start (Month, UTC) | date | YYYY-MM (UTC) | First month of the data in scope for this CI row (YYYY-MM UTC). |
| period_end_month | Period End (Month, UTC) | date | YYYY-MM (UTC) | Last month of the data in scope for this CI row (YYYY-MM UTC). |
| scope | Scope | string |  | One of: 'full_period' \| 'calendar_year'. |
| year | Year (if scope=calendar_year) | int | YYYY | Calendar year (YYYY) if scope='calendar_year'; blank otherwise. |
| metric_key | Metric Key | string |  | Key of the metric this CI pertains to (see definitions.ci_supported_metrics). |
| metric_label | Metric Label | string |  | Human-readable label of the metric at export time. |
| metric_basis | Basis | string |  | Computation basis: 'monthly' \| 'eom' \| 'intramonth'. |
| units | Units | string |  | Units consistent with the base metric (e.g., 'decimal', 'R-multiple', 'dimensionless ratio'). |
| rounding_family | Rounding Family | string |  | Formatting hint: 'percent_like' \| 'xrisk_like' \| 'sharpe_sortino_like' \| 'ratio_like'. |
| method | CI Method | string |  | Method per row: 'bootstrap_percentile' \| 'bootstrap_bca'. |
| ci_level_pct | CI Level (%) | float |  | Confidence level, e.g., 90. |
| estimate | Point Estimate | float | same as base metrics | Point estimate in the metric’s units and sign conventions. |
| ci_low | CI Low | float |  | Lower bound of the CI in the metric’s units and sign conventions. |
| ci_high | CI High | float |  | Upper bound of the CI in the metric’s units and sign conventions. |
| bootstrap_type | Bootstrap Type | string |  | For metric CIs: e.g., 'stationary_bootstrap' or 'moving_block_bootstrap'. |
| block_mean_length_months | Block Mean Length (Months) | int | months | L for monthly/EoM bases. |
| block_mean_length_days | Block Mean Length (Days) | int |  | L for intramonth basis in calendar days. |
| n_boot | Bootstrap Replicates | int |  | Number of bootstrap replicates for metric CIs. |
| seed | Seed | int |  | Random seed for reproducibility (optional). |


## trades_full_period_summary.csv

### Columns

| name | label | type | units | desc |
| --- | --- | --- | --- | --- |
| trade_count |  | int | count | Total trade_count (full period). |
| win_rate |  | float | decimal | Share of R>0 among trades with R≠0; denominator excludes zero-R trades. |
| profit_factor |  | float |  | PF on sums; zeros (R=0) are excluded from both sums. |
| average_winning_trade_r |  | float | R | mean(R \| R > +eps). |
| average_losing_trade_r |  | float | R | mean(R \| R < -eps). |
| payoff_ratio |  | float |  | average_winning_trade_r / \|average_losing_trade_r\|. |
| expectancy_mean_r |  | float | R | Mean R. |
| expectancy_median_r |  | float | R | Median R. |
| r_std_dev |  | float |  | Stdev R, ddof=1. |
| r_min |  | float |  | Min R. |
| r_max |  | float |  | Max R. |
| worst_5_trade_run_r |  | float | R-multiple | Worst k-trade run by R: minimum rolling sum of R over window k=5; REPORTED AS NEGATIVE (drawdown sign). If trade_count<5 → NaN. |
| worst_10_trade_run_r |  | float | R-multiple | Worst k-trade run by R over window k=10; REPORTED AS NEGATIVE (drawdown sign). If trade_count<10 → NaN. |
| worst_20_trade_run_r |  | float | R-multiple | Worst k-trade run by R over window k=20; REPORTED AS NEGATIVE (drawdown sign). If trade_count<20 → NaN. |
| edr_100_trades_p50_r | EDR100 — Median (R) | float | R-multiple | EDR100 (Median): median MaxDD of cumulative R over a 100-trade horizon from stationary bootstrap on trades; REPORTED AS NEGATIVE. |
| edr_100_trades_p95_r | EDR100 — P95 (R) | float | R-multiple | EDR100 (P95): 95th percentile (more conservative) of MaxDD of cumulative R over a 100-trade horizon; REPORTED AS NEGATIVE. |
| prob_maxdd_100trades_le_5r | Pr[MaxDD ≤ −5R] (100 trades) | float | decimal |  |
| prob_maxdd_100trades_le_7r | Pr[MaxDD ≤ −7R] (100 trades) | float | decimal |  |
| prob_maxdd_100trades_le_10r | Pr[MaxDD ≤ −10R] (100 trades) | float | decimal |  |
| losing_streak_max_p50_100trades | Max Losing Streak — P50 (Trades) | int | trades | Median of the maximum consecutive losing-trade streak over a 100-trade horizon from stationary bootstrap on the R sequence. |
| losing_streak_max_p95_100trades | Max Losing Streak — P95 (Trades) | int | trades | 95th percentile of the maximum consecutive losing-trade streak over a 100-trade horizon from stationary bootstrap on the R sequence. |
| prob_losing_streak_ge_7_100trades | Pr[Losing Streak ≥ 7] (100 trades) | float | decimal | Probability (share of paths) from stationary bootstrap on the trade-level R sequence. |
| prob_losing_streak_ge_10_100trades | Pr[Losing Streak ≥ 10] (100 trades) | float | decimal | Probability (share of paths) from stationary bootstrap on the trade-level R sequence. |
| holding_period_mean_minutes | Avg Holding Period (Minutes) | float | minutes | Average duration of closed trades in calendar minutes: mean((exit_ts - entry_ts)/60). Closed trades only; UTC. |
| holding_period_median_minutes | Median Holding Period (Minutes) | float | minutes | Median duration of closed trades (minutes, UTC). |
| holding_period_p95_minutes | Holding Period — P95 (Minutes) | float | minutes | 95th percentile of closed-trade durations (minutes, UTC). |
| holding_period_mean_minutes_wins | Avg Holding (Minutes, Wins) | float | minutes | Average duration of winning trades (R > +eps), in minutes (UTC). |
| holding_period_mean_minutes_losses | Avg Holding (Minutes, Losses) | float | minutes | Average duration of losing trades (R < -eps), in minutes (UTC). |


## yearly_summary.csv

### Columns

| name | label | type | units | desc |
| --- | --- | --- | --- | --- |
| year |  | int | YYYY | Calendar year (YYYY). |
| annual_return_calendar | Return (Calendar Year / YTD) | float | decimal | Π(1+ret_m_year) - 1. |
| eom_max_drawdown_intra_year | Max Drawdown (EoM, Year) | float | decimal | Maximum drawdown on end-of-month (EoM) monthly equity within the calendar year (peak resets on Jan 1, UTC): min(dd_t); reported as a negative decimal. |
| intramonth_max_drawdown_intra_year | Max Drawdown (Intramonth, Year) | float | decimal | Maximum drawdown on the Intramonth (path-level) equity within the calendar year (peak resets on Jan 1, UTC): min(dd_t); reported as a negative decimal. |
| trade_count | Trades — Count | int | count | Trades in the year. |
| win_rate | Win Rate | float | decimal | Share of R>0 among trades with R≠0; denominator excludes zero-R trades. |
| profit_factor | Profit Factor | float |  | PF on sums within the year; zeros (R=0) are excluded from both sums. |
| active_months_count | Active Months — Count | int | months | Number of active months in the year (trade_count>0). |
| active_months_share | Active Months — Share | float | decimal | Active months share in the year = active_months_count/12. |
| annual_return_active | Return (Active Months — Supp.) | float | decimal | Return computed on active months only (supplementary). |
| volatility_active_annualized | Volatility (Annualized, Active Months — Supp.) | float | decimal (0.012 = 1.2%) | Annualized vol on active months (ddof=1)*sqrt(12). |
| sharpe_active_annualized | Sharpe (Annualized, Active Months — Supp.) | float |  | Sharpe on active months; rf_m if supplied; sqrt(12). |
| sortino_active_annualized | Sortino (Annualized, Active Months — Supp.) | float |  | Sortino on active months; default target 0; sqrt(12). |
| cagr_active | CAGR (Active Months — Supp.) | float | decimal (0.012 = 1.2%) | Active-only CAGR: (Π(1+r_active))^(12/N_active)-1. |
| insufficient_months |  | bool |  | True if months<12. |
| insufficient_active_months |  | bool |  | True if active_months_count < supplementary_active_only_metrics.min_active_months_for_metrics. |
| insufficient_negative_months |  | bool |  | True if negative months < 2 (affects Sortino). |
| insufficient_trades |  | bool |  | True if trade_count < minimum_sample_policy.min_trades_per_year_warning. |
| volatility_annualized_year | Volatility (Calendar Year, Annualized) | float | decimal (0.012 = 1.2%) | Annualized volatility within the calendar year (ddof=1)*sqrt(12). |
| sharpe_ratio_annualized_year | Sharpe (Calendar Year, Annualized) | float |  | Sharpe Ratio within the calendar year; rf_m if supplied; sqrt(12). |
| sortino_ratio_annualized_year | Sortino (Calendar Year, Annualized) | float |  | Sortino within the calendar year; default target 0; sqrt(12). |
| calmar_ratio_year | Calmar Ratio (EoM, Calendar Year) | float |  | annual_return_calendar / \|eom_max_drawdown_intra_year\|; Calmar-style ratio for the calendar year. |
| negative_months_in_year | Negative Months — Count (Calendar Year) | int | months | Number of months with monthly_return<0 in the calendar year. |
| months_in_year_available | Months in Year — Available | int | months | Number of monthly observations available in the calendar year (1..12). |
| is_ytd | YTD (Partial Year) | bool |  | True only for the latest calendar year if months_in_year_available < 12; false for prior partial years. |
| positive_months_in_year | Positive Months — Count (Calendar Year) | int | months | Number of months with monthly_return>0 in the calendar year. |
| omega_ratio_year_target_0m | Omega (τ=0, Year) | float |  | Omega(τ=0) within the calendar year. |
| monthly_var_95_year | Monthly VaR 95% (Year) | float | decimal | Historical monthly VaR95 computed on the year’s monthly returns; reported as a negative decimal monthly return. |
| monthly_es_95_year | Monthly ES 95% (Year) | float | decimal | Historical monthly ES95 computed on the year’s monthly returns; reported as a negative decimal monthly return. |
| year_start_nav_ccy_raw | Year Start NAV (Raw) | float | USD | Observed previous-December EoM NAV used as raw opening of the calendar year (may be null for the first year). |
| opening_cashflow_ccy | Opening Cashflow (Year) | float | USD | Opening cash flow applied at the start of the calendar year (e.g., due to rebase/cash-out). |
| year_end_nav_ccy_raw | Year End NAV (Raw) | float | USD | Observed current-December EoM NAV used as raw closing of the calendar year. |


# definitions


- **active_month**: Boolean flag per month: True if at least one trade occurred in the calendar month (trade_count>0).
- **active_months_count**: Number of months with active_month=True within the aggregation scope (year or full period).
- **active_months_share**: Share of active months within the aggregation scope = active_months_count / total months in scope.
- **annual_return_active**: Annual Return (Active Months): product over active months only minus 1; supplementary.
- **annual_return_calendar**: Within-year return from monthly series: Π(1+ret_m_year) - 1.
- **average_losing_trade_r**: mean(R | R < -eps).
- **average_winning_trade_r**: mean(R | R > +eps).
- **best_month_return_full_period**: Best single monthly return over the full compounding period.
- **best_year**: Calendar year (YYYY) corresponding to best_year_return_calendar_full_period.
- **best_year_return_calendar_full_period**: Maximum calendar-year return observed over the full period (based on annual_return_calendar).
- **block_mean_length_months**: Expected block length L (months). In runtime this may be a single integer (e.g., 6) or a list for sensitivity (e.g., [3,6,9]). In CSV output each row carries a scalar L in 'block_mean_length_months'. For stationary bootstrap the restart probability is p=1/L.
- **cagr_active**: Active-only CAGR: (Π(1+r_m_active))^(12/N_active) - 1; N_active is number of active months.
- **cagr_annualized_p05**: 5th percentile of annualized CAGR across simulated paths over the given horizon.
- **cagr_annualized_p50**: 50th percentile (median) of annualized CAGR across simulated paths over the given horizon.
- **cagr_annualized_p95**: 95th percentile of annualized CAGR across simulated paths over the given horizon.
- **cagr_annualized_pXX**: Percentile of annualized CAGR across bootstrap paths: ((Π(1+r))^(12/H) − 1).
- **cagr_full_period**: (Π(1+monthly_return))^(12/N) - 1, where N is the number of months.
- **calmar_active**: Not defined. The Calmar ratio is reported only on the calendar series (Full Period and Calendar Year) using EoM drawdowns; do not compute for active months.
- **calmar_ratio_full_period**: Calmar Ratio (EoM, Full Period): cagr_full_period / |eom_max_drawdown_full_period| computed on the EoM monthly equity.
- **calmar_ratio_year**: Calmar Ratio (EoM, Calendar Year): annual_return_calendar / |eom_max_drawdown_intra_year|. If |eom_max_drawdown_intra_year| < eps_den: +Inf if annual_return_calendar > 0; −Inf if < 0; NaN if |annual_return_calendar| < eps_zero.
- **ci_high**: Upper bound of the confidence interval in the metric’s native units and sign conventions.
- **ci_level_pct**: Confidence level in percent (e.g., 90).
- **ci_low**: Lower bound of the confidence interval in the metric’s native units and sign conventions.
- **ci_method**: Confidence-interval method used in confidence_intervals.csv.method: 'bootstrap_percentile' | 'bootstrap_bca'.
- **cond_es_maxdd_ge_kx_risk**: Conditional expected depth of EoM MaxDD in R-multiples given that the breach threshold k×risk has been exceeded at least once within the horizon. NaN if no paths breached.
- **dd_episodes_count**: Count of CLOSED underwater episodes (from equity peak to recovery to new high) on the EoM monthly grid during the full period.
- **dd_observations_count**: Number of months with dd_t<0 on the EoM monthly grid used to compute drawdown depth quantiles over the full period.
- **dd_thresholds_eom**: List of absolute EoM drawdown thresholds θ (decimals) used to compute probabilities Pr[MaxDD_eom ≥ θ]. Example: [0.05, 0.07, 0.10].
- **dd_thresholds_eom_mode**: Enum switch for Monte Carlo exceedance probabilities of EoM MaxDD: 'normalized' (θ = k × risk_per_trade_pct), 'absolute' (θ are fixed decimals), or 'both' (compute and publish both families).
- **dd_thresholds_eom_normalized_multipliers**: Optional list of multipliers k for risk-normalized thresholds: θ = k × runtime.risk_per_trade_pct. Example: [5, 7, 10].
- **downside_deviation_annualized_full_period**: Annualized downside deviation of monthly returns with target=0% annual converted to monthly; ddof=1; multiplied by sqrt(12).
- **drawdown_curve_definition**: Define monthly equity curve eq_t = Π_{k≤t}(1+r_k). Monthly drawdown dd_t = eq_t / cummax(eq_t) - 1 (≤0). Underwater periods are contiguous spans with dd_t<0; a period ends when a new high is made.
- **drawdown_p90_full_period**: 90th percentile of monthly drawdown depths dd_t for months with dd_t<0 on the EoM monthly grid; negative decimal.
- **drawdown_p95_full_period**: 95th percentile of monthly drawdown depths dd_t for months with dd_t<0 on the EoM monthly grid; negative decimal.
- **drawdown_p99_full_period**: 99th percentile of monthly drawdown depths dd_t for months with dd_t<0 on the EoM monthly grid; negative decimal.
- **edr_100_trades_p50_r**: EDR100 (Median): median MaxDD of cumulative R over 100 trades from a stationary bootstrap on the R sequence; reported as negative.
- **edr_100_trades_p95_r**: EDR100 (P95): 95th percentile of MaxDD of cumulative R over 100 trades from a stationary bootstrap; reported as negative.
- **ending_nav_full_period**: Ending NAV over the full period on the compounding track: NAV0 * product(1 + monthly_return). With NAV0=starting_nav_ccy (anchor).
- **ending_nav_p05**: 5th percentile of Ending NAV over the horizon given NAV0=starting_nav_ccy (USD).
- **ending_nav_p50**: Median of Ending NAV over the horizon given NAV0=starting_nav_ccy (USD).
- **ending_nav_p95**: 95th percentile of Ending NAV over the horizon given NAV0=starting_nav_ccy (USD).
- **eom_longest_underwater_months**: Longest underwater spell on the EoM monthly equity measured from the most recent peak (dd=0) preceding the spell to the first recovery (dd_t ≥ 0), inclusive of the recovery month; if no recovery occurs, measure to the last month with dd_t < 0. Reported as integer months.
- **eom_max_drawdown_full_period**: Maximum drawdown on end-of-month (EoM) monthly equity: min(dd_t) where dd_t = eq_t / cummax(eq_t) - 1 ≤ 0; reported as a negative decimal.
- **eom_max_drawdown_intra_year**: Maximum drawdown on end-of-month (EoM) monthly equity within the calendar year (peak resets on Jan 1, UTC): min(dd_t); reported as a negative decimal.
- **eom_months_since_maxdd_trough**: Months since the maximum drawdown trough on the EoM monthly equity: exclude the trough month and include the period end month (last available month). Reported as integer months.
- **eom_time_to_recover_months**: Time to recover for the maximum drawdown on the end-of-month (EoM) monthly equity: number of months from the trough month to the first month with dd_t ≥ 0, inclusive of the recovery month; if recovery has not occurred by period_end_month, report NaN.
- **estimate**: Point estimate of the metric for the CI row, computed on the original sample (not the bootstrap mean). Reported in the same units and sign conventions as the base metric.
- **expectancy_mean_r**: mean(R).
- **expectancy_median_r**: median(R).
- **flow_aware_ending_value_ccy**: Ending value for the investor: observed_last_eom_nav_ccy + total_cash_out_ccy − total_cash_in_ccy. Equivalent: observed_last_eom_nav_ccy − net_flows_ccy.
- **flow_aware_wealth_multiple**: flow_aware_ending_value_ccy / starting_nav_ccy.
- **gain_to_pain_ratio_monthly_full_period**: Sum of positive monthly returns divided by absolute sum of negative monthly returns; zeros excluded; apply generic_ratio.
- **holding_period_mean_minutes**: Average duration of closed trades: mean((exit_ts - entry_ts)/60) in calendar minutes (UTC).
- **holding_period_mean_minutes_losses**: Average duration of losing trades (R < -eps) in minutes (UTC).
- **holding_period_mean_minutes_wins**: Average duration of winning trades (R > +eps) in minutes (UTC).
- **holding_period_median_minutes**: Median duration of closed trades in calendar minutes (UTC).
- **holding_period_p95_minutes**: 95th percentile of closed-trade durations in calendar minutes (UTC).
- **horizon_months**: Simulation horizon in months. Runtime accepts integers (e.g., 12, 36) and the alias 'full_period' (resolved to N months in the dataset). In CSV output 'horizon_months' is always the resolved integer.
- **insufficient_active_months**: Flag: True if active months are below supplementary_active_only_metrics.min_active_months_for_metrics for the scope.
- **insufficient_months**: Flag: True if number of months in scope is below the minimum for stable annualization (see minimum_sample_policy).
- **insufficient_negative_months**: Flag: True if the count of months with r_m below the target within the scope is < 2 (affects Sortino).
- **insufficient_trades**: Flag: True if trade_count in the calendar year is below methodology.minimum_sample_policy.min_trades_per_year_warning.
- **intramonth_drawdown_curve_definition**: Along the Intramonth (path-level) equity, define dd_t = equity_t / cummax(equity_t) - 1 with cummax taken over time (UTC).
- **intramonth_equity_path**: Path-level equity built from all available equity observations sorted by timestamp in UTC (stable, input order on ties). No resampling to month-end.
- **intramonth_longest_underwater_months**: Longest underwater spell on the Intramonth path measured from the last peak (dd = 0) to the first recovery (dd >= 0), inclusive of the recovery. Duration in months = months_between(peak_ts, recovery_ts) + 1; if no recovery, measure to the last negative observation without +1.
- **intramonth_max_drawdown_full_period**: Minimum dd_t on the Intramonth path over the full period. Reported as a negative decimal.
- **intramonth_max_drawdown_intra_year**: Minimum dd_t on the Intramonth path within the calendar year; the peak resets on January 1 (UTC). Reported as a negative decimal.
- **intramonth_months_since_maxdd_trough**: Full months from the MaxDD trough to the last available timestamp on the Intramonth path: months_between(trough_ts, last_ts). The trough month is excluded; the end month is included.
- **intramonth_time_to_recover_months**: Time to recover for the MaxDD on the Intramonth path: months_between(trough_ts, recovery_ts) + 1. If recovery never occurs by period end, report NaN.
- **kurtosis_excess_full_period**: Excess kurtosis (kurtosis minus 3) of monthly returns (ddof=1 for standardization).
- **losing_streak_max_p50_100trades**: Median (P50) of the maximum consecutive losing-trade streak over a 100-trade horizon (in trades).
- **losing_streak_max_p95_100trades**: P95 of the maximum consecutive losing-trade streak over a 100-trade horizon (in trades).
- **martin_ratio_full_period**: Martin Ratio (EoM, Full Period): cagr_full_period / ulcer_index_full_period; follow NaN/Inf denominator policy.
- **max_consecutive_down_months_full_period**: Maximum number of consecutive months with monthly_return<0 in the full period; zeros break streaks.
- **max_consecutive_up_months_full_period**: Maximum number of consecutive months with monthly_return>0 in the full period; zeros break streaks.
- **max_drawdown_magnitude_p95**: 95th percentile of maximum drawdown magnitude on the EoM monthly grid across simulated paths; positive decimal.
- **max_drawdown_magnitude_p99**: 99th percentile of maximum drawdown magnitude on the EoM monthly grid across simulated paths; positive decimal.
- **max_drawdown_magnitude_pXX**: Percentile of maximum drawdown magnitude on the EoM monthly grid across simulated paths; reported as positive decimal.
- **mc_cap_nav_ccy**: Cap level (currency) used by the policy (NAV is trimmed to this level when checked).
- **mc_cap_tolerance_ccy**: Absolute tolerance (currency) below which no trimming is applied.
- **mc_method**: Bootstrap method used in monte_carlo_summary.csv.method: 'stationary_bootstrap' | 'moving_block_bootstrap'.
- **mc_policy**: Monte-Carlo policy name applied to simulated NAV paths (e.g., cap_nav).
- **mc_policy_apply**: How the policy is checked on the path (yearly or monthly).
- **mc_policy_start_month**: Month number (1..12) where a yearly policy check is performed.
- **mc_probability_conventions**: Event probabilities (fields prefixed with 'prob_') are reported as shares of simulation paths in [0,1]. If no paths breached a given threshold, the breach probability is 0.0 and the complement 'prob_no_breach_*' equals 1.0.
- **mc_ttb_conventions**: Time-to-breach metrics (fields prefixed with 'mc_ttb_') are conditional on the event. If no paths breached the threshold within the horizon, these statistics (e.g., mc_ttb_ge_10x_risk_p50) are reported as NaN.
- **mc_ttb_ge_kx_risk_p50**: Median number of months to the first EoM drawdown breach of |dd| ≥ k×risk among simulated paths where such a breach occurs; computed per horizon and block-length L. NaN if no simulation paths breached the threshold on the horizon.
- **mean_monthly_return_full_period**: Arithmetic mean of monthly returns over the full compounding period.
- **median_monthly_return_full_period**: Median of monthly returns over the full compounding period.
- **method**: Bootstrap method used to generate simulated monthly return paths (e.g., stationary_bootstrap or moving_block_bootstrap).
- **metric_basis**: Computation basis of the metric for CI: 'monthly' | 'eom' | 'intramonth'.
- **month**: Month key used in rolling CSVs as a month-anchored date (YYYY-MM-01 UTC). In monthly_returns.csv, the 'month' column is an integer 1–12 used together with 'year'.
- **monthly_es_95**: Historical (empirical) Expected Shortfall at 95% on monthly returns: the mean of r among observations r ≤ VaR95.
- **monthly_es_95_full_period**: Historical monthly ES at 95%: mean(r <= VaR95) with the same quantile method; negative decimal.
- **monthly_es_95_full_period_xrisk**: Risk-normalized magnitude: |monthly_es_95_full_period| / risk_per_trade_pct; reported as an R-multiple (positive).
- **monthly_es_99**: Historical Expected Shortfall at 99% on monthly returns: the mean of r among observations r ≤ VaR99.
- **monthly_es_99_full_period**: Historical (empirical) monthly ES at 99% on full-period monthly returns: the mean of r where r ≤ VaR99 (tail mean), using the same quantile_method='linear'. Reported as a negative decimal monthly return (≤0).
- **monthly_es_99_full_period_xrisk**: Risk-normalized magnitude: |monthly_es_99_full_period| / risk_per_trade_pct; reported as an R-multiple (positive).
- **monthly_return**: Monthly return from EoM NAV; decimal units; Months with no trade_count: monthly_return=0. If there are no trades and NAV is unchanged ⇒ monthly_return=0; if trades occurred but net monthly PnL is exactly 0 (NAV unchanged) ⇒ monthly_return=0.
- **monthly_var_95**: Historical (empirical) Value-at-Risk at 95% on monthly returns: the 5th percentile of r; typically a negative decimal return.
- **monthly_var_95_full_period**: Historical monthly VaR at 95% on monthly returns using quantile_method='linear' (HF type 7); reported as a negative decimal.
- **monthly_var_95_full_period_xrisk**: Risk-normalized magnitude: |monthly_var_95_full_period| / risk_per_trade_pct; reported as an R-multiple (positive).
- **monthly_var_99**: Historical Value-at-Risk at 99% on monthly returns: the 1st percentile of r.
- **monthly_var_99_full_period**: Historical (empirical) monthly VaR at 99% on full-period monthly returns: the 1st percentile of r computed with quantile_method='linear'. Reported as a negative decimal monthly return (≤0).
- **monthly_var_99_full_period_xrisk**: Risk-normalized magnitude: |monthly_var_99_full_period| / risk_per_trade_pct; reported as an R-multiple (positive).
- **months**: Number of monthly observations over the full period.
- **months_between**: Calendar month difference used for Intramonth durations: months_between(a, b) = (year_b - year_a) * 12 + (month_b - month_a). No +1 at the boundary unless explicitly stated by the metric.
- **months_in_year_available**: Number of monthly observations available in the calendar year (1..12).
- **moving_block_bootstrap**: Moving block bootstrap: resample fixed-length overlapping blocks of returns (length L months) with replacement; preserves short-run dependence.
- **n_paths**: Number of simulated bootstrap paths.
- **negative_months_count_12m**: Count of months with r_m<0 in the last 12 months (rolling window).
- **negative_months_count_36m**: Count of months with r_m<0 in the last 36 months (rolling window).
- **negative_months_count_full_period**: Count of months with monthly_return < 0 over the full period.
- **negative_months_in_year**: Count of months with monthly_return < 0 within the calendar year.
- **negative_months_share_12m**: Share of negative months in the 12M window = negative_months_count_12m/12.
- **negative_months_share_36m**: Share of negative months in the 36M window = negative_months_count_36m/36.
- **net_flows_ccy**: Net flows = total_cash_in_ccy - total_cash_out_ccy.
- **newey_west_p_value_mean_monthly_return**: Two-sided p-value for the Newey–West t-statistic under asymptotic normal approximation.
- **newey_west_tstat_mean_monthly_return**: HAC-robust t-statistic for the mean monthly return using Newey–West standard error with Bartlett kernel and lag q.
- **next_first_month**: Month (YYYY-MM) of the first closed-balance snapshot immediately after the year boundary (usually January).
- **next_first_nav**: NAV at the first closed-balance snapshot after the boundary (raw, before rebase).
- **observed_last_eom_nav_ccy**: Observed NAV at the last end-of-month snapshot.
- **omega_ratio**: For monthly returns r_m and a monthly threshold τ: Omega(τ) = mean(max(r_m − τ, 0)) / mean(max(τ − r_m, 0)). Shows the balance of gains vs losses around τ. Apply methodology.nan_inf_policy.generic_ratio to the denominator.
- **omega_ratio_full_period_target_0m**: Omega(τ=0) over the full compounding period: Omega(0) = mean(max(r_m − 0, 0)) / mean(max(0 − r_m, 0)) computed on monthly returns. Require at least one observation above and below τ; otherwise report NaN or ±Inf per methodology.nan_inf_policy.generic_ratio.
- **omega_ratio_year_target_0m**: Omega(τ=0) computed within the calendar year on that year's monthly returns. Require at least one observation above and below τ; otherwise apply methodology.nan_inf_policy.generic_ratio.
- **opening_cashflow_ccy**: Opening cash flow at the start of the new calendar year, derived from the boundary between the last closed-balance of the prior year and the first snapshot of the new year. Positive = cash-in (deposit), negative = cash-out (withdrawal).
- **pain_index_full_period**: Mean magnitude of EoM drawdowns: mean(|dd_t|) over months with dd_t<0 on the EoM grid; positive decimal.
- **pain_ratio_full_period**: CAGR (Full Period) divided by pain_index_full_period. Apply methodology.nan_inf_policy.generic_ratio.
- **payoff_ratio**: average_winning_trade_r / |average_losing_trade_r|.
- **peak_anchor_policy**: Underwater spells start at the most recent peak (dd = 0) immediately preceding the first dd < 0 within the spell.
- **period_end_month**: Last month in the dataset (month-anchored date, UTC).
- **period_start_month**: First month in the dataset (month-anchored date, UTC).
- **policy_avg_cashout_events**: Average number of policy-triggered cash-out events per MC path.
- **policy_ending_nav_ccy_p05**: P5 of ending NAV (currency) after applying the policy on MC paths.
- **policy_ending_nav_ccy_p50**: Median of ending NAV (currency) after applying the policy on MC paths.
- **policy_ending_nav_ccy_p95**: P95 of ending NAV (currency) after applying the policy on MC paths.
- **policy_flow_aware_ending_value_ccy_p05**: P5 of flow-aware ending value = ending NAV + total cash-out (currency).
- **policy_flow_aware_ending_value_ccy_p50**: Median of flow-aware ending value = ending NAV + total cash-out (currency).
- **policy_flow_aware_ending_value_ccy_p95**: P95 of flow-aware ending value = ending NAV + total cash-out (currency).
- **policy_flow_aware_multiple_p05**: P5 of flow-aware multiple = flow-aware ending value / starting NAV.
- **policy_flow_aware_multiple_p50**: Median of flow-aware multiple = flow-aware ending value / starting NAV.
- **policy_flow_aware_multiple_p95**: P95 of flow-aware multiple = flow-aware ending value / starting NAV.
- **policy_prob_any_cashout**: Share of MC paths with at least one policy-triggered cash-out.
- **policy_total_cash_out_ccy_p05**: P5 of total cash-out (currency) across policy-aware MC paths.
- **policy_total_cash_out_ccy_p50**: Median of total cash-out (currency) across policy-aware MC paths.
- **policy_total_cash_out_ccy_p95**: P95 of total cash-out (currency) across policy-aware MC paths.
- **positive_months_count_12m**: Count of months with r_m>0 in the last 12 months (rolling window).
- **positive_months_count_36m**: Count of months with r_m>0 in the last 36 months (rolling window).
- **positive_months_count_full_period**: Count of months with monthly_return > 0 over the full period.
- **positive_months_in_year**: Count of months with monthly_return > 0 within the calendar year.
- **positive_months_share_12m**: Share of positive months in the 12M window = positive_months_count_12m/12.
- **positive_months_share_36m**: Share of positive months in the 36M window = positive_months_count_36m/36.
- **prev_eom_month**: Month (YYYY-MM) of the last closed-balance snapshot immediately before the year boundary (usually December).
- **prev_eom_nav**: NAV at the last closed-balance snapshot before the boundary (raw, before rebase).
- **prob_losing_streak_ge_7_100trades**: Probability that the maximum losing streak over 100 trades is ≥ 7 (share of bootstrap paths; decimal).
- **prob_maxdd_100trades_le_10r**: Probability that trade-equity MaxDD over 100 trades is ≤ −10R (share of bootstrap paths; decimal).
- **prob_maxdd_100trades_le_5r**: Probability that trade-equity MaxDD over 100 trades is ≤ −5R (share of bootstrap paths; decimal).
- **prob_maxdd_100trades_le_7r**: Probability that trade-equity MaxDD over 100 trades is ≤ −7R (share of bootstrap paths; decimal).
- **prob_maxdd_ge_10pc_eom**: Probability (share of bootstrap paths) that EoM maximum drawdown magnitude over the horizon is ≥ 10%. Equals 0.0 if no paths breached.
- **prob_maxdd_ge_10x_risk_eom**: Probability that EoM maximum drawdown magnitude over the horizon is ≥ 10×runtime.risk_per_trade_pct. Equals 0.0 if no paths breached.
- **prob_maxdd_ge_20pc_eom**: Probability that EoM MaxDD over the horizon is ≥ 20%.
- **prob_maxdd_ge_30pc_eom**: Probability that EoM MaxDD over the horizon is ≥ 30%.
- **prob_maxdd_ge_5pc_eom**: Probability (share of bootstrap paths) that EoM maximum drawdown magnitude over the horizon is ≥ 5%. Equals 0.0 if no paths breached.
- **prob_maxdd_ge_5x_risk_eom**: Probability (share of MC paths) that the EoM maximum drawdown over the horizon is ≥ 5×risk. Equals 0.0 if no paths breached.
- **prob_maxdd_ge_7pc_eom**: Probability (share of bootstrap paths) that EoM maximum drawdown magnitude over the horizon is ≥ 7%. Equals 0.0 if no paths breached.
- **prob_maxdd_ge_7x_risk_eom**: Probability that EoM maximum drawdown magnitude over the horizon is ≥ 7×runtime.risk_per_trade_pct. Equals 0.0 if no paths breached.
- **prob_negative_horizon_return**: Share of simulated paths with cumulative horizon return below 0 (decimal).
- **prob_no_breach_ge_kx_risk_eom**: Complement probability to breach at threshold k×risk over the horizon: 1 − prob_maxdd_ge_kx_risk_eom. Equals 1.0 if no paths breached.
- **profit_factor**: PF = sum(R>+eps) / |sum(R<-eps)|; trades with |R| ≤ eps are excluded from both sums.
- **r**: R = pnl_pct / risk_per_trade_pct (risk_per_trade_pct from runtime/parameters).
- **r_max**: max(R).
- **r_min**: min(R).
- **r_multiple_units**: R-multiple (value divided by risk_per_trade_pct; dimensionless).
- **r_std_dev**: stdev(R, ddof=1).
- **rebase_audit_note**: When Dec→Jan boundary gaps exceed tolerance, a multiplicative rebase is applied to the Jan+ segment to maintain flow-neutral returns. An audit log (rebasing_applied.csv).
- **risk_per_trade_pct**: Per-trade risk budget (decimal of equity) effective at runtime for the simulation/output row.
- **rolling_calmar_12m**: Rolling 12M Calmar: rolling_return_12m / |rolling_max_drawdown_12m|; NaN/Inf policy applies.
- **rolling_calmar_36m**: Rolling Calmar: rolling_return_annualized_36m / |rolling_max_drawdown_36m|; NaN/Inf policy applies.
- **rolling_max_drawdown_12m**: Maximum drawdown on the 12-month EoM sub-curve; reported as a negative return.
- **rolling_max_drawdown_36m**: Maximum drawdown on the 36-month EoM sub-curve; reported as a negative return.
- **rolling_return_12m**: Rolling cumulative return over the last 12 months: product(1+r_m) - 1.
- **rolling_return_annualized_36m**: Annualized return over the last 36 months: (product(1+r_m))^(12/N)-1, N=36.
- **rolling_sharpe_annualized_12m**: Annualized Sharpe using last 12 months (rf=0 by default): mean(r_m)/stdev(r_m, ddof=1)*sqrt(12).
- **rolling_sharpe_annualized_36m**: Annualized Sharpe on the last 36 months (rf=0 by default): mean(r_m)/stdev(r_m, ddof=1)*sqrt(12).
- **rolling_sortino_annualized_12m**: Annualized Sortino on the last 12 months: mean excess over target divided by downside stdev computed from min(r_m−t_m,0); ddof=1; ×sqrt(12).
- **rolling_sortino_annualized_36m**: Annualized Sortino on the last 36 months: downside via min(r_m−t_m,0); ddof=1; ×sqrt(12).
- **rolling_volatility_annualized_12m**: Annualized volatility using last 12 months: stdev(r_m, ddof=1)*sqrt(12).
- **rolling_volatility_annualized_36m**: Annualized volatility on the last 36 months: stdev(r_m, ddof=1)*sqrt(12).
- **rounding_family**: Formatting group used to mirror the rounding of the underlying metric.
- **seed**: Random seed used for reproducible simulation runs (optional).
- **sharpe_active_annualized**: Sharpe on active months only; (mean(r_m - rf_m)/stdev(r_m - rf_m, ddof=1))*sqrt(12).
- **sharpe_ratio_annualized_full_period**: (mean(r_m - rf_m) / stdev(r_m - rf_m, ddof=1)) * sqrt(12); rf=0 by default.
- **sharpe_ratio_annualized_year**: Sharpe Ratio (annualized) within a calendar year: (mean(r_m - rf_m) / stdev(r_m - rf_m, ddof=1)) * sqrt(12), where r_m are monthly returns of that year.
- **skewness_full_period**: Skewness of monthly returns (use sample standard deviation ddof=1 in standardization).
- **sortino_active_annualized**: Sortino on active months only; downside via min(r_m - t_m,0) with ddof=1; sqrt(12) annualization.
- **sortino_ratio_annualized_full_period**: Sortino Ratio (Annualized): Let monthly returns be r_m and target t_m. Numerator = mean(r_m - t_m). Denominator (downside deviation) = sqrt(sample_mean( min(r_m - t_m, 0)^2 )), ddof=1 on negatives. Annualization via sqrt(12): Sortino_ann = (mean(r_m - t_m) / downside_std_m) * sqrt(12). Default target t_m = 0. If an annual target t_ann is supplied (e.g., MAR or rf), use t_m = (1 + t_ann)^(1/12) - 1. Reporting policy: downside_std_m requires at least two negative monthly observations (ddof=1); if fewer than two negatives, Sortino is undefined and must be reported as NaN.
- **sortino_ratio_annualized_year**: Sortino Ratio (annualized) within a calendar year: target t_m per sortino_target_policy (default 0); downside uses min(r_m - t_m, 0) with ddof=1 on negatives; sqrt(12) annualization.
- **starting_nav_ccy**: Configurable starting capital (currency). Used to scale currency-valued outputs and to compute flow_aware_wealth_multiple = flow_aware_ending_value_ccy / starting_nav_ccy. (compounding only)
- **stationary_bootstrap**: Stationary bootstrap (Politis–Romano): resample returns by stitching blocks whose lengths are geometrically distributed with mean L months. Preserves serial dependence in expectation.
- **tail_ratio_p95_p5**: Tail Ratio = Q95(r) / |Q5(r)| on the monthly return distribution. Default scope: all calendar months (not active-only) unless explicitly stated.
- **tail_ratio_p95_p5_full_period**: Tail Ratio over the full compounding period: Q95(r_m) / |Q5(r_m)| on monthly returns. Default scope: all calendar months (not active-only); apply methodology.nan_inf_policy.generic_ratio when the denominator is near zero. The primary exported full-period field is 'tail_ratio_p95_p5_full_period'; an active-only variant is exported only when explicitly enabled via runtime.metrics_options.tail_ratio_mode='active'. In runtime.metrics_options, tail_ratio_mode='all' means compute/export the default calendar-months variant (not active-only).
- **total_cash_in_ccy**: Total cash-in over the full period (absolute currency units).
- **total_cash_out_ccy**: Total cash-out over the full period (absolute currency units).
- **total_return_full_period**: Total return over the full period: product(1 + monthly_return) - 1, computed on the compounding monthly series.
- **trade_count**: Total number of executed trades within the aggregation scope of the file (calendar year or full period).
- **trade_count_full_period**: Total number of executed trades over the full period (all months).
- **trough_tie_breaker_policy**: If the drawdown minimum occurs multiple times, select the earliest occurrence (first-in-time trough).
- **ulcer_index_full_period**: Ulcer Index: sqrt(mean(dd_t^2)) computed over months with dd_t<0 on the EoM monthly grid; positive decimal.
- **underwater_duration_p90_full_period**: 90th percentile of closed underwater episode durations in months (rounded half-up).
- **underwater_duration_p95_full_period**: 95th percentile of closed underwater episode durations in months (rounded half-up).
- **underwater_months_share_full_period**: Fraction of months with dd_t<0: dd_observations_count / months (decimal).
- **volatility_active_annualized**: Annualized volatility computed only on active months; stdev(active r_m, ddof=1)*sqrt(12).
- **volatility_annualized_full_period**: stdev(monthly_return, ddof=1) * sqrt(12).
- **volatility_annualized_year**: Annualized volatility computed within a calendar year: stdev(monthly_return_in_year, ddof=1) * sqrt(12).
- **wealth_multiple_full_period**: Wealth multiple over the full compounding period: 1 + total_return_full_period.
- **wealth_multiple_p05**: 5th percentile of the wealth multiple over the horizon (Π(1+r)).
- **wealth_multiple_p50**: Median of the wealth multiple over the horizon (Π(1+r)).
- **wealth_multiple_p95**: 95th percentile of the wealth multiple over the horizon (Π(1+r)).
- **win_rate**: win_rate = count(R>+eps) / count(|R|>eps); trades with |R| ≤ eps are excluded.
- **window_end_month**: Last month included in the rolling window (inclusive; YYYY-MM UTC).
- **window_months**: Size of the rolling window in months.
- **window_start_month**: First month included in the rolling window (inclusive; YYYY-MM UTC).
- **worst_10_trade_run_r**: Worst k-trade run with k=10 per worst_k_trade_run_r; reported as negative.
- **worst_20_trade_run_r**: Worst k-trade run with k=20 per worst_k_trade_run_r; reported as negative.
- **worst_5_trade_run_r**: Worst k-trade run with k=5 per worst_k_trade_run_r; reported as negative.
- **worst_k_trade_run_r**: Worst k-trade run by R: min over overlapping windows of Σ_{i=j..j+k-1} R_i. REPORTED AS NEGATIVE. R=0 are included. If trade_count<k → NaN.
- **worst_month_return_full_period**: Worst single monthly return over the full compounding period.
- **worst_year**: Calendar year (YYYY) corresponding to worst_year_return_calendar_full_period.
- **worst_year_return_calendar_full_period**: Minimum calendar-year return observed over the full period (based on annual_return_calendar).
- **xrisk_normalization**: Risk-normalized (×Risk) metric: value_xrisk = magnitude(value) / risk_per_trade_pct for losses/drawdowns/tails, reported as positive R-multiples unless explicitly signed. For Ulcer and quantiles, divide the (already positive) magnitude by risk_per_trade_pct.
- **year**: Calendar year (YYYY) for yearly rows (UTC-based).
- **year_end_nav_ccy_raw**: Observed EoM NAV of the current December used as raw closing of the calendar year.
- **year_start_nav_ccy_raw**: Observed EoM NAV of the previous December used as raw opening of the calendar year (may be null for the first year).
- **yearly_peak_reset_policy**: For all *_intra_year metrics (EoM and Intramonth), reset the peak at 00:00:00 UTC on January 1 of each calendar year.
- **years_covered**: Number of calendar years represented in the dataset (count of distinct years with at least one month).
- **ytd_flag_policy**: Set is_ytd=true only for the latest calendar year present in the dataset if months_in_year_available < 12; for all prior years is_ytd=false regardless of completeness. period_end_month is the last available month in that year. CI rows for that year use the same YTD range.
- **zero_months_count_full_period**: Number of months with monthly_return == 0 over the full period.

## ci_supported_metrics
- cagr_full_period
- volatility_annualized_full_period
- eom_max_drawdown_full_period
- ulcer_index_full_period
- monthly_var_95_full_period
- monthly_es_95_full_period
- monthly_var_99_full_period
- monthly_es_99_full_period
- intramonth_max_drawdown_full_period
- eom_max_drawdown_intra_year
- intramonth_max_drawdown_intra_year


# metadata

## risk_profiles

- **active_runtime_value**: 0.01
- **default**: risk_1pct

### available
- risk_1pct
- risk_1_5pct

## sample_quality
### warnings
- Annualized metrics may be unstable if months < preferred_min_months_for_annualization.
- Annual metrics flagged if months<12 (insufficient_months=true).
- Active-only metrics flagged if active_months_count below threshold.
- Sortino is undefined (NaN) if negative months < 2 (applies to full-period, yearly, and active-only variants)
- Trade count warning if yearly trade_count < 12.

## sortino_target_used

- **type**: default
- **annual**: 0
- **monthly**: 0
- **source**: sortino_target_policy


# methodology


- **active_month_definition**: Active month = a calendar month with at least one trade (trade_count>0). This is the only criterion; do not infer it from monthly_return.
- **calmar_policy**: Calmar is NOT defined on the active-months subset. Do not compute or publish a Calmar ratio for active-only metrics. Rationale: Calmar pairs a return with a drawdown measured on the same continuous equity basis. The active-month subset is discontinuous and has no well-defined drawdown curve; mixing bases (e.g., CAGR_active with EoM MaxDD) is not permitted.
- **drawdown_curve**: Both bases for drawdowns: (1) EoM (monthly grid) — build eq_t = Π(1 + r_m) with dd_t = eq_t / cummax(eq_t) - 1; (2) Intramonth (path-level) — sort equity points by UTC time, compute dd_t = equity_t / cummax(equity_t) - 1 along the path; convert durations to months via months_between.
- **drawdown_frequency_policy**: EoM metrics are computed strictly on the month-end grid (UTC). Intramonth metrics are computed on the full equity path (UTC); durations are then converted to integer months via months_between. Max drawdown values and dd quantiles are reported as negative decimals; DD-based magnitudes (e.g. Ulcer Index) are reported as positive decimals; durations are integer months. Operationally, maximum drawdown is computed as min(dd_t) on the chosen grid/path; we report the dd value (negative), while DD magnitudes are reported as positive decimals. Durations are integer-valued months but stored as float to allow NaN where applicable.
- **starting_nav_note**: Starting capital is configurable (parameters.starting_nav_ccy or CLI --start-nav). All currency-valued outputs scale accordingly; multiples are invariant.
- **ytd_flag_policy**: is_ytd = true only for the latest calendar year present if months_in_year_available < 12; false for prior years.

## distribution_shape_policy

- **kurtosis_excess**: Compute excess kurtosis (kurtosis-3) using sample standard deviation (ddof=1).
- **skewness**: Compute skewness on monthly returns using sample standard deviation (ddof=1).

## drawdown_quantiles_policy

- **duration**: Durations are computed for CLOSED underwater episodes only (from new high to recovery). Ongoing episode at period end is excluded from duration quantiles.
- **grid**: Monthly end-of-month equity; dd_t = eq_t/cummax(eq_t) - 1; use only months with dd_t<0.
- **magnitude**: drawdown_pNN_full_period are computed on dd_t and reported as negative decimals.
- **period**: Full available dataset (e.g., 2008–2025) unless otherwise specified in the file's start/end fields.
- **quantile_method**: linear

## edge_cases

- **sortino_ratio_annualized**: Downside deviation (ddof=1) needs ≥2 negative months. If count_neg<2 → Sortino = NaN (undefined).

## omega_policy

- **definition**: Omega(τ) = mean(max(r_m − τ, 0)) / mean(max(τ − r_m, 0)) computed on monthly returns.
- **sample_requirements**: Require at least one observation above and below τ; otherwise Omega is NaN or ±Inf per nan_inf_policy.generic_ratio.
- **thresholds**: Default τ = 0 (monthly). Optionally reuse sortino_target_policy to set τ from an annual target (rf/MAR) via τ = (1 + t_ann)^(1/12) − 1.

## trade_duration_policy

- **bar_only_fallback**: If only bar timestamps are available (no execution timestamps), use an approximation: duration_minutes ≈ bars_held * bar_length_minutes (e.g., 5).
- **basis**: Calendar minutes between entry_ts and exit_ts (UTC): duration_minutes = (exit_ts - entry_ts)/60.
- **fills_aggregation**: Duration is computed at the position level: from the first opening fill to the final closing fill; intermediate adds/partials within one position do not create separate trades for duration.
- **rounding**: Store as float minutes in CSV; presentation rounding is controlled by rounding_policy.
- **same_bar_trades**: Trades opened and closed within the same 5-minute bar can yield small durations (including <5 minutes) based on actual execution timestamps.
- **scope**: Closed trades only; positions open at period end are excluded.

## trade_run_policy

- **basis**: Chronological trade sequence (UTC); use raw R per trade, not monthly aggregation.
- **insufficient_sample**: If trade_count < k, return NaN.
- **sign_convention**: All worst-run and EDR figures are reported with a negative sign (drawdown).
- **window**: Rolling sum over window size k; windows are overlapping; R=0 are included.

## trades_bootstrap_policy

- **notes**: Preserves short-run dependence through blocks; R=0 trades are kept.
- **block_mean_length_trades**: Expected block length L in trades; institutional default L=20 (sensitivity 10–30 OK).
- **horizons_trades**: List of trade horizons; for EDR use 100 trades.
- **maxdd_measure**: Maximum drawdown (most negative excursion) of cumulative R over the horizon.
- **method**: Stationary bootstrap (Politis–Romano) on the trade-level R sequence.
- **outputs**: Publish quantiles p50 and p95 (optionally mean).
- **paths**: 10,000 bootstrap paths by default.
- **seed**: Reproducibility via runtime/parameters seed; otherwise system entropy.

## var_es_policy

- **es_tail_rule**: mean of observations r <= VaR_alpha using the same quantile method; no fractional weighting
- **method**: Historical (empirical) VaR/ES on monthly returns; no parametric assumption.
- **quantile_hf_type**: 7
- **quantile_method**: linear
- **reporting**: Report as decimal monthly returns (typically negative).
- **rolling**: For rolling windows, compute within-window; for calendar years, compute on that year's monthly returns.

### levels
- 95
- 99

## xrisk_policy

- **applicability**: Applies to EoM and Intramonth drawdowns, Ulcer Index, monthly VaR/ES, rolling VaR/ES and drawdown, and Monte Carlo MaxDD-related outputs. Not applicable to calendar-year (yearly) metrics; ×Risk metrics are not published for calendar-year outputs.
- **definition**: Express risk-bearing quantities in R-multiples by dividing by runtime.risk_per_trade_pct.
- **labels**: Append '×Risk' to labels to disambiguate from percent/decimal units.
- **magnitudes_positive**: All ×Risk fields are reported as positive magnitudes (R-multiples).
- **trade_series_exception**: Metrics based on the trade-level R sequence (e.g., EDR and worst_k_trade_run_r) keep their native sign.


# benchmark_policy


- **benchmark_policy_note**: Buy-and-hold equity or FX indices (e.g., S&P 500, EURUSD spot) are not used as benchmarks. The strategy is intraday, broadly market-neutral, with near-zero directional EURUSD exposure; index comparisons are non-representative and potentially misleading. Accordingly, performance and risk are reported on an absolute-return basis (rf = 0).


# confidence_intervals_policy


- **engine**: bootstrap_for_metrics

## bootstrap_for_metrics

- **notes**: Quantiles use linear interpolation (HF type 7) across base metrics and bootstrap CIs. Do not mix methods.
- **method**: bootstrap_bca
- **n_boot**: 5000
- **quantile_hf_type**: 7
- **quantile_method**: linear

### intramonth

- **block_mean_length_days**: 5
- **bootstrap_type**: stationary_bootstrap

#### applies_to
- intramonth_max_drawdown_full_period
- intramonth_max_drawdown_intra_year

### monthly_eom

- **block_mean_length_months**: 6
- **bootstrap_type**: stationary_bootstrap

#### applies_to
- cagr_full_period
- volatility_annualized_full_period
- eom_max_drawdown_full_period
- ulcer_index_full_period
- monthly_var_95_full_period
- monthly_es_95_full_period
- monthly_var_99_full_period
- monthly_es_99_full_period
- eom_max_drawdown_intra_year

## levels
- 90


# month_counting_policy


- **longest_underwater_includes_recovery**: The longest underwater stretch is measured peak → first recovery inclusive; if no recovery occurs, measure to the last negative observation. For Intramonth, use months_between(...)+1 only when recovery exists.
- **since_trough_excludes_trough**: Months since trough exclude the trough month and include the period end: EoM = (n - 1) - trough_idx; Intramonth = months_between(trough_ts, last_ts).
- **ttr_includes_recovery**: Time-to-recover (TTR) includes the recovery month: EoM = rec_idx - trough_idx + 1; Intramonth = months_between(trough_ts, recovery_ts) + 1.


# underwater_anchor_policy


- **peak_selection**: Underwater spells start at the most recent peak (dd = 0) before the spell.
- **trough_selection**: MaxDD trough is the first-in-time occurrence of the global dd minimum within the scope.
- **yearly_reset**: For *_intra_year metrics, reset the peak at 00:00:00 UTC on January 1.


# epsilons


- **eps_den**: 1e-12
- **eps_zero**: 1e-12


# minimum_sample_policy


- **cagr_min_months**: 1
- **min_trades_per_year_warning**: 12
- **preferred_min_months_for_annualization**: 12
- **publish_under_min_months**: Compute if formula-defined and return result; also emit a warning flag in metadata.sample_quality.
- **sharpe_ratio_annualized_min_months**: 2
- **sortino_ratio_annualized_min_negative_months**: 2
- **volatility_annualized_min_months**: 2


# monte_carlo_policy


- **type**: cap_nav
- **enabled_flag**: Controlled by runtime.policy.enabled. When false, MC runs ignore policy checks.
- **scope**: Applies only to Monte Carlo (bootstrap) NAV paths; does not change historical/monthly returns.

## cap_nav

- **logic**: After each simulated month is applied to the path NAV, perform the policy check on the end-of-month NAV. If NAV_eom > (cap_nav_ccy + cap_tolerance_ccy), withdraw the excess so that NAV_eom becomes cap_nav_ccy. The withdrawn amount is treated as a positive cash-out event and accumulated.
- **outputs_note**: When runtime.policy.enabled=false, mc_policy='none' and all policy_* columns are populated with NaN per nan_inf_display; probabilities/counters are NaN as non-applicable.
- **randomness**: Policy is deterministic given a simulated path; it does not change the bootstrap sampling, only trims path NAV according to the rule.
- **rationale**: Models systematic profit-taking / investor distributions by capping path NAV at a user-defined level.

### accounting

- **cash_out_sign**: Cash-out amounts are positive currency values.
- **ending_nav**: Ending NAV after withdrawals feeds policy_ending_nav_ccy_* percentiles.
- **flow_aware_value**: Flow-aware ending value = ending NAV + total cash-out; percentiles reported as policy_flow_aware_ending_value_ccy_* and normalized multiple as policy_flow_aware_multiple_*. Under cap_nav policy cash-in events are not generated (cash_in=0 by design), so the general formula NAV + cash_out − cash_in reduces to NAV + cash_out.
- **totals**: Sum of all policy withdrawals per path feeds policy_total_cash_out_ccy_* percentiles.

### apply_modes

- **monthly**: Check after every simulated month.
- **yearly**: Check exactly once per simulated year in month == mc_policy_start_month (1..12).


# naming_conventions


- **note_negative_months**: Negative months are reported per calendar year for both profiles, and additionally as a full-period audit metric for compounding.
- **ui_policy**: Always append the family display_tag to metric labels to avoid ambiguity (e.g., 'Sharpe (Annualized, Full Period)' vs 'Sharpe (Calendar Year, Annualized)').
- **ui_ytd_label_policy**: If is_ytd=True for a given year, append '— YTD' to metric labels in displays. Keys remain unchanged. This is a UI rendering rule; CSV labels themselves do not include '— YTD'.

## families
### active_only_supplementary

- **description**: Supplementary metrics computed on months with trade activity only; not a replacement for calendar metrics.
- **display_tag**: Active Months (Supp.)

### calendar_year_from_monthlies

- **description**: Metrics computed strictly within each calendar year using that year's monthly returns; annualization via sqrt(12) where applicable.
- **display_tag**: Calendar Year

### full_period_from_monthlies

- **description**: Metrics computed over the entire available series of monthly returns; annualization via sqrt(12) where applicable.
- **display_tag**: Full Period


# nan_inf_display


- **nan**: NaN
- **neg_inf**: -Inf
- **pos_inf**: Inf


# nan_inf_policy


- **calmar_ratio_full_period**: If |eom_max_drawdown_full_period| < eps_den: +Inf if cagr_full_period > 0; −Inf if cagr_full_period < 0; NaN if |cagr_full_period| < eps_zero.
- **generic_ratio**: If |den|<eps_den: return +Inf if num>0; -Inf if num<0; NaN if |num|<eps_zero.
- **payoff_ratio**: payoff = avg_win / |avg_loss|. If count_losses=0 and count_wins>0 ⇒ +Inf. If count_wins=0 and count_losses>0 ⇒ 0. If both zero ⇒ NaN.
- **profit_factor**: PF = sum(R>0) / |sum(R<0)|. If |sum(R<0)|<eps_den and sum(R>0)>eps_zero ⇒ +Inf. If sum(R>0)<eps_zero and |sum(R<0)|≥eps_den ⇒ 0. If both sums <eps_zero ⇒ NaN.
- **sharpe_ratio_annualized**: If stdev_m<eps_den: +Inf if mean_excess>0; -Inf if mean_excess<0; NaN if |mean_excess|<eps_zero.
- **sortino_ratio_annualized**: Requires ≥2 negative months (downside sample, ddof=1). If <2 negatives: NaN. If downside_std_m<eps_den with ≥2 negatives: +Inf if mean_excess>0; -Inf if mean_excess<0; NaN if |mean_excess|<eps_zero.


# newey_west_policy


- **notes**: For small samples consider Student-t with df=N−1; default is asymptotic normal commonly used in practice.
- **estimator**: Newey–West HAC standard error with Bartlett kernel.
- **lag_selection**: Runtime parameter newey_west.lag_months; institutional range 3–6 months; default 6.
- **test**: H0: mean monthly return = 0. Use HAC standard error for t-stat; p-value two-sided via asymptotic normal approximation.


# no_oos_policy

OOS/Walk-Forward are not applied: a single fixed EMM M5 logic is used with no re-optimization or curve fitting; the purpose of OOS is to detect overfitting after parameter tuning, which is not the case here. Multiple testing bias is absent: a single hypothesis/strategy was tested with no parameter sweep (selection bias is minimal). Profile robustness is supported by a long history, 12/36-month rolling windows, drawdown quantiles, BCa confidence intervals, and Monte Carlo (stationary bootstrap across all stated configurations). For audit, on request, a holdout with no retraining and no parameter changes can be prepared for independent verification.

# rebasing


- **fixed**: Anchor NAV=100000 at YYYY-01-01 00:00:00 UTC (anchor NAV=100000) for each calendar year.

## compounding

- **notes**: Compounding track: monthly_return = NAV_EoM(m) / NAV_EoM(m-1) - 1; for the first month use the BoM anchor NAV=starting_nav_ccy. EoM NAV is ffilled to month-end if needed. (was: Anchor NAV=starting_nav_ccy at t0-ε (just before first observation).)
- **anchor**: BoM of the first observed month: YYYY-MM-01 00:00:00 UTC; NAV=starting_nav_ccy


# risk_parametrization

R-metrics are scale-invariant given R = pnl_pct / risk_per_trade_pct. Switching risk_per_trade_pct between 1% and 1.5% changes NAV-based metrics (returns/vol/Sharpe/Sortino/MaxDD) but leaves R-based trade metrics comparable.

# rolling_windows


- **alignment**: Windows are end-anchored by month (YYYY-MM-01 UTC).
- **annualization**: Monthly data annualized via sqrt(12) for risk metrics; returns annualized via exponent 12/N where applicable.
- **completeness**: Metrics are computed only for complete windows (>= window_months). Exception: count fields (*_count_*) remain computed even when insufficient_months=true; shares and other ratio-like fields follow the NaN rule.
- **completeness_counts**: For share fields (active/positive/negative months shares), set values to NaN when insufficient_months=true. Count fields remain populated.
- **insufficient_negative_months_policy**: Set insufficient_negative_months=true and Sortino to NaN when negative shortfalls < 2 within the window.
- **nan_inf**: Use global NaN/Inf policy; Calmar uses denominator epsilon guard.
- **sortino_target**: Default target is 0% annual converted to monthly; may be overridden by global sortino_target_policy.
- **variance_ddof**: Sample standard deviation (ddof=1).
- **ytd_note**: Rolling windows always use the last available completed months; YTD status affects only the dataset tail, not window logic.


# sharpe_rf

Sharpe computed with rf=0 unless an rf series is supplied (then use effective monthly rf: (1+rf_ann)^(1/12)-1).

# sortino_target

Target return for Sortino set to 0 by default. If an annual target (MAR or rf) is supplied, convert to monthly: t_m = (1 + t_ann)^(1/12) − 1.

# sortino_target_policy


- **conversion_to_monthly**: t_m = (1 + t_ann)^(1/12) − 1
- **default_target_ann**: 0
- **default_target_label**: zero_annual_return
- **preference**: If both risk_free_ann and mar_ann are provided, prefer mar_ann (conservative) unless a mandate specifies rf.

## allowed_targets
- risk_free_ann
- mar_ann


# streaks_policy


- **grid**: Monthly end-of-month grid (UTC).
- **output**: Report max consecutive up/down months over the full period.
- **thresholds**: Up if r_m>0; down if r_m<0; zero returns break streaks and are not part of either class.


# supplementary_active_only_metrics


- **annualization**: Use N_active in exponents; e.g., CAGR_active = (Π(1+r_active))^(12/N_active) - 1.
- **disclosure**: Active-only metrics are computed using months with active_month=True; not a replacement for calendar metrics.
- **min_active_months_for_metrics**: 6
- **rationale**: In fixed-start profiles the equity resets annually to 100k; cross-year aggregation is not meaningful. Institutions typically avoid active-only Sharpe/Sortino/CAGR for fixed-start; keep only activity counts.
- **sharpe_target**: rf_m if supplied, else 0.
- **sortino_target**: t_m from sortino_target_policy (default 0); downside uses min(r_m - t_m, 0) with ddof=1 on negatives.
- **status**: supplementary_only
- **tail_ratio_scope_note**: Tail Ratio metrics default to all calendar months; an active-only variant may be reported only when explicitly flagged.

## applies_to
- compounding


# ulcer_martin_policy


- **martin_ratio**: CAGR / Ulcer Index with NaN/Inf denominator policy.
- **pain_index**: Pain Index = mean(|dd_t|) over dd_t<0 (EoM).
- **pain_ratio**: Pain Ratio = CAGR / Pain Index; generic_ratio policy.
- **ulcer_index**: Compute RMS of dd_t over months with dd_t<0 on the EoM monthly grid; positive decimal.
- **variance_ddof**: ddof=1 for any standard deviations used in standardization/ancillary steps.


# variance

All standard deviations are sample (ddof=1).

# visualization_policy

## rolling_windows

- **notes**: When plotting multiple rolling series of the same family, use a shared Y-axis scale across the chart to enable visual comparability. This is a visualization recommendation; CSV values are unaffected.
- **unified_y_axis**: true

### families
#### percent_like
- *return*
- volatility_*
- *share*
- *drawdown*

#### ratio_like
- *sharpe*
- *sortino*
- *calmar*


# yearly_annualization_policy

Yearly risk metrics (vol/sharpe/sortino) are computed on the available months within the calendar year and annualized via sqrt(12). If months_in_year_available<12, set insufficient_months=True. Sortino remains undefined (NaN) if negative months < 2.

# yearly_metrics_policy


- **notes**: Computed strictly within calendar-year monthly returns; ddof=1; see methodology.nan_inf_policy. ×Risk variants (R-multiples) are not published for calendar-year metrics.

## applies_to
- compounding

## metrics
- volatility_annualized_year
- sharpe_ratio_annualized_year
- sortino_ratio_annualized_year
- calmar_ratio_year
- omega_ratio_year_target_0m
- monthly_var_95_year
- monthly_es_95_year
- eom_max_drawdown_intra_year
- intramonth_max_drawdown_intra_year


# parameters


- **drawdown_basis**: both

## confidence_intervals

- **seed**: 43

### bootstrap

- **type**: stationary_bootstrap
- **block_mean_length_months**: 6
- **n_boot**: 5000

### intramonth

- **block_mean_length_days**: 5
- **n_boot**: 5000

## monte_carlo
### block_mean_length_months

- **desc**: Expected block length in months (L). Accepts either a single integer (e.g., 6) or a list for sensitivity runs (e.g., [3,6,9]). Use 6 as institutional default; 3 as tighter alternative; 12 for stronger dependence.
- **type**: int|list

#### default
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12

### dd_thresholds_eom
- 0.05
- 0.07
- 0.1
- 0.2
- 0.3

### dd_thresholds_eom_mode

- **desc**: Controls which family of exceedance probabilities to compute and publish for EoM MaxDD. 'normalized' uses thresholds θ = k × runtime.risk_per_trade_pct (fields: prob_maxdd_ge_{5,7,10}x_risk_eom). 'absolute' uses fixed absolute thresholds θ in decimals (fields: prob_maxdd_ge_{5,7,10}pc_eom). 'both' computes and publishes both families.
- **type**: enum
- **default**: normalized

#### allowed_values
- normalized
- absolute
- both

### dd_thresholds_eom_normalized_multipliers
- 5
- 7
- 10

### horizons_months

- **desc**: List of horizons to simulate. 'full_period' is resolved to the dataset length in months.
- **type**: list

#### default
- 12
- 36
- full_period

### method

- **desc**: Bootstrap method for monthly resampling. Stationary (Politis–Romano) preserves dependence with random-length blocks.
- **type**: enum
- **default**: stationary_bootstrap

#### allowed_values
- stationary_bootstrap
- moving_block_bootstrap

### n_paths

- **desc**: Number of simulated paths.
- **type**: int
- **default**: 10000

#### allowed_range
- 1000
- 100000

### seed

- **desc**: Random seed for reproducibility (optional).
- **type**: int
- **default**: 42

## newey_west
### lag_months

- **desc**: Newey–West lag (months) for HAC-robust standard error of mean monthly return; Bartlett kernel.
- **type**: int
- **default**: 6

#### allowed_range
- 3
- 6

## risk_per_trade_pct

- **desc**: Per-trade risk budget as a fraction of equity; switch between 0.01 (1%) and 0.015 (1.5%).
- **type**: float
- **default**: 0.01
- **unit**: fraction_of_equity

### allowed_values
- 0.01
- 0.015

## starting_nav_ccy

- **desc**: Starting capital in account currency. Can be overridden at runtime/CLI (e.g., --start-nav).
- **type**: currency
- **default**: 100000

## trades_bootstrap
### block_mean_length_trades

- **type**: int
- **default**: 20

#### allowed_range
- 10
- 30

### horizons_trades

- **type**: list

#### default
- 100

### method

- **type**: enum
- **default**: stationary_bootstrap

#### allowed_values
- stationary_bootstrap

### n_paths

- **type**: int
- **default**: 10000

#### allowed_range
- 1000
- 100000

### seed

- **type**: int
- **default**: 44


# profiles

## risk_1_5pct

- **runtime.risk_per_trade_pct**: 0.015

## risk_1pct

- **runtime.risk_per_trade_pct**: 0.01


# rounding_policy

## currency_like

- **decimals**: 2
- **rounding_mode**: half_up

### applies_to
- ending_nav_full_period
- ending_nav_p05
- ending_nav_p50
- ending_nav_p95
- total_cash_in_ccy
- total_cash_out_ccy
- net_flows_ccy
- observed_last_eom_nav_ccy
- flow_aware_ending_value_ccy
- year_start_nav_ccy_raw
- year_end_nav_ccy_raw
- opening_cashflow_ccy
- mc_cap_nav_ccy
- mc_cap_tolerance_ccy
- policy_total_cash_out_ccy_p05
- policy_total_cash_out_ccy_p50
- policy_total_cash_out_ccy_p95
- policy_ending_nav_ccy_p05
- policy_ending_nav_ccy_p50
- policy_ending_nav_ccy_p95
- policy_flow_aware_ending_value_ccy_p05
- policy_flow_aware_ending_value_ccy_p50
- policy_flow_aware_ending_value_ccy_p95

## duration_like_minutes

- **decimals**: 0

### applies_to_patterns
- *holding_period*_minutes*

## percent_like

- **decimals**: 4

### applies_to_patterns
- *downside_deviation*
- *drawdown*
- *return*
- *cagr*
- *share*
- *ulcer*
- annual_return_*
- prob_*
- volatility_*
- win_rate
- *var_*
- *_es_*
- *pain_index*

## priority_order
- currency_like
- wealth_like
- sharpe_sortino_like
- ratio_like
- pvalue_like
- r_multiple_like_trades
- xrisk_like
- duration_like_minutes
- percent_like

## pvalue_like

- **decimals**: 3

### applies_to_patterns
- *p_value*

## r_multiple_like_trades

- **decimals**: 2

### applies_to_patterns
- worst_*_trade_run_r
- edr_*_trades_*_r

## ratio_like

- **decimals**: 2

### applies_to_patterns
- *profit_factor*
- *payoff*
- *martin*
- *skew*
- *kurt*
- *omega*
- *tail_ratio*
- *gain_to_pain*
- *pain_ratio*
- *tstat*
- flow_aware_wealth_multiple

## sharpe_sortino_like

- **decimals**: 2

### applies_to_patterns
- *sharpe*
- *sortino*
- *calmar*

## wealth_like

- **decimals**: 2

### applies_to_patterns
- wealth_multiple_*

## xrisk_like

- **decimals**: 2

### applies_to_patterns
- *xrisk*


# runtime


- **risk_per_trade_pct**: 0.01
- **starting_nav_ccy**: 100000

## confidence_intervals
### bootstrap

- **block_mean_length_months**: 6
- **n_boot**: 5000
- **seed**: 43

### intramonth

- **block_mean_length_days**: 5
- **n_boot**: 5000
- **seed**: 43

## metrics_options

- **tail_ratio_mode**: all

### tail_ratio_mode_allowed
- all
- active

## monte_carlo

- **dd_thresholds_eom_mode**: both
- **method**: stationary_bootstrap
- **n_paths**: 10000
- **seed**: 42

### block_mean_length_months
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10
- 11
- 12

### horizons_months
- 12
- 36
- full_period

## newey_west

- **lag_months**: 6

## policy

- **type**: cap_nav
- **apply**: yearly
- **cap_nav_ccy**: null
- **cap_tolerance_ccy**: 0
- **enabled**: false
- **start_month**: 1

## quantiles

- **hf_type**: 7
- **method**: linear

## trades_bootstrap

- **block_mean_length_trades**: 20
- **method**: stationary_bootstrap
- **n_paths**: 10000
- **seed**: 44

### horizons_trades
- 100


# schema_id

metrics_schema

# schema_title

Full metrics set

# units


- **currency**: USD
- **duration_minutes**: calendar minutes (UTC)
- **returns**: decimal (0.012 = 1.2%)
- **risk_per_trade_pct**: decimal share of capital (e.g., 0.01 for 1%)
- **timezone**: UTC (all timestamps, anchors, and buckets)
- **x_risk**: R-multiple (value divided by risk_per_trade_pct; dimensionless)

## strict_R_classification

- **note**: Zero-R trades are those with |R| ≤ eps; zeros are excluded from PF denominator and from win_rate numerator/denominator; included as-is for expectancy/std/min/max.
- **eps**: 1e-12
- **loss**: R < -eps
- **win**: R > +eps
- **zero**: |R| ≤ eps


# version

metrics_schema-1.0