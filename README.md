# MKT6600 — Home Credit Capstone

Short description
-----------------
This repository contains the analyses and models for the Home Credit business problem: predicting loan default (TARGET) for an underbanked population using the Home Credit dataset. The project includes exploratory data analysis (EDA), feature engineering experiments (human vs AI-assisted), modeling (logistic regression, random forest, gradient boosting), resampling experiments, and an ensemble approach evaluated by AUC.

Repository structure
--------------------
- data/                - (not tracked) raw and processed data (see Data section)
- notebooks/           - EDA and modeling notebooks (notebooks or HTML exports)
- src/                 - scripts and modules (preprocessing, training, evaluation)
- reports/             - HTML reports and model output (EDA.html, Modeling.capstone.html)
- requirements.txt     - pinned Python package list (create this)
- README.md            - this file

Quick links
-----------
- EDA report: reports/EDA.html (or open: https://github.com/user-attachments/files/24023878/EDA.html)
- Modeling report: reports/Modeling.capstone.html (or open: https://github.com/user-attachments/files/24023888/Modeling.capstone.html)

Prerequisites
-------------
- Python 3.8+ (or the version you used)
- Git
- (Optional) conda for environment management

Setup — local
-------------
1. Clone the repo:
   git clone https://github.com/u0705475/MKT6600.git
2. Create environment:
   python -m venv .venv
   source .venv/bin/activate   # macOS/Linux
   .venv\Scripts\activate      # Windows
3. Install dependencies (create requirements.txt first if missing):
   pip install -r requirements.txt
4. Place datasets (see Data section) into data/raw/
5. Open notebooks or HTML reports:
   - Jupyter notebooks: jupyter notebook or jupyter lab
   - HTML: open reports/EDA.html in a browser

Data
----
This project uses the Home Credit dataset (Home Credit Default Risk). The raw data is not included in this repo. To reproduce the analysis:
- Download the dataset from (insert authoritative link e.g., Kaggle Home Credit Default Risk) and place the CSVs into data/raw/.
- Keep personally-identifiable information out of the repo and follow dataset license/usage terms.

How to reproduce the analysis
-----------------------------
- Run preprocessing script (if present): python src/preprocess.py --input data/raw --output data/processed
- Run training script: python src/train.py --config configs/train.yml
- Or open and run the notebooks in notebooks/ in order:
  1. notebooks/eda.ipynb  (exploratory analysis)
  2. notebooks/feature_engineering.ipynb
  3. notebooks/modeling.ipynb

Summary of results
------------------
- Baseline logistic regression AUC: <add value>
- Tuned logistic regression with interactions AUC: <add value>
- Random Forest AUC: <add value>
- Gradient Boosting AUC: <add value>
- Ensemble AUC: <add value>

(Replace <add value> with your final evaluation numbers and short interpretation.)

Recommendations / notes
-----------------------
- Keep data/ listed in .gitignore to avoid committing sensitive data.
- Add a requirements.txt (or environment.yml) with pinned versions so others can reproduce results.
- Add a LICENSE (e.g., MIT) and a short CONTRIBUTING.md if you want collaborators.
- Add a brief Methods section describing preprocessing, resampling, and evaluation metrics.
- Document hyperparameters and cross-validation scheme used for final models.
- Add a small "How to use" section that explains how to run a script to get the final model predictions.

Contributing
------------
If you want me to contribute changes (add a proper README, requirements.txt, .gitignore, or CI), tell me which files I should create or update and I will prepare a branch/PR.

Contact
-------
Author: u0705475
Email: (add preferred contact)
