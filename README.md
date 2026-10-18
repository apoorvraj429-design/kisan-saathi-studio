# Kisan Saathi: ready for GitHub and Streamlit

This folder contains the complete working app and freshly trained models. No coding or retraining is required to launch the prediction pages.

## Upload these files

1. Extract `Kisan_Saathi_GitHub_Upload.zip` on your computer using **Extract All**.
2. Create or open your new GitHub repository, such as `kisan-saathi-studio`.
3. Upload the extracted contents together. Keep `app.py` directly at the repository root, with `assets/` and `.streamlit/` preserved as folders.
4. Commit the files. Do not upload your private API key or raw loan datasets.
5. In Streamlit Community Cloud, select this repository, branch `main`, entry file `app.py`, and Python **3.12**. Deploy.

If your file picker hides `.streamlit`, create `.streamlit/config.toml` through GitHub's Create new file option and copy its supplied contents. The example secrets file contains placeholders only. Full beginner instructions, local commands, troubleshooting, and rollback are in `START_HERE.md`.

## Exactly four sections

- Overview
- Income estimator
- Batch predictions
- AI assistant

## File guide

| File or folder | Purpose |
| --- | --- |
| `app.py` | Complete interface and four-section navigation |
| `core.py` | Trained-model loading, prediction and input definitions |
| `batch_tools.py` | CSV reading, row validation and upload limits |
| `assistant.py` | Conversational AI connection and streaming |
| `requirements.txt` | Exact compatible Python packages |
| `model_bundle.joblib` | Freshly fitted linear and logistic regression models |
| `metrics.json` | Matching model checksum, feature schema and actual evaluation results |
| `dashboard_data.json` | Aggregate charts and hypothetical example profiles |
| `assets/` | Local farmer photographs, styles and image licences |
| `.streamlit/config.toml` | Colours and upload configuration |
| `.streamlit/secrets.toml.example` | Placeholder-only AI settings |
| `input_template.csv` | Blank batch-input headers |
| `sample_batch.csv` | Three hypothetical example profiles |
| `MODEL_REPORT.md` | Measured model performance and limitations |
| `VERIFICATION.md` | Checks performed on the complete project |
| `START_HERE.md` | Beginner setup guide |
| `.gitignore` | Prevents accidental commits of secrets and private data |

The design includes a farmer-photo hero, a second agricultural photograph, green and cream colours, responsive forms, prominent income results, contribution charts, and batch income-distribution charts.

## Connect the AI assistant

In your deployed app's **Settings > Secrets**, add your private configuration:

```toml
OPENAI_API_KEY = "replace-with-your-own-key"
OPENAI_MODEL = "gpt-5.4-mini"
```

API usage may incur charges. The prediction pages work without a key. The chatbot has no connected live weather, mandi-price or web-search feed. It shares one estimate only when the user explicitly opts in; batch files are not automatically sent to the AI provider.

## Model meaning

The linear model estimates total income in INR, including known non-farm income. The source does not establish an income period or land-area unit. The logistic model estimates the probability of income below the fit-set median, ₹942,000. That benchmark is a dataset comparison, not a poverty or eligibility threshold. See MODEL_REPORT.md for results and limitations.

This is a compact deployment package. The separate full project ZIP contains `train.py`, `Train_Your_Models.ipynb`, tests and additional data notes. References to retraining or running those tests in the longer guide apply to that full project package. No raw training data or real credentials are included here.
