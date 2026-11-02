# Reproduction cross-check against the manuscript

This file records a full run of `run_pipeline.py` using the supplied city CSVs and `study_config.yaml`. The exact environment, input SHA-256 hashes, and per-city split counts are in `results/run_manifest.json`. Values below are computed from the committed CSV outputs; values reported in the manuscript are rounded, so comparisons use that precision.

## Results matching the manuscript to its reported precision

- **Table 3 macro RMSE:** Ridge-AR is 8.5448 (PM2.5) and 10.2136 (PM10); persistence is 11.3388 and 13.2549; Ridge-All is 11.5328 and 13.8069 μg m⁻³. These agree with the manuscript values to two decimals.
- **Table 4 city SARIMA RMSE:** the pipeline output agrees with the manuscript city values to two decimals (e.g., Quetta PM2.5 3.81 and PM10 18.53; Lahore PM2.5 11.35 and PM10 11.44 μg m⁻³).
- **Nine-location sensitivity means and SDs:** recomputed macro RMSE values after excluding Rawalpindi agree with the manuscript Table 5a to two decimals. Islamabad remains included.
- **Table 6 city-specific best models:** interpret this table as ranking the non-SARIMA benchmark candidates; SARIMA has a separate city-level table. Under that model set, the reported city/model selections are reproduced.

## Discrepancies requiring author reconciliation

### Paired city-bootstrap interval endpoints

The point differences match the manuscript after rounding, but percentile interval endpoints from the archived code run differ slightly from those printed in the manuscript. This run uses 10,000 location-level paired resamples, seed `20260731`, and excludes Rawalpindi while retaining Islamabad.

| Contrast | Pollutant | Mean ΔRMSE | Recomputed 95% interval | Manuscript interval |
|---|---|---:|---:|---:|
| Ridge-AR − Persistence | PM2.5 | −2.6755 | −4.0797 to −1.3551 | −4.081 to −1.330 |
| Ridge-AR − Persistence | PM10 | −2.9603 | −4.2549 to −1.7318 | −4.261 to −1.771 |
| Ridge-All − Persistence | PM2.5 | 0.6310 | −1.6293 to 2.9210 | −1.629 to 2.921 |
| Ridge-All − Persistence | PM10 | 1.0047 | −1.3138 to 3.3921 | −1.295 to 3.448 |

The first two intervals remain entirely below zero; both Ridge-All intervals include zero. Do not describe the endpoints as exact reproductions until the manuscript’s original resampling implementation or saved bootstrap draws are compared with this package.

### DM-style test for macro mean squared-error differences

`results/dm_style_results.csv` contains a 24-lag HAC DM-style test on the time series formed by averaging each model’s squared errors across the ten cities at each common test origin. For this specified aggregation, the macro p-values are 0.1367 for PM2.5 and 0.0032 for PM10. Thus, the PM2.5 macro comparison is not statistically significant at 0.05, whereas PM10 is significant. If the manuscript states that the macro squared-error comparison is significant for both pollutants, that sentence/table must be checked against the intended DM aggregation and corrected or its alternative calculation documented. City-level rows are also provided and should not be substituted for this macro result without labeling the change in inferential scope.

## Scope and fitting caveats

- Each city has one chronological terminal test block; the 414 scored forecasts are sequential one-step predictions within that block, not 414 independent test replicates and not forecasts from multiple origins or winters.
- Targets are CAMS-derived model fields, not ground-station measurements. Results therefore concern predictability of the archived fields.
- Ridge uses five validation-selected alpha values. Tree and MLP configurations are fixed; comparisons do not establish architecture-wide superiority.
- MLP uses the configuration recorded in `run_manifest.json` and may emit convergence warnings at its configured iteration cap.

