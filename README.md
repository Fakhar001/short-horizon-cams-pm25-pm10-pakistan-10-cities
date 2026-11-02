# Short-Horizon Forecasting of CAMS-Derived PM2.5 and PM10 across Ten Pakistani Cities

Reproducibility repository for **“Short-Horizon Forecasting of CAMS-Derived PM2.5 and PM10 across Ten Pakistani Cities: A Leakage-Free Benchmarking and Explainability Study.”** It contains the analysis code, the archived input data used by the paper, the complete outputs from a full pipeline run, and a mapping from manuscript items to code and files.

## Contents

- `data/` — ten city-level hourly CSV files and `analysis_manifest.json` (21,840 city-hour records in total).
- `pm25pm10_q1/` — forecasting, preprocessing, metrics, inference, plotting, and attribution implementation.
- `run_pipeline.py`, `study_config.yaml`, `requirements.txt` — pipeline entry point, manuscript configuration, and Python dependencies.
- `results/` — full-run metrics, per-origin predictions, diagnostics, bootstrap and DM-style results, SHAP outputs, figures, and run manifest.
- `MANUSCRIPT_MAPPING.md` — manuscript sections, tables, and figures mapped to implementation and generated outputs.
- `REPRODUCTION_AUDIT.md` — numerical cross-check of the archived full-run outputs against manuscript-reported results, including discrepancies that need author review.

## Data and study scope

The targets are CAMS-derived gridded estimates retrieved through the Open-Meteo Air Quality API; they are not ground-station observations. Meteorological inputs are from the Open-Meteo Historical Weather API. The supplied CSV files are the archived inputs used in this analysis. The original API request and response logs are not included, so the repository reproduces the analysis from those archived files, not the upstream retrieval transaction.

The period is 6 November 2025 through 4 February 2026, in UTC. Models use a 24-hour information window and chronological 70/10/20% train/validation/test blocks. Windows are constructed within each block, giving 1,504 training, 194 validation, and 414 test windows for each city. There is one terminal test block per city, not a multi-season or multi-origin evaluation.

## Reproduce the full analysis

Requires Python 3.10 or later. From the repository root:

```bash
python -m pip install -r requirements.txt
python run_pipeline.py --output-dir results_reproduced
```

The pipeline uses the default `study_config.yaml`, including rolling one-step SARIMA with fixed training-estimated parameters and sequential state updates. See `python run_pipeline.py --help` for supported overrides. The committed `results/` directory is the output of a complete run with both SARIMA and the MLP enabled. Some MLP fits emit a scikit-learn convergence warning at the configured `max_iter=120`; outputs are retained as generated and this warning is documented rather than suppressed.

## Outputs

The `results/` directory includes:

- city and macro model metrics, the full long-form predictions, and selected Ridge hyperparameters;
- SARIMA diagnostics, data-quality summary, peak-regime metrics, paired city-bootstrap results, and DM-style HAC results;
- grouped and lag-level SHAP attribution tables;
- manuscript figures (Figures 1, 3, and 5) and target-specific RMSE and SHAP figures;
- `run_manifest.json`, with the exact input hashes, software versions, configuration, and split counts for this run.

See `REPRODUCTION_AUDIT.md` before citing exact numbers: headline macro metrics and SARIMA city metrics match the manuscript to reported rounding, while some bootstrap interval endpoints and one macro DM-style inference require reconciliation.

## Interpretation limits

The outputs quantify prediction performance for the archived CAMS-derived fields over one winter period and the specified coordinates. They do not establish station-level accuracy, personal exposure validity, performance in other seasons, or warning calibration. Ridge models were selected over a five-value validation grid; Random Forest, Gradient Boosting, and MLP used fixed configurations, so rankings apply to these evaluated configurations rather than model families generally. Bootstrap intervals resample cities and quantify across-location sensitivity, not temporal uncertainty.

## Citation

Please cite the accompanying manuscript using its final publication details when available. No software/data reuse license is asserted in this repository; contact the authors for reuse terms.

