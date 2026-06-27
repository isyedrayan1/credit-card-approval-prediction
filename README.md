# Credit Card Approval Prediction

A machine learning system built with Python, pandas, and scikit-learn to automate credit card application screening and predict applicant eligibility based on demographic attributes and historical repayment behaviors.

---

## System Overview

The application processes applicant profiles and monthly credit ledgers to predict whether a credit card application should be Approved or Denied. The system provides:

- An approval decision (Approved/Denied).
- A probability confidence score.
- A detailed multi-point audit of risk metrics.
- Locally logged JSON audit trails for each evaluated applicant.

---

## Technical Stack

- **Python 3**
- **pandas** for data ingestion, cleaning, and preprocessing
- **scikit-learn** for training and evaluating machine learning classifiers
- **Flask** for serving the web application and prediction API
- **joblib** for serializing and loading trained model pipelines
- **Jinja2** for modular HTML views

---

## Project Structure

- **app/**: Backend package containing preprocessing modules.
- **frontend/**: Client-side Jinja2 template views (Dashboard, Predictor, Documentation).
- **models/**: Trained model artifacts and predictions history log directories.
- **notebook/**: Jupyter notebooks for EDA and initial model training.
- **project documentation/**: Folder reserved for project specifications.

---

## Dataset Schema

The system integrates data from two primary inputs linked by a unique ID key.

### 1. Application Record Schema (application_record.csv)

Contains the applicant's demographic and financial profile metrics.

| Column Name | Data Type | Description |
|---|---|---|
| ID | Numerical | Unique applicant identifier |
| CODE_GENDER | Categorical | Gender of the applicant |
| FLAG_OWN_CAR | Categorical | Car ownership flag (Y/N) |
| FLAG_OWN_REALTY | Categorical | Real estate ownership flag (Y/N) |
| CNT_CHILDREN | Numerical | Number of children |
| AMT_INCOME_TOTAL | Numerical | Annual income |
| NAME_INCOME_TYPE | Categorical | Source type of income |
| NAME_EDUCATION_TYPE | Categorical | Completed education level |
| NAME_FAMILY_STATUS | Categorical | Marital status |
| NAME_HOUSING_TYPE | Categorical | Housing situation |
| DAYS_BIRTH | Numerical | Age in days (negative value counting backward from evaluation date) |
| DAYS_EMPLOYED | Numerical | Employment length in days (negative; 365243 represents unemployed) |
| FLAG_MOBIL | Categorical | Mobile phone registry flag (1/0) |
| FLAG_WORK_PHONE | Categorical | Work phone registry flag (1/0) |
| FLAG_PHONE | Categorical | Home phone registry flag (1/0) |
| FLAG_EMAIL | Categorical | Email registry flag (1/0) |
| OCCUPATION_TYPE | Categorical | Job category |
| CNT_FAM_MEMBERS | Numerical | Total family size |

### 2. Credit Record Schema (credit_record.csv)

Contains historical monthly credit balance and payment delay indicators.

| Column Name | Data Type | Description |
|---|---|---|
| ID | Numerical | Applicant identifier |
| MONTHS_BALANCE | Numerical | Month index for the balance record (negative offset) |
| STATUS | Categorical | Repayment status code (X=No loan, C=Closed, 0=On-time, 1-5=Months Late) |

---

## Feature Engineering & Preprocessing

Raw data columns are preprocessed and engineered into high-signal feature vectors:

### Application Features
- **EMPLOYED**: Binary flag representing current employment status.
- **YEARS_EMPLOYED**: Float representation of employment duration in years.
- **INCOME_EMPLOY_RATIO**: Ratio of annual income to years employed.
- **AGE**: Numerical age of the applicant.

### Credit Ledger Aggregations
- **STATUS_MAX**: Worst historical payment status code.
- **STATUS_MIN**: Best historical payment status code.
- **STATUS_MEAN**: Average payment status score.
- **STATUS_LAST**: Most recent monthly payment status code.
- **STATUS_TREND**: Payment trend coefficient.
- **NUM_LATE_MONTHS**: Count of delinquent months.

---

## Model Training & Performance

Model classification is trained inside the training pipelines:
1. Merges demographic profiles with credit summaries.
2. Applies feature engineering pipelines and imputes missing occupation flags.
3. Generates target labels using a multi-rule heuristic risk policy.
4. Trains a Random Forest Classifier to achieve stable validation accuracy.

---

## Running the Application

### Training the Models
To run the model training script:
```bash
python models/simple_credit_approval_model.py
```

### Running the Flask Web Server
To launch the interactive dashboard and predictor application:
```bash
python main.py
```
Open your browser and navigate to `http://127.0.0.1:5000` to access the interface.