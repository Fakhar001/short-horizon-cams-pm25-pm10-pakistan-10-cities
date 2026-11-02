# Short-Horizon Forecasting of CAMS-Derived PM2.5 and PM10 across Ten Pakistani Cities

Reproducibility package for **“Short-Horizon Forecasting of CAMS-Derived PM2.5 and PM10 across Ten Pakistani Cities: A Leakage-Free Benchmarking and Explainability Study.”**

## Complete paper package

Download [short-horizon-cams-pm25-pm10-pakistan-10-cities.zip](https://github.com/Fakhar001/short-horizon-cams-pm25-pm10-pakistan-10-cities/raw/refs/heads/main/short-horizon-cams-pm25-pm10-pakistan-10-cities.zip). The archive contains the runnable source code, all ten city CSV files plus the combined dataset, configuration, dependencies, full-run metrics and predictions, diagnostics, SHAP outputs, figures, and the run manifest. The repository also includes the manuscript crosswalk and numerical reproduction audit.

## Reproduce

Extract the archive, then run from its root with Python 3.10 or later:

```bash
python -m pip install -r requirements.txt
python run_pipeline.py --output-dir results_reproduced
```

The default configuration reproduces the full analysis with rolling one-step SARIMA and the MLP enabled. Some MLP fits may emit a scikit-learn convergence warning at the configured iteration limit; this is recorded in the package documentation.

## Data and scope

The targets are CAMS-derived gridded estimates retrieved through the Open-Meteo Air Quality API, not ground-station observations. The period is 6 November 2025 through 4 February 2026 in UTC. Each city uses one chronological 70/10/20 train/validation/test split and one terminal test block. The archived API request and response logs are not included.

## Reproduction audit

Review [REPRODUCTION_AUDIT.md](REPRODUCTION_AUDIT.md) before citing exact figures. Headline macro metrics and city SARIMA metrics match the manuscript to reported rounding. The audit records small differences in bootstrap interval endpoints and a discrepancy in the macro DM-style PM2.5 inference that should be reconciled in the manuscript.

## Limitations

Results concern predictability of the archived model-derived fields for this one winter and the specified city coordinates. They do not establish station-level accuracy, personal exposure validity, future-winter performance, or warning calibration. Ridge models used validation selection over five alpha values; the tree and MLP configurations were fixed, so comparisons apply to the tested configurations, not whole model families. City bootstrap intervals quantify between-location sensitivity, not temporal uncertainty.

Please cite the accompanying manuscript using its final publication details when available. No software or data reuse license is asserted here; contact the authors for reuse terms.
