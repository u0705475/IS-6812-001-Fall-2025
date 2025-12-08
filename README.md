```markdown
# MKT6600 — Home Credit Capstone

Short description
-----------------
This repository contains the analyses and models for the Home Credit business problem: predicting loan default (TARGET) for an underbanked population using the Home Credit dataset. The project includes exploratory data analysis (EDA), feature engineering experiments (human vs AI-assisted), modeling (logistic regression, random forest, gradient boosting), resampling experiments, and an ensemble approach evaluated by AUC.

Repository structure
--------------------
- data/                - (not tracked) raw and processed data (see data/README.md)
- notebooks/           - EDA and modeling notebooks (notebooks or HTML exports)
- src/                 - scripts and modules (preprocessing, training, evaluation)
- reports/             - HTML reports and model output (EDA.html, Modeling.capstone.html)
- requirements.txt     - pinned Python package list
- README.md            - this file

Quick links
-----------
- EDA report: reports/EDA.html (or open: https://github.com/user-attachments/files/24023878/EDA.html)
- Modeling report: reports/Modeling.capstone.html (or open: https://github.com/user-attachments/files/24023888/Modeling.capstone.html)
- Detailed dataset retrieval & placement instructions: data/README.md

Prerequisites
-------------
- Python 3.8+
- NumPy == 2.3.4
- scikit-learn == 1.5.1
- Git
- (Optional) conda for environment management

Setup — local
-------------
1. Clone the repo:
   git clone https://github.com/u0705475/MKT6600.git

2. Create and activate a virtual environment (Windows example shown; adapt to your OS if different):
   - Create venv:
     python -m venv .venv
   - Activate venv (Windows PowerShell):
     .venv\Scripts\Activate.ps1
     or (Windows CMD):
     .venv\Scripts\activate.bat

   If you prefer conda:
     conda create -n mkt6600 python=3.8
     conda activate mkt6600

3. Install dependencies:
   - If a requirements.txt file exists, install with:
     pip install -r requirements.txt
   - If you use conda and there is an environment.yml, create it with:
     conda env create -f environment.yml

   Notes:
   - The project was developed and tested with NumPy==2.3.4 and scikit-learn==1.5.1; pin these exact versions in requirements.txt or your environment to reproduce results.
   - "Install dependencies" means using pip (or conda) to install the Python libraries required to run the scripts and notebooks.

4. Place datasets (see data/README.md) into data/raw/

5. Open notebooks or HTML reports:
   - Jupyter notebooks: jupyter notebook or jupyter lab
   - HTML: open reports/EDA.html in a browser

Data
----
This project uses the Home Credit dataset (Home Credit Default Risk). The raw data is not included in this repo. See data/README.md for the instructor-provided download link and exact instructions.

Expected input files (how scripts assume the data is organized)
- data/raw/application_train.csv
- data/raw/application_test.csv
- data/raw/bureau.csv

Notes about data usage
- The analysis merges bureau.csv with application_train.csv and application_test.csv as part of feature construction. Do not commit raw data to this repository — keep raw files in data/raw/ locally and out of version control.
- For dataset retrieval instructions (Canvas link and any access notes), see data/README.md.

Business problem — summary
--------------------------
Lenders need to estimate the probability that an individual borrower will default on a loan so they can make better underwriting, pricing, and portfolio-management decisions. This project uses the Home Credit dataset to build and compare credit-risk models that predict the binary outcome TARGET (default vs. non-default) for retail loan applicants. Improving the accuracy and stability of default predictions helps the business:

- Reduce expected credit losses by declining or repricing high-risk applicants.
- Increase approval rates for low-risk applicants by enabling more confident underwriting.
- Prioritize collections and risk-mitigation efforts on accounts with higher predicted loss probability.
- Evaluate model fairness and feature-driven risk signals to reduce unintended bias.

Project objective
-----------------
The objective of this capstone is to produce a reproducible, well-documented pipeline that demonstrates how to go from raw Home Credit data to evaluated credit-risk models and a candidate production artifact. Concretely, the project aims to:

1. Prepare and document a reproducible data-processing pipeline that merges application and bureau records and produces tidy, model-ready datasets.
2. Establish a baseline model and iteratively improve it using feature engineering and model tuning (examples: logistic regression baseline, random forest, gradient boosting).
3. Evaluate models primarily by AUC using 5‑fold cross‑validation, compare performance across algorithms, and produce an ensemble that aggregates strengths of individual models.
4. Deliverables:
   - Reproducible preprocessing, training, and evaluation scripts under src/.
   - Notebooks and HTML reports containing EDA, feature engineering rationale, and modeling experiments under notebooks/ and reports/.
   - A model artifact (models/*.joblib) and a short “how to use” guide for generating predictions.
   - A README and data/README.md documenting data acquisition, expected inputs, environment, and usage.
5. Success criteria:
   - Models achieve the reported validation AUCs (baseline 0.5498; tuned LR 0.7508; RF 0.7423; GBM 0.7580; ensemble 0.7570) on held-out folds in the 5‑fold CV setup, and the code reproduces these experiments locally.
   - Code is organized, tested (where feasible), and contains clear instructions so another analyst can reproduce preprocessing, training, and evaluation steps.

Contributions & role
--------------------
Sina Odejinmi (Author: u0705475) led and contributed to the EDA and model validity work for this project. Specific contributions included:

- Designing and executing exploratory data analysis methods to surface data issues and candidate features.
- Validating model behavior and using diagnostic tools (confusion matrix, calibration plots) to ensure models behaved as expected across score ranges.
- Using the confusion matrix to help identify an operational probability cutoff with the least false positive rate consistent with business profitability targets.
- Documenting preprocessing decisions and the evaluation approach used to select the final candidate model.

Group solution and operational decision
--------------------------------------
Our group's solution to the business problem was to construct several candidate models (baseline logistic regression, tuned logistic regression, random forest, gradient boosting, and an ensemble) and select the single model with the highest validation AUC to generate predictions on the test set.

Decision rule and operational cutoff:
- We used a probability cutoff of 0.5 to identify high-risk applicants. With this cutoff we rejected (declined) the riskiest top ~5% of applicants by predicted probability, prioritizing reduction in actual defaults while controlling false positives (applicants predicted to default but who would have repaid).

Observed impact of the chosen cutoff and policy:
- Overall default rate reduced from 8.0% to 6.9% after applying the cutoff.
- Using the 0.5 cutoff:
  - Removes ~20% of actual defaulters from the active portfolio (reduction in number of defaulters).
  - Avoids approximately $16M in annual losses per 100,000 loans (estimated loss avoidance based on modeled exposure and loss severity assumptions).
  - Approves ~95% of applicants (approval rate after rejecting the top ~5% highest-risk applicants).

Notes on interpretation and deployment
- The numbers above are summary statistics from our evaluation and depend on assumptions about loan exposure, average loss given default, and the distribution of applicant risk in the deployment population. Before operational deployment, validate these savings with financial/legal stakeholders and perform a sensitivity analysis on cutoff choice (e.g., examine tradeoffs at cutoffs 0.4, 0.5, 0.6).
- Monitor post-deployment metrics (default rate, approval rate, false positive rate, income and demographic impacts) and retrain models periodically to account for data drift.
- Consider adding a business-level wrapper that can vary the cutoff dynamically based on portfolio targets (loss budget, approval volume) and fairness constraints.

Challenges and limitations
--------------------------
Key challenges the group encountered during the project:

- High missingness: many raw predictors had missing values approaching ~70%, which made feature engineering and model stability challenging. We used imputation and aggregation strategies, but high missingness limited which predictors could be used reliably.
- Feature grouping: we were unable to fully explore grouping predictors into small, similar groups and performing group-wise importance/drop tests to remove low-impact groups; this remains an area for future work.
- Cross-tool workflow friction: some Python code (local analysis scripts) did not run cleanly on the R-focused platform used by other team members. To maintain progress, the group shifted to using the R platform for the remaining work and results integration, which required reimplementation and cross-checks of key steps.

How to reproduce the analysis
-----------------------------
- Preprocess:
  python -m src.preprocess --input data/raw --output data/processed

- Train:
  python -m src.train --data data/processed --model-dir models

- Evaluate:
  python -m src.evaluate --data data/processed --model models/logistic_model.joblib

- Or open and run the notebooks in notebooks/ in order:
  1. notebooks/eda.ipynb  (exploratory analysis)
  2. notebooks/feature_engineering.ipynb
  3. notebooks/modeling.ipynb

How to use
----------
This section gives concrete, tested commands and describes inputs/outputs for common tasks.

1) Prepare data (manual step)
   - Download the instructor-provided Home Credit CSVs (see data/README.md) and place them into:
     data/raw/application_train.csv
     data/raw/application_test.csv
     data/raw/bureau.csv

2) Preprocess (creates processed CSVs)
   - Run the preprocessing step to produce processed files used by the models:
     python -m src.preprocess --input data/raw --output data/processed
   - Expected outputs:
     - data/processed/application_train_processed.csv
     - data/processed/application_test_processed.csv
     - additional feature files depending on preprocessing steps (e.g., bureau_aggregates.csv)
   - If filenames differ, either rename your CSVs in data/raw/ or update the filename checks in src/preprocess.py.

3) Train (produces model artifacts)
   - Train models using the processed training data:
     python -m src.train --data data/processed --model-dir models
   - Notes:
     - The experiments reported in this repository were evaluated using 5‑fold cross‑validation to balance runtime and estimate stability. The training scripts can be extended to change the number of folds; check src/train.py for available CLI options or add a --cv flag if needed.
     - Training produces a model artifact saved under models/, for example:
       models/logistic_model.joblib

4) Evaluate (compute metrics on processed dataset or test set)
   - Evaluate a saved model:
     python -m src.evaluate --data data/processed --model models/logistic_model.joblib
   - The evaluation step prints metrics (AUC) and can be extended to save detailed reports or plots under reports/.

5) Quick example (Windows PowerShell)
   - Activate venv:
     .venv\Scripts\Activate.ps1
   - Install deps:
     pip install -r requirements.txt
   - Preprocess:
     python -m src.preprocess --input data/raw --output data/processed
   - Train:
     python -m src.train --data data/processed --model-dir models
   - Evaluate:
     python -m src.evaluate --data data/processed --model models/logistic_model.joblib

Tips and notes
- If a script expects a filename that differs from what you downloaded, either rename the CSVs in data/raw/ or update the filename references inside src/preprocess.py.
- The 5‑fold cross‑validation used for the reported experiments trades off runtime vs. stability; for final model training consider increasing folds if computational resources allow.
- Keep data/ listed in .gitignore to avoid committing sensitive or large files.

Summary of results
------------------
- Baseline logistic regression AUC: 0.5498
- Tuned logistic regression with interactions AUC: 0.7508
- Random Forest AUC: 0.7423
- Gradient Boosting AUC: 0.7580
- Ensemble AUC: 0.7570

Interpretation
--------------
- The tuned logistic regression and gradient boosting models substantially improved over the baseline logistic regression. The final ensemble delivered a small uplift over the single best model; inspect feature importances and calibration to assess practical utility before deployment.

Recommendations / notes
-----------------------
- Keep data/ listed in .gitignore to avoid committing sensitive or large files.
- Pin versions in requirements.txt (NumPy==2.3.4, scikit-learn==1.5.1) so others can reproduce results.
- Add a brief Methods section describing preprocessing, resampling, and evaluation metrics (5‑fold CV used for experiments reported).
- Document hyperparameters and cross-validation scheme used for final models (best params, folds, random seed).
- Add a small "How to use" section that explains how to run a script to get the final model predictions and where outputs are saved (see above).

Contributors
------------
- Miles McCunniff
- Sebastian Perez Parra
- Sina Odejinmi

Contact
-------
Author: Sina Odejinmi  
Email: u0705475@utah.edu
```
