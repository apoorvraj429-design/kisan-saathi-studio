# Kisan Saathi: ready for GitHub and Streamlit

This folder contains the complete working app and freshly trained models. No coding or retraining is required to launch the prediction pages.

## Upload these files

1. Download this repository with **Code → Download ZIP**, then use **Extract All** on your computer.
2. Create or open your new GitHub repository, such as `kisan-saathi-studio`.
3. Upload the extracted contents together. Keep `app.py` directly at the repository root, with `assets/` and `.streamlit/` preserved as folders.
4. Commit the files. Do not upload raw loan datasets or private credentials.
5. In Streamlit Community Cloud, select this repository, branch `main`, entry file `app.py`, and Python **3.12**. Deploy.

If your file picker hides `.streamlit`, create `.streamlit/config.toml` through GitHub's Create new file option and copy its supplied contents. Full beginner instructions, local commands, troubleshooting, and rollback are in `START_HERE.md`.

## Exactly three sections

- Overview
- Income estimator
- Batch predictions

## File guide

| File or folder | Purpose |
| --- | --- |
| `app.py` | Complete interface and three-section navigation |
| `core.py` | Trained-model loading, prediction and input definitions |
| `batch_tools.py` | CSV reading, row validation and upload limits |
| `requirements.txt` | Exact compatible Python packages |
| `model_bundle.joblib` | Freshly fitted linear and logistic regression models |
| `metrics.json` | Matching model checksum, feature schema and actual evaluation results |
| `dashboard_data.json` | Aggregate charts and hypothetical example profiles |
| `assets/` | Local farmer photographs, styles and image licences |
| `.streamlit/config.toml` | Colours and upload configuration |
| `input_template.csv` | Blank batch-input headers |
| `sample_batch.csv` | Three hypothetical example profiles |
| `MODEL_REPORT.md` | Measured model performance and limitations |
| `VERIFICATION.md` | Checks performed on the complete project |
| `START_HERE.md` | Beginner setup guide |
| `.gitignore` | Prevents accidental commits of secrets and private data |

The design includes a farmer-photo hero, a second agricultural photograph, green and cream colours, responsive forms, prominent income results, contribution charts, and batch income-distribution charts.

No API key or paid external service is required to use this app.

## Model meaning

The linear model estimates total income in INR, including known non-farm income. The source does not establish an income period or land-area unit. The logistic model estimates the probability of income below the fit-set median, ₹942,000. That benchmark is a dataset comparison, not a poverty or eligibility threshold. See MODEL_REPORT.md for results and limitations.

This is a compact deployment package. The separate full project ZIP contains `train.py`, `Train_Your_Models.ipynb`, tests and additional data notes. References to retraining or running those tests in the longer guide apply to that full project package. No raw training data or real credentials are included here.
