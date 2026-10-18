# Release verification

Original project checked on 18 October 2026 using Python 3.12.14 and the exact packages in requirements.txt. The retained model and prediction checks below describe that original validation. The current interface contains three sections: Overview, Income estimator, and Batch predictions.

## Current interface update

The three-section update keeps the prepared prediction models and data-processing modules unchanged. Six focused checks passed before publication:

- All three pages render without the removed connection module.
- The individual example displays the actual saved-model estimate of ₹742,307.
- All three example profiles produce the same complete outputs as the saved baseline.
- A valid three-row batch upload renders results and generates the prediction-download control.
- A malformed batch upload is rejected with a validation message.
- An existing session on a removed page safely returns to Overview.

The focused checks use Streamlit AppTest. They do not repeat the original desktop/mobile browser or full-data evaluation below.

## Models and data

- Read all three supplied CSVs and their dictionary. No synthetic training data was substituted.
- Fitted new ordinary-least-squares linear and logistic regression pipelines from the labelled source. Earlier trained artifacts were not reused.
- Retained 47,970 labelled records; repeated loan IDs stay in the same fit, calibration or holdout partition.
- Evaluated against the internal 9,592-row labelled holdout. All 9,986 rows in the separate unlabelled file were scored without claiming accuracy for that file.
- Compared against fit-mean, fit-median, known-nonfarm-only, known-nonfarm-plus-typical-farm, and no-nonfarm-model baselines. Complete measured results are in MODEL_REPORT.md and metrics.json.
- Tests check that changing holdout outcomes cannot alter fitted model coefficients, preprocessing, benchmark or the calibration band. Source, script, split and artifact fingerprints support reproducibility.
- A model/metadata checksum mismatch is rejected before deserializing the model. Keep the files from the same run together.

## Automated checks

The original full-project automated suite passed. Its retained prediction-related checks included:

- 11 training-protocol checks.
- 18 prediction and serialization checks.
- 11 CSV size, row-limit, header, validation and identifier-protection checks.
- 1 artifact-pair mismatch check.
- Streamlit integration checks covering navigation and actual model results.

These counts describe the original validation, not a rerun after the interface update. The compact deployment package does not include the full training package's test directory.

## Browser and notebook checks

- Ran the actual app in Firefox at desktop 1440×1000 and mobile 390×844 sizes.
- Inspected the prediction pages, local photographs, responsive forms, a real example prediction, contribution chart and prediction download in the original interface.
- Uploaded the complete 13 MB test CSV, scored all 9,986 rows, and verified the downloaded row count and exclusion of loan IDs.
- Checked invalid batch inputs and confirmed useful validation messages.
- Checked that mobile pages fit without page-level horizontal overflow. Wide data tables can scroll within their container.
- Fixed a chart rendering problem found during visual inspection, using aggregate income bands for batch charts.
- Executed the notebook's setup, data-selection, retraining, report/chart and clean-export cells in a separate project copy. Real metrics reproduced; the three chart specifications validated.
- Checked notebook ZIP extraction against unsafe paths/symlinks and export against accidental raw-data/credential inclusion. Shared notebook code cells have no saved private outputs.

## Not verified

- The Colab browser upload interface and notebook package-installation cell: notebook code was tested locally with the pinned packages already installed.
- Future-season accuracy, unseen-village performance, causal effects or population representativeness.

The earlier six-section project and its draft redesign pull request are separate from this repository.
