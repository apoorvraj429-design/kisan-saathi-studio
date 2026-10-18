# Kisan Saathi Studio: fresh model report

Fresh OLS linear regression and logistic regression were fitted from the supplied labeled CSV. Earlier trained artifacts were not reused; the previous project was left unchanged. The application code adapts the earlier project's interface. This is a historical data prototype, not a current-season forecast.

## Data and separation

- Labeled source: 47,970 rows, 105 columns, 47,948 distinct loan agreement IDs.
- All labeled rows retained. 22 repeated-ID excess rows, including 2 exact duplicate rows, stay within a single partition. No duplicate input profile is removed.
- FarmerID identifies loan agreements, not verified unique people. Identifiers, demographic fields, addresses, geography below state, and loan/credit variables are excluded from model predictors.
- The source has 71 missing land values. Numeric imputation, scaling, common-state selection and the income benchmark use the fit partition only.
- Deterministic GroupShuffleSplit on loan agreement ID, seed 42 (second split seed 43); no shared loan IDs across partitions.

| Partition | Records | Loan agreement groups |
|---|---:|---:|
| train | 28,781 | 28,768 |
| validation | 9,597 | 9,590 |
| holdout | 9,592 | 9,590 |

The validation partition is reserved for error-band calibration, not model selection. Logistic C=1 and threshold 0.5 were fixed in advance. Neither model is refitted after calibration or evaluation. The internal holdout is distinct from the separate unlabeled scoring CSV.

## Models and held-out results

The eight predictors are state, land area, known non-farm income, distance to mandi, Kharif rainfall, Rabi rainfall, cropping-density index and agricultural-score index. Numeric predictors use fit-set median imputation and standardisation. State uses one-hot encoding; states with fewer than 30 fit rows share OTHER. The linear model is ordinary least squares. Logistic regression estimates whether income is below the fit-set median of ₹942,000, a dataset comparison rather than a poverty or eligibility threshold.

| Internal holdout measure | Value |
|---|---:|
| Linear MAE | ₹363,566.97 |
| Linear RMSE | ₹762,907.82 |
| Linear R² | 0.898689 |
| Logistic accuracy | 71.89% |
| Logistic ROC-AUC | 0.794080 |
| Logistic Brier score | 0.185516 |

## The known-income component matters

Total income already includes non-farm income. Providing that component is essential; an unknown value must not be replaced with zero. High overall R² must not be interpreted as accurate prediction from agricultural information alone.

| Internal holdout comparison | MAE | R² |
|---|---:|---:|
| Full OLS model | ₹363,566.97 | 0.898689 |
| Known non-farm income alone | ₹968,358.04 | 0.699200 |
| Known non-farm income + typical fit-set farm component | ₹415,289.80 | 0.857490 |
| Fit-set mean for every record | ₹614,945.81 | -0.000555 |
| Fit-set median for every record | ₹554,368.52 | -0.019017 |
| OLS without known non-farm income | ₹571,207.85 | 0.012284 |

The stronger component baseline adds the fit-set median of (total income − known non-farm income), ₹800,000, to each known non-farm value. It is learned without calibration or holdout outcomes. OLS minimizes squared errors, so a simpler median-based baseline can have lower mean absolute error even when OLS has higher R². The app retains OLS as the requested model and does not select a model by inspecting holdout results.

## Error-band calibration

The band radius is ₹690,914.56. First calculate absolute errors of displayed non-negative estimates in the calibration partition. Take the maximum error per loan agreement group; use sorted order statistic ceil((n+1)×0.90), capped at n, across 9,590 calibration groups (rank 8,632). Holdout outcomes do not set the radius. Report the estimate ± radius with the lower bound clipped at zero.

Observed internal-holdout row coverage was 89.45%; complete-loan-group coverage was 89.45%. These are empirical results, not a guaranteed interval for an individual. Loan groups can share villages, so independent or exchangeable sampling is not established.

## Limits and interpretation

- Internal holdout groups loan agreements, not villages. Unseen-village generalization is not tested.
- Known non-farm income is already part of total income; strong overall fit partly reflects this shared component.
- Income reporting period and farmer land-area unit are not specified. Environmental source inputs refer to 2022.
- No current-season validation, causal claims, calibrated poverty category or credit decision use is established.
- Loan groups can share villages; empirical coverage is not an individual guarantee or a proof of independent sampling.
- All labeled rows are retained, including repeated loan IDs; repetition is contained within a partition.
- 85.26% of holdout rows have a state/district/village group represented in the fit partition. This evaluation does not establish unseen-village performance.
- Predictions are floored at zero without changing the OLS model. A total below the known non-farm component is flagged, not silently forced upward.
- Strong income outliers remain in training and evaluation; extreme profiles can have much larger error than the overall summaries.

## Separate unlabeled scoring CSV

Scored 9,986 rows after fitting and evaluation. It has zero observed targets: no accuracy, R², MAE or AUC is claimed for this file. Predictions are saved locally in ignored `outputs/lte_test_predictions.csv` with source-row numbers and no loan IDs. No scoring-file feature influenced fitting, preprocessing, benchmark, calibration or model choice.

## Reproduce and files

Run `python train.py --train /path/lte_train.csv --test /path/lte_test.csv --output-dir fresh_run`. Omit `--test` to train only. The script refuses to replace generated files unless `--overwrite` is explicitly supplied. Source-file SHA-256 hashes, dependency versions, seed, split assignment hash and complete metrics are in `metrics.json`. The fit/calibration/holdout IDs and source CSVs are not included in deployment files.
