# Project Log: Probation Population Trends Analysis

## 1. Project Objective
I initiated this project to build an end-to-end data pipeline that mirrors the complexities of real-world criminal justice data. My primary goal was to demonstrate the ability to handle "dirty" administrative records, protect sensitive information (PII), and derive actionable insights regarding recidivism risks.

## 2. Architecture & Workflow Decisions
I deliberately moved away from a single-file approach to a modular architecture to ensure scalability and reproducibility.

* **Modular Separation:** I isolated the workflow into three distinct stages:
    * `01_data_cleaning/`: Raw ingestion and cleaning logic.
    * `02_analysis/`: Statistical exploration (EDA).
    * `03_visualization/`: Final reporting assets.
* **Data Isolation:** I created a dedicated `04_data/` directory to separate input/output files from code, ensuring that data processing steps are clear and traceable.

## 3. Data Privacy & Security Implementation
**Challenge:** Real probation data contains sensitive Personally Identifiable Information (PII) which cannot be hosted on public repositories.
**Solution:**
1.  **Git Ignore Strategy:** I configured `.gitignore` to strictly exclude all files within `04_data/raw/` and `04_data/processed/`. This ensures that even if I work with real files locally, they can never be accidentally pushed to GitHub.
2.  **Synthetic Data Generation:** To allow others to verify my code without compromising privacy, I wrote a Python script (`generate_data.py`) to manufacture a 15,000-row synthetic dataset that mathematically mimics the distribution and errors of real caseloads.

## 4. Engineering "Dirty" Data
To prove my data cleaning capabilities, I did not want a perfect dataset. I engineered specific data quality issues into the generation script to simulate a broken ETL pipeline:
* **Type Inconsistencies:** I forced the `COMPAS_Score` column to mix integers (1-10) with text strings ("High", "Med") to require robust type coercion.
* **Logical Fallacies:** I introduced date errors where `Actual_Discharge_Date` occurs *before* the `Sentence_Date`.
* **Clerical Errors:** I injected typos into categorical variables (e.g., "Revoked" vs "Revoked - Tech") and added duplicate rows to simulate data entry mistakes.

## 5. Next Steps
The immediate next phase of this project involves writing the cleaning scripts in `01_data_cleaning` to systematically detect, flag, and remediate the errors listed above.