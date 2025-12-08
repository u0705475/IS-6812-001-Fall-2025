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
- The 5‑fold cross‑validation used for the reported experiments trades off runtime vs. estimate stability; for final model training consider increasing folds if computational resources allow.
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

Contact
-------
Author: Sina Odejinmi  
Email: u0705475@utah.edu
```
