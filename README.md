# REAL-TIME UPI / DIGITAL PAYMENT FRAUD DETECTION

**Context-Aware Fraud Intelligence and Cost-Sensitive Payment Decision
System**

## 1. Project overview

This project analyses UPI-style transactions and evaluates fraud
detection using:

-   Supervised classification: Logistic Regression, Random Forest and
    XGBoost.
-   Class-imbalance approaches: class weighting and SMOTENC within a
    training pipeline.
-   Unsupervised anomaly detection: Isolation Forest, trained without
    fraud labels.
-   Cost-sensitive threshold analysis and a three-action decision
    framework: Approve, Step-up OTP and Block.

The supplied dataset contains 250,000 transactions, including 480
labelled fraud cases (0.192%). The project therefore reports precision,
recall, F1-score and PR-AUC rather than relying on accuracy alone.

## 2. Project files

Place the files in a working folder with a structure similar to this:

``` text
project-folder/
├── README.md
├── 01_Data_Audit.ipynb
└── upi_transactions 2024(1).csv
```

If your notebook has a different filename, use that filename when
opening it. Keep the CSV in the same folder as the notebook, or update
the dataset path in the notebook.

## 3. Requirements and dependencies

Recommended environment: Python 3.10 or 3.11.

Required Python packages:

-   pandas
-   numpy
-   matplotlib
-   seaborn
-   scikit-learn
-   imbalanced-learn
-   xgboost
-   jupyter
-   ipykernel

Install Python first, then open a terminal in the project folder.

### Install dependencies

``` bash
python -m venv .venv
```

Activate the environment:

**Windows PowerShell**

``` powershell
.venv\Scripts\Activate.ps1
```

**Windows Command Prompt**

``` cmd
.venv\Scripts\activate.bat
```

**macOS / Linux**

``` bash
source .venv/bin/activate
```

Install the packages:

``` bash
python -m pip install --upgrade pip
python -m pip install pandas numpy matplotlib seaborn scikit-learn imbalanced-learn xgboost jupyter ipykernel
```

If using VS Code, install the **Python** and **Jupyter** extensions,
then select the `.venv` Python interpreter as the notebook kernel.

## 4. Dataset setup

1.  Put `upi_transactions 2024(1).csv` in the project folder.
2.  Open the notebook in VS Code or Jupyter.
3.  Find the cell that loads the CSV.
4.  Confirm the path matches the actual filename and folder location.

Example when the CSV is in the same folder:

``` python
import pandas as pd

df = pd.read_csv("upi_transactions 2024(1).csv")
```

Do not change the target column name or silently substitute a different
dataset. The expected fraud target is `fraud_flag`.

## 5. Execution instructions

1.  Open the main `.ipynb` notebook in VS Code or Jupyter.
2.  Select the project virtual environment as the kernel.
3.  Confirm the dataset loads successfully.
4.  Run the notebook cells from top to bottom in order.
5.  Check each section's printed outputs and charts.
6.  Save the notebook after execution.

The notebook is organised around the challenge problem statements:

-   **PS1 --- Data Quality & Reliability:** inspect the dataset, missing
    values, duplicates, data types, timestamps and eligible features.
-   **PS2 --- Fraud Lens & Accuracy Trap:** examine class imbalance and
    the always-genuine baseline.
-   **PS3 --- Fraud Pattern Analysis:** compare observed fraud rates
    across available transaction attributes.
-   **PS4 --- Supervised Fraud Detection:** use a chronological
    train/validation/test split, compare models and evaluate imbalance
    handling.
-   **PS5 --- Anomaly Detection:** train Isolation Forest without fraud
    labels and evaluate anomaly rankings afterwards.
-   **PS6 --- Cost-Based Threshold:** compare precision-recall
    performance and examine illustrative cost assumptions.
-   **PS7 --- Three-Band Decision System:** convert validation scores
    into Approve, Step-up OTP and Block actions.
-   **PS8 --- Findings & Recommendations:** summarise the evidence and
    provide exactly three prioritised management recommendations.

Run cells in order because later sections use variables created earlier.
If a kernel is restarted, rerun from the first cell.

## 6. Assumptions and evaluation rules

-   The supplied CSV is the source dataset for the reported analysis.
-   `fraud_flag` is the target label. It must not be included among
    model input features.
-   Transaction identifiers and timestamps are excluded from model input
    features. Transaction status is excluded because it may reflect
    information available after a payment decision.
-   No balance columns are used as fraud prediction features.
-   Data is sorted by timestamp and split chronologically: 70% training,
    15% validation and 15% test.
-   Model comparison and threshold selection use validation data. The
    test set is reserved for final evaluation and must not be used for
    model or threshold tuning.
-   Resampling must be applied only within the training pipeline/fold.
    Do not resample validation or test data.
-   Isolation Forest is trained without fraud labels. Labels are used
    only after scoring to evaluate the anomalies.
-   Accuracy is reported only to demonstrate the accuracy trap.
    Precision, recall, F1-score and PR-AUC are more informative for this
    rare-fraud dataset.
-   Cost values are illustrative team assumptions because numerical
    values were not provided by the organisers: missed fraud ₹5,000,
    false block ₹500 and OTP challenge ₹5.
-   The assumed OTP prevention effectiveness is not measured. No claim
    is made that OTP challenges prevent a specific percentage of fraud.
-   The provisional three-band thresholds (Approve below 0.30; Step-up
    OTP from 0.30 to below 0.70; Block at or above 0.70) are
    demonstration settings, not production-approved thresholds.
-   Cost outputs are scenario estimates, not observed savings or actual
    operating costs.
-   The dataset does not support claims about repeat offenders,
    account-level transaction velocity, device novelty or
    transaction-network relationships where the required
    identifiers/history are unavailable.
-   Results describe patterns in the supplied dataset; they do not
    establish causation or guarantee real-world performance.

## 7. Reproducibility

The notebook uses fixed random seeds in the model configurations where
applicable (for example, `random_state=42`). For reproducible results:

-   Use the same dataset version.
-   Use the same package environment.
-   Run the notebook from top to bottom.
-   Do not change the chronological split, feature exclusions or
    validation thresholds without documenting the change.

Package versions can be recorded in the active environment with:

``` bash
python --version
python -m pip freeze > requirements-lock.txt
```

## 8. Troubleshooting

**The CSV file is not found**

-   Confirm the CSV is in the same folder as the notebook.
-   Check spelling, spaces and file extension.
-   Otherwise, update the path in the CSV loading cell.

**A package import fails**

-   Activate the project virtual environment.
-   Install the missing package using
    `python -m pip install package-name`.
-   Restart the notebook kernel and select the correct interpreter.

**The notebook uses the wrong kernel**

-   In VS Code, use the kernel selector in the top-right of the
    notebook.
-   Select the Python interpreter from `.venv`.

**XGBoost or SMOTENC is unavailable**

-   Install `xgboost` and `imbalanced-learn` using the installation
    command above.
-   Restart the kernel after installation.

## 9. Important deployment note

This project is a hackathon prototype and analytical demonstration. Its
validation results are not strong enough to justify production
deployment or fully automated payment blocking. Real deployment would
require stronger and more representative data, validated business costs,
operational testing, model monitoring, customer-impact assessment and
appropriate security/compliance review.
