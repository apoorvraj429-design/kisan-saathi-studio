# Put Kisan Saathi online, from zero

The easiest route is **prepared project files → new GitHub repository → Streamlit Community Cloud**. The supplied package already contains trained linear and logistic models. You can deploy first and study or repeat training afterwards. Your earlier app can stay available at its existing address.

## 1. Know what each service does

- **GitHub** stores your project files and their history. A project folder there is called a **repository**, or repo.
- **Streamlit** turns Python code into the website. **Streamlit Community Cloud** runs it online; this is called **deployment**.
- **OpenAI's API** optionally supplies chatbot replies. An **API key** is a private password that your server uses to make these requests.
- A **dependency** is a Python package the app needs. `requirements.txt` lists exact versions.
- A **commit** saves a set of file changes. A **branch** lets you work separately before updating the main version.
- A **terminal** is a command window, such as Windows PowerShell. You need it only for local running or optional local retraining.

You need GitHub and Streamlit accounts for the online route. You need an OpenAI API account only for real chatbot replies. The prediction sections work without it.

## 2. Extract the supplied project

Download and extract the complete deployment ZIP. Open the folder that directly contains `app.py`: this is the **project root**. Keep a clean copy of the ZIP as your starting backup.

The package contains app code, prepared model files, metadata, photographs, documentation, and a prediction template. Raw training CSVs and private API keys are separate. Keep raw CSVs on your computer or private training workspace; do not add them to GitHub for deployment.

A GitHub repository should contain the extracted project contents, rather than the ZIP or another outer folder. In the repository, `app.py` must be visible at the top level.

## 3. Create your new GitHub repository

