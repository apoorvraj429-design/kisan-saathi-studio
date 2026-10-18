# Release verification

Checked on 18 October 2026 using Python 3.12.14 and the exact packages in requirements.txt. Results below describe checks actually performed on this new project.

## Models and data

- Read all three supplied CSVs and their dictionary. No synthetic training data was substituted.
- Fitted new ordinary-least-squares linear and logistic regression pipelines from the labelled source. Earlier trained artifacts were not reused.
- Retained 47,970 labelled records; repeated loan IDs stay in the same fit, calibration or holdout partition.
- Evaluated against the internal 9,592-row labelled holdout. All 9,986 rows in the separate unlabelled file were scored without claiming accuracy for that file.
- Compared against fit-mean, fit-median, known-nonfarm-only, known-nonfarm-plus-typical-farm, and no-nonfarm-model baselines. Complete measured results are in MODEL_REPORT.md and metrics.json.
- Tests check that changing holdout outcomes cannot alter fitted model coefficients, preprocessing, benchmark or the calibration band. Source, script, split and artifact fingerprints support reproducibility.
- A model/metadata checksum mismatch is rejected before deserializing the model. Keep the files from the same run together.

## Automated checks

`python -m unittest discover -s tests -v` passed **66 tests**:

- 11 training-protocol checks.
- 18 prediction and serialization checks.
- 11 CSV size, row-limit, header, validation and identifier-protection checks.
- 18 chatbot transport and failure-handling checks using mocked responses.
- 7 Streamlit integration checks for navigation, actual model results, chat history, opt-in context, interruptions and clearing.
- 1 artifact-pair mismatch check.

These tests do not call a paid AI service. Mocked success replies exercise the code path without proving an external account works.

## Browser and notebook checks

- Ran the actual app in Firefox at desktop 1440×1000 and mobile 390×844 sizes.
- Inspected all four sections, local photographs, responsive forms, a real example prediction, contribution chart and prediction download.
- Uploaded the complete 13 MB test CSV, scored all 9,986 rows, and verified the downloaded row count and exclusion of loan IDs.
- Checked invalid batch inputs and missing credentials without breaking prediction pages.
- Checked that mobile pages fit without page-level horizontal overflow. Wide data tables can scroll within their container.
- Fixed a chart rendering problem found during visual inspection, using aggregate income bands for batch charts.
- Executed the notebook's setup, data-selection, retraining, report/chart and clean-export cells in a separate project copy. Real metrics reproduced; the three chart specifications validated.
- Checked notebook ZIP extraction against unsafe paths/symlinks and export against accidental raw-data/credential inclusion. Shared notebook code cells have no saved private outputs.

## Not verified

- A real authenticated OpenAI reply, model access or billing: no API key was provided.
- Live weather, mandi prices, government scheme feeds or web search: none are connected.
- The Colab browser upload interface and notebook package-installation cell: notebook code was tested locally with the pinned packages already installed.
- A new GitHub-hosted deployment or Streamlit Cloud installation: the prepared package still needs a new repository and deployment. Follow START_HERE.md and test the resulting public or private app address.
- Future-season accuracy, unseen-village performance, causal effects or population representativeness.

The previous GitHub repository, its draft redesign pull request, and the previous live app were left unchanged during this new project.
