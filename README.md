# Credit Card Approval Prediction

Welcome to a friendly and interactive credit-card approval demo built with Python, pandas, and scikit-learn. This project lets you explore how a simple machine-learning model makes approval decisions from applicant data and credit-history behavior.

> ✨ The experience is designed to feel clear, visual, and engaging, with highlights, structured steps, and easy-to-follow outputs.

---

## 🌟 What this project does

The system takes an applicant’s information, transforms it into useful features, trains a lightweight logistic-regression model, and then predicts whether the application should be:

- 🟢 Approved
- 🔴 Denied

It also adds a probability score and a short explanation so the decision feels more understandable.

### Included features

- 🧠 A simple trained prediction model
- 💬 An interactive terminal form for entering applicant details
- 📄 JSON output for each prediction
- 🗂️ A consolidated history of all past decisions
- 📦 Batch-style example runs for comparison

---

## 🎯 Main outcome of the model

For every applicant, the project returns:

- A final decision: Approved or Denied
- A probability score between 0 and 1
- A short reason summary
- A saved record of the full input data

> 💡 A probability closer to 1 means the model is more confident in approval, while a value closer to 0 means it is more confident in denial.

---

## 🛠️ Technologies used

This project uses a lightweight and beginner-friendly stack:

- Python 3
- pandas for data loading and feature engineering
- scikit-learn for training the model
- joblib for saving and loading the trained model
- JSON for saving predictions and history

---

## 📁 Project structure

- data and eda/
  - application_record.csv
  - credit_record.csv
  - credit-card-approval-prediction-notebook.ipynb
- models/
  - simple_credit_approval_model.py
  - predict_from_form.py
  - simple_credit_approval_model.joblib
  - predictions/
    - prediction_summary.json
    - prediction_history.json
    - batch_examples/
    - batch_2/
    - performance_demo/

---

## 🧾 Dataset schema

The project uses two main input files.

### 1) Application record schema

The file application_record.csv contains the applicant’s main profile details.

| Column | Description |
|---|---|
| ID | Unique applicant identifier |
| CODE_GENDER | Gender of the applicant |
| FLAG_OWN_CAR | Whether the applicant owns a car |
| FLAG_OWN_REALTY | Whether the applicant owns real estate |
| CNT_CHILDREN | Number of children |
| AMT_INCOME_TOTAL | Annual income |
| NAME_INCOME_TYPE | Type of income source |
| NAME_EDUCATION_TYPE | Education level |
| NAME_FAMILY_STATUS | Family or marital status |
| NAME_HOUSING_TYPE | Housing situation |
| DAYS_BIRTH | Age-related value in days (negative values) |
| DAYS_EMPLOYED | Employment duration in days (negative values; 365243 means unemployed/unknown) |
| FLAG_MOBIL | Mobile phone flag |
| FLAG_WORK_PHONE | Work phone flag |
| FLAG_PHONE | Phone flag |
| FLAG_EMAIL | Email flag |
| OCCUPATION_TYPE | Occupation category |
| CNT_FAM_MEMBERS | Total family members |
| DAYS_REGISTRATION | Days since registration |
| DAYS_ID_PUBLISH | Days since ID publication |

### 2) Credit record schema

The file credit_record.csv contains monthly credit-history behavior.

| Column | Description |
|---|---|
| ID | Applicant identifier linked to application_record.csv |
| MONTHS_BALANCE | Month index for the balance record |
| STATUS | Credit status code for that month |

The STATUS values are converted into numeric categories for modeling. Examples include:

- X, C, 0 = neutral or resolved credit behavior
- 1, 2, 3, 4, 5 = increasing severity of late or risky behavior

---

## 🔧 Feature engineering

The model does not learn from the raw tables alone. It first creates smarter signals from them.

### From the application data

The script creates:

- EMPLOYED: whether the applicant is currently employed
- YEARS_EMPLOYED: years of employment derived from DAYS_EMPLOYED
- INCOME_EMPLOY_RATIO: income divided by employment years, helping show income sustainability

### From the credit-history data

The script creates useful summaries such as:

- STATUS_MAX: worst credit status observed
- STATUS_MIN: best credit status observed
- STATUS_MEAN: average credit status
- STATUS_LAST: latest recorded status
- STATUS_TREND: recent direction of behavior
- NUM_LATE_MONTHS: count of late or delinquent months

These engineered features are the main inputs the model learns from.

---

## 🧪 How the model is trained

Training is handled by simple_credit_approval_model.py.

### Training workflow

1. Load the applicant and credit datasets.
2. Merge the applicant profile with the credit-history summaries.
3. Create engineered features.
4. Build a target label using a conservative approval rule.
5. Split the data into training and testing sets.
6. Train a logistic-regression classifier with balancing.
7. Save the trained model and the decision threshold.

### Training target logic

The target is built from several signals, including:

- Income level
- Employment stability
- Credit history quality
- Family size and household balance

This makes the model more selective and helps avoid approving almost every applicant.

---

## 🔮 How predictions work

Predictions are handled by predict_from_form.py.

### Interactive workflow

1. The script asks for applicant details one by one.
2. It transforms the answers into the same engineered features used during training.
3. It sends them to the trained model.
4. The model returns a probability score.
5. The script compares that score to a threshold.
6. A final decision is displayed:
   - 🟢 Approved if the score meets the threshold
   - 🔴 Denied otherwise

### Probability and threshold

The model outputs a probability rather than a raw yes/no answer.

- Values near 1 indicate strong approval confidence
- Values near 0 indicate strong denial confidence

The current decision threshold is 0.6.

---

## 💬 How reasons are generated

Each prediction also comes with a short reason summary.

These reasons are simple heuristic explanations based on the final decision and confidence level. They are designed to make the output feel more human-readable and easier to interpret.

Examples include:

- Strong financial profile and low-risk indicators
- Moderate confidence approval based on available signals
- High-risk pattern detected; strong reasons for denial
- Mixed profile with notable risk signals; review recommended

---

## 📦 What gets saved

Every interactive prediction is stored in the predictions folder.

### Saved outputs

- A JSON file for each applicant prediction
- A consolidated summary file with the full history
- Batch example outputs for side-by-side inspection
- Demo profiles for comparison

This makes it easy to review and compare many different outcomes.

---

## ▶️ How to run it

### Train the model

```bash
python models/simple_credit_approval_model.py
```

### Run the interactive prediction form

```bash
python models/predict_from_form.py
```

The script will prompt for values such as:

- Applicant ID
- Gender
- Income
- Employment status
- Education level
- Family status
- Housing type
- Basic credit-related flags

---

## ✨ Best way to use it

The most enjoyable way to explore the project is:

1. Run the interactive form for one applicant
2. Review the decision, probability, and reason
3. Open the saved JSON file
4. Compare several applicants using the batch examples

This makes the project feel less like a static notebook and more like a small decision assistant.

---

## ✅ Summary

This repository shows a complete mini workflow for credit-card approval prediction:

- It uses applicant and credit-history data
- It creates useful engineered features
- It trains a simple but effective logistic-regression model
- It predicts approval or denial
- It explains the result in a simple, friendly way
- It saves each decision for review and experimentation

It is intentionally simple, transparent, and interactive so the whole process is easy to understand and fun to explore.