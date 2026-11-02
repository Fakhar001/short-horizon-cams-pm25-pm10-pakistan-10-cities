# Manuscript-to-package crosswalk

| Manuscript item | Implementation | Generated output |
|---|---|---|
| Section 2.1 forecasting formulation and within-block windows | `pm25pm10_q1/data.py::prepare_city` | `run_manifest.json` block counts; `predictions_long.csv` origins |
| Section 2.2 training-only scaling and circular wind encoding | `pm25pm10_q1/data.py` | Input preparation recorded in configuration and run manifest |
| Section 2.3 persistence and seasonal-naive references | `pm25pm10_q1/models.py::fit_benchmarks` | `city_model_metrics.csv`, `predictions_long.csv` |
| Sections 2.4 and 2.6 Ridge, tree, MLP models and validation selection | `pm25pm10_q1/models.py::fit_benchmarks` | `ridge_hyperparameters.csv`, metrics and predictions |
| Section 2.5 seasonal SARIMA with sequential one-step state updates | `pm25pm10_q1/classical.py::sarima_forecast` | `sarima_diagnostics.csv`, SARIMA rows in metrics and predictions |
| Section 2.7 grouped interventional linear attribution | `pm25pm10_q1/shap_linear.py::exact_linear_shap` | `shap_grouped_summary.csv`, `shap_group_values.csv`, `shap_lag_summary.csv` |
| Section 2.8 test metrics and P95 threshold-exceedance evaluation | `pm25pm10_q1/metrics.py` | City metrics and `peak_regime_metrics.csv` |
| Section 2.9 paired city bootstrap and DM-style HAC comparison | `pm25pm10_q1/inference.py` | `paired_bootstrap_results.csv`, `dm_style_results.csv` |
| Sections 3.1–3.3 data, quality audit, split and windows | `data/`, `pm25pm10_q1/data.py`, `pm25pm10_q1/pipeline.py` | `data_quality_summary.csv`, `run_manifest.json` |
| Table 1 city means and descriptive P95 values | Archived input CSV files and `_data_quality` | `data_quality_summary.csv`; `figure1_city_means.png` |
| Table 3 macro-averaged model performance | `pm25pm10_q1/metrics.py`, `pipeline.py::_macro_metrics` | `macro_model_metrics.csv` |
| Table 4 city-level SARIMA performance | `pm25pm10_q1/classical.py`, `pm25pm10_q1/metrics.py` | City rows for SARIMA in `city_model_metrics.csv` |
| Tables 5a–5b macro RMSE and nine-location paired bootstrap | `pm25pm10_q1/inference.py::paired_city_bootstrap` | Macro metrics and `paired_bootstrap_results.csv` |
| Table 6 lowest test-RMSE configuration by city and pollutant | Derived from `city_model_metrics.csv`; test-selected ranking is descriptive and retrospective | `city_model_metrics.csv` |
| Table 7 grouped SHAP magnitudes and shares | `pm25pm10_q1/shap_linear.py::exact_linear_shap` | `shap_grouped_summary.csv`; `figure5_shap_attributions.png` |
| Figure 3 macro RMSE with between-city SD whiskers | `pm25pm10_q1/pipeline.py::_write_figures` | `figure3_macro_rmse.png` |
| Peak-regime results by city and pollutant | `pm25pm10_q1/metrics.py::peak_regime_rows` | `peak_regime_metrics.csv` |

All reported error metrics are in the original concentration units. The city bootstrap resamples locations and should not be interpreted as temporal or across-winter uncertainty. Peak-regime results are conditional on training-period P95 exceedance and do not measure event detection or peak timing.