1. Sign in to [GitHub](https://github.com).
2. Open [the new repository form](https://github.com/new), or select **+ → New repository**.
3. Select your account as owner, and name the repository **kisan-saathi-studio**. If unavailable, choose another name and use it consistently in Streamlit.
4. Enter a short description, such as “Farmer income assignment using linear and logistic regression, with a Streamlit app.”
5. Choose **Private** initially. You can choose **Public** if you want everyone to see the deployment source and have confirmed it contains no confidential data or credentials.
6. Leave the optional README, `.gitignore`, and licence initialization off because the package already contains project files. Click **Create repository**.

This opens a new project location; it does not deploy the app yet. Repository creation was not completed automatically because the available GitHub connection could not create a repository. Use the form above to complete this account step yourself. If you have already created it, open that repository and continue.

## 4. Upload the project files

On the new repository page, use **uploading an existing file** in Quick Setup. For a repository that already lists files, use **Add file → Upload files**. Drag the extracted project's **contents**, preserving `assets/` and `.streamlit/` as folders.

Check the upload before committing:

- Include `app.py`, `core.py`, `batch_tools.py`, `assistant.py`, `train.py`, `requirements.txt`, prepared model files, metadata, the template, documentation, assets, and configuration.
- Exclude raw training CSVs, real `.streamlit/secrets.toml`, `.venv`, `__pycache__`, and `.git`.
- Preserve `.streamlit/config.toml` and `.streamlit/secrets.toml.example`. If your file picker hides the folder, use **Add file → Create new file**, enter each full path, and paste the corresponding supplied text.
- Upload the individual extracted files, not the deployment ZIP. GitHub's browser upload has a 25 MiB limit per file and a 100-file limit per upload; use another batch if necessary.

Enter a commit message such as **Add Kisan Saathi app and trained models** and save the upload. Confirm the default branch is `main`, and check that `app.py` is directly at the repository root. Your new URL will follow `https://github.com/YOUR_USERNAME/kisan-saathi-studio`.

## 5. Deploy the app to Streamlit

1. Sign in to [Streamlit Community Cloud](https://share.streamlit.io) and connect the GitHub account containing the new repository. Grant it the required access if the repository is private.
2. Click **Create app** and choose the path for an app whose code already exists.
3. Set the repository to `YOUR_USERNAME/kisan-saathi-studio`, branch to `main`, and main file path to `app.py`.
4. In **Advanced settings**, select **Python 3.12**. Optionally choose an available app subdomain.
5. You can leave AI secrets empty for the first launch. Save settings and deploy.

Wait for the app to finish installing. Open the resulting `streamlit.app` address. The income and batch pages should work before AI is configured. Save that new address; it is distinct from the old app's URL.

Future commits to the branch Streamlit monitors update the deployed app. Changes to `requirements.txt` can take longer because packages are reinstalled. Keep the supplied pinned versions when using the prepared model bundle.

## 6. Enable real chatbot replies

1. Sign in to [OpenAI's API platform](https://platform.openai.com/). Select or create an API project.
2. Review [API pricing](https://openai.com/api/pricing/) and configure API billing in that account. ChatGPT subscription payments do not themselves pay for API calls.
3. Create a private key on the [API keys page](https://platform.openai.com/api-keys).
4. Return to Streamlit Community Cloud. Next to your app, use **three-dot menu → Settings → Secrets**. Paste:

```toml
OPENAI_API_KEY = "replace-with-your-own-key"
OPENAI_MODEL = "gpt-5.4-mini"
```

Replace only the key placeholder with your own key, keep the quotation marks, and click **Save**. Open **AI assistant** and send a short question. Keys belong in Streamlit Secrets, not in GitHub or the public app.

A private key should be treated like a password. If exposed, revoke it in the API platform and create a replacement. The supplied example configuration contains placeholders only.

### Cost and cheaper option

Official model rates checked on 18 October 2026, in USD per million text tokens:

| Model setting | Input | Output | Use |
| --- | ---: | ---: | --- |
| `gpt-5.4-mini` | $0.75 | $4.50 | Supplied default |
| `gpt-6-luna` | $0.10 | $0.50 | Lower-cost alternative |

A token is a small piece of text. For illustration, a request with 2,000 input and 500 output tokens costs about $0.00375 using the default, or $0.00045 with the cheaper option. Actual charges depend on usage and billing terms; recent chat and instructions count as input. To try the cheaper option, set `OPENAI_MODEL = "gpt-6-luna"` and test its answers. The supplied answer cap is 1,600 tokens. Browser-session limits do not provide an account-wide spending cap: review usage and account controls before sharing widely.

Sources: [GPT-5.4 Mini](https://developers.openai.com/api/docs/models/gpt-5.4-mini) and [GPT-6 Luna](https://developers.openai.com/api/docs/models/gpt-6-luna). No API key was available for implementation testing, so you must confirm your own authenticated reply and model access.

## 7. Use the two prediction models correctly

In **Income estimator**, try the clearly labelled example first, then use your source-consistent values for a real profile.

- The linear estimate is **total income in INR**, including farm and non-farm income. The reporting period and land unit are unspecified in the data; do not relabel them as annual income or acres.
- The logistic probability concerns whether total income falls below a benchmark defined by the fit-set median. It is a comparison within this dataset, not a government, poverty, or lending category.
- Cropping density and agricultural score are source indices. Use values matching the original definitions rather than making up a conversion.
- Rainfall and other inputs relate to historical source features. The app is not a verified live crop-season forecast.
- The displayed error band reflects validation performance and is not a promise for an individual. The data cannot prove that changing an input causes income to change.

In **Batch predictions**, download the blank `input_template.csv` or try `sample_batch.csv`, which contains hypothetical profiles. Enter your records while preserving the eight headers. Review the preview and fix flagged rows before downloading results. Keep uploads within 15,000 rows and 25 MB. Downloads use row numbers to match input records and exclude unused identifiers.

In **AI assistant**, **Share my last submitted estimate with the AI** starts unchecked. It is disabled before an estimate exists. Select it to attach only that profile and its actual model result; open **Review the profile and result that will be sent** first. **Clear conversation** clears history and unchecks sharing. Batch files are not attached automatically.

This release has no connected web search, weather, mandi-price, or scheme-update feed. Ask the assistant to explain concepts, but do not treat its replies as verified current facts. It should tell you when current information is unavailable.

## 8. Optional: run on your computer

Install [Python 3.12](https://www.python.org/downloads/) if needed. Open the extracted project folder in a terminal. On Windows PowerShell, use one command at a time:

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m streamlit run app.py
```

On macOS/Linux:

```bash
python3.12 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m streamlit run app.py
```

The `.venv` folder isolates packages for this project. Open the local web address shown, usually `http://localhost:8501`. Leave the terminal running; press `Ctrl+C` to stop.

For local AI, copy `.streamlit/secrets.toml.example` to `.streamlit/secrets.toml`, put your real key in that local copy, and restart if necessary. Never upload the real copy. Environment variables with the same names are an alternative configuration method.

## 9. Optional: repeat training for your assignment

Deploying the prepared package does not require retraining. Repeat training to understand the complete workflow, reproduce the evaluation, or deliberately use a revised dataset. Your source files are `lte_train(1).csv`, `lte_test(1).csv`, and `lte_dictionary(1).csv`; keep them privately. The dictionary explains source columns and is useful for your assignment; the training command uses the labelled training CSV and optionally the unlabeled test CSV.

### Easier training route: Google Colab

1. Open [Google Colab](https://colab.research.google.com/) and choose **File → Upload notebook**. Select the supplied `Train_Your_Models.ipynb`.
2. Read the notebook's introduction. Run its setup and upload cells in order using each cell's play button.
3. When requested, upload the supplied project ZIP and your private CSV copies. The notebook installs the pinned packages and runs `train.py`; follow its actual filename prompts.
4. Read the generated model report and download the trained deployment outputs. A hosted Colab runtime is temporary, so download before ending the session.
5. Review the generated files locally and keep them together. Replace a deployed model only after you have checked the new report and tested its predictions.

Raw datasets belong in the private runtime, not a public GitHub repository. A shared notebook can include saved cell outputs, so avoid displaying confidential raw rows in a copy you plan to share. Colab runs the training workflow; Streamlit Community Cloud hosts the app.

### Local training route

After installing dependencies using step 8, copy the original CSVs into a private folder such as `C:\Users\YOUR_NAME\Documents\FarmerData`. **First save a separate backup of the supplied project ZIP and its trained outputs.** The commands below use `--overwrite` to replace the prepared artifacts in the project folder deliberately; without that flag, the trainer refuses to overwrite an existing run. From the project root, use:

```powershell
.\.venv\Scripts\python.exe train.py --train "C:\Users\YOUR_NAME\Documents\FarmerData\lte_train(1).csv" --test "C:\Users\YOUR_NAME\Documents\FarmerData\lte_test(1).csv" --output-dir . --overwrite
```

On macOS/Linux, substitute your actual private paths:

```bash
.venv/bin/python train.py --train "/path/to/lte_train(1).csv" --test "/path/to/lte_test(1).csv" --output-dir . --overwrite
```

`--train` points to labelled training data, `--test` is optional, and `--output-dir .` saves generated artifacts in the current project folder. `--overwrite` confirms replacement of that folder's existing model outputs; use it only after making the backup. For a separate empty output directory, set a new `--output-dir` path and omit `--overwrite`. Omitting `--test` still trains and evaluates the models; it simply skips the optional unlabeled-file predictions.

### What the script does

1. Reads and validates your source data, separating the eight features from the income target.
2. Keeps each FarmerID in one split: about 60% fit, 20% calibration, and 20% holdout. IDs are grouping variables, not model predictors.
3. Learns missing-value handling, scaling, and state encoding from the fit split only. This prevents information from the later evaluation records leaking into training.
4. Fits ordinary-least-squares linear regression for total INR income. It labels fit income below the fit median and trains logistic regression with fixed `C=1` for that binary outcome.
5. Uses calibration residuals for the empirical error band, then evaluates the fitted models on the holdout without refitting them afterwards.
6. Writes `model_bundle.joblib`, `metrics.json`, `dashboard_data.json`, `MODEL_REPORT.md`, `input_template.csv`, and `sample_batch.csv`. If `--test` is supplied, also writes `outputs/lte_test_predictions.csv` without FarmerID. The optional predictions are private outputs, not deployment data.

A holdout is a reserved part of the labelled dataset used to measure performance. The separate `lte_test(1).csv` has no true income labels, so its predictions cannot tell you accuracy. Read actual MAE, RMSE, R², and classification metrics in `MODEL_REPORT.md` rather than describing all outputs as “accuracy”. Keep the trained bundle and matching metadata from one run together.

## 10. Test the deployed result

1. Confirm exactly four sections: Overview, Income estimator, Batch predictions, AI assistant.
2. Submit a valid example. Both the INR estimate and below-benchmark probability should appear.
3. Try a required field left blank, a negative value, and an unsupported state in a CSV. Confirm useful validation messages.
4. Upload `sample_batch.csv`, or fill `input_template.csv` with valid records before uploading it, then download predictions. The blank input template contains headers only and cannot produce predictions by itself. Check row numbers and the absence of unused IDs.
5. With a valid key, send a short AI question. Then opt into sharing an estimate and inspect its preview.
6. Clear the conversation and confirm both chat history and sharing reset.
7. In local testing, remove the key and confirm the estimator and batch sections remain usable.
8. Ask for today's local mandi price. The assistant should explain that it cannot verify current prices in this release.
9. Open the deployed app on a laptop and phone. Check that forms, charts, and results fit.

Read the release verification notes for what was actually tested. A saved metric in a JSON file is evidence from that evaluation run, not a new measurement every time the website opens.

## Assignment submission pack

Use the format requested by your professor. A practical submission includes:

- The completed `Train_Your_Models.ipynb`, with your executed training outputs.
- The generated `MODEL_REPORT.md`, explaining the method, actual evaluation scores, and limitations.
- Your working Streamlit app URL.
- Your new GitHub repository URL, with access provided if it is private.
- Screenshots of all four app sections, including an actual individual estimate and a completed batch result. Include a real AI reply only after configuring and testing your API key.

Use results from your actual run and label the hypothetical example profiles. Do not invent scores, replies, or a deployed URL. Keep raw borrower datasets and credentials outside the public submission files.

## 11. Logs, common errors, and rollback

While signed in, open your deployed app and select **Manage app** at the lower right. Cloud logs show installation and runtime errors; the panel's menu includes download, settings, and reboot options. Locally, read the terminal running Streamlit.

| Problem | First check |
| --- | --- |
| Missing package | `requirements.txt` is uploaded and dependency installation completed |
| Missing model | All prepared model/metadata files are at their expected paths |
| Model version warning | Use the supplied pinned versions and Python 3.12 |
| Missing photographs | Preserve the `assets/` folder names and paths |
| AI configuration message | Save the real key in this deployed app's Secrets tab |
| Authentication error | Key is active and copied correctly |
| Quota/rate-limit error | Check API billing, usage, model access, and limits |
| Old page still visible | Correct repository/branch/file are selected; wait for rebuilding and reload |
| CSV error | Use template headers, supported state names, and finite non-negative numbers |

Before changing code or models, download a backup with **GitHub → Code → Download ZIP**, and note the current commit. For recovery, restore the previous files as a new commit or use GitHub's revert flow for a merged change. Keep model files and metadata from the same release together. Code rollback does not restore secrets edited in Streamlit; manage those separately.

## Official help

Instructions checked on 18 October 2026:

- [Create a GitHub repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
- [Upload files](https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository)
- [Deploy Streamlit](https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/deploy)
- [Secrets management](https://docs.streamlit.io/deploy/streamlit-community-cloud/deploy-your-app/secrets-management)
- [App settings](https://docs.streamlit.io/deploy/streamlit-community-cloud/manage-your-app/app-settings)
- [Cloud logs and updates](https://docs.streamlit.io/deploy/streamlit-community-cloud/manage-your-app)
- [Colab FAQ](https://research.google.com/colaboratory/intl/en-GB/faq.html)
