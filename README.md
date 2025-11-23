## Project Architecture & Reproducibility
To ensure the analysis is reproducible and modular, this project follows a strict directory structure:

* **`01_data_cleaning/`**: Scripts to ingest raw data and handle null values/inconsistencies.
* **`02_analysis/`**: Exploratory Data Analysis (EDA) and statistical modeling.
* **`03_visualization/`**: Dashboard assets and final reporting outputs.
* **`04_data/`**: Local storage for input/output files. Divided into `raw` (read-only) and `processed` (clean).

## Data Privacy & Ethics
**Note on Data Availability:**
Due to the sensitive nature of probation records (PII), the raw dataset (`data/raw`) is **not included** in this repository.
* A `.gitignore` file was configured to strictly exclude all Excel/CSV files in the data directory to prevent accidental leakage of sensitive information.
* To run this code, a user would need to supply their own synthetic or anonymized dataset matching the schema described in the data dictionary.

## Data Dictionary & Synthetic Generation
To respect privacy laws regarding probation records, this project uses a **synthetic dataset** generated via Python (`generate_data.py`). The script creates 15,000 records mimicking real-world "dirty" data patterns found in government systems.

### Schema
| Column | Description | Data Type |
| :--- | :--- | :--- |
| **DOC_Number** | Unique Department of Corrections ID | Integer |
| **COMPAS_Score** | Recidivism Risk Score (1-10 or Text) | Mixed |
| **Sentence_Date** | Date probation supervision began | Date |
| **Discharge_Date** | Projected end of supervision | Date |
| **Actual_Discharge_Date** | Realized end date (if applicable) | Date |
| **Discharge_Type** | Outcome (Successful, Revoked, etc.) | String |
| **Assigned_Officer** | Supervising Probation Agent | String |

### Intentional "Dirty" Data Logic
The generation script injects specific data quality issues to simulate a raw ETL pipeline environment:
* **Inconsistent Encodings:** `COMPAS_Score` varies between integers (1-10) and text strings ("High", "Med", "Low") within the same column.
* **Temporal Logic Errors:** `Actual_Discharge_Date` occasionally predates `Sentence_Date`.
* **Clerical Typos:** `Discharge_Type` contains variations (e.g., "Revoked" vs. "Revoked - Tech").
* **Missingness:** Null values introduced in `Actual_Discharge_Date` for active cases.
