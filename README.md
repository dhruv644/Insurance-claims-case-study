# 🏥 Insurance Claims — EDA & Hypothesis Testing

A end-to-end exploratory data analysis and statistical hypothesis testing project on insurance claims data, covering data cleaning, feature engineering, visual analysis, and formal statistical inference.

---

## 📁 Project Structure

```
Insurance-Claims-EDA/
│
├── Insurance_Claims.ipynb       # Main analysis notebook
├── claims.csv                   # Claims transaction data (1,100 records)
├── cust_demographics.csv        # Customer demographic data (1,085 records)
└── README.md
```

---

## 🎯 Objective

To perform a comprehensive analysis of insurance claims data by:

- Merging and auditing datasets for a unified 360° customer view
- Cleaning, transforming, and engineering meaningful features
- Deriving business insights through aggregation and visualization
- Validating key business hypotheses using statistical tests

---

## 📊 Dataset Overview

**claims.csv** — 1,100 rows, 10 columns

| Column | Description |
|---|---|
| `claim_id` | Unique identifier for each claim |
| `customer_id` | Customer identifier (can repeat across claims) |
| `claim_date` | Date the claim was filed |
| `claim_amount` | Amount claimed (raw string with `$` prefix) |
| `incident_cause` | Cause of the incident (Driver error, Crime, Natural causes, etc.) |
| `claim_area` | Area of claim — Auto or Home |
| `claim_type` | Type of claim (Injury only, etc.) |
| `police_report` | Whether a police report was filed (Yes / No / Unknown) |
| `total_policy_claims` | Total number of policy claims by the customer |
| `fraudulent` | Whether the claim was fraudulent (Yes / No) |

**cust_demographics.csv** — 1,085 rows, 6 columns

| Column | Description |
|---|---|
| `CUST_ID` | Customer identifier (join key) |
| `gender` | Customer gender |
| `DateOfBirth` | Date of birth |
| `State` | US state of residence |
| `Segment` | Customer market segment |
| `Contact` | Contact number |

---

## 🔧 Steps Performed

### 1. Data Import & Merging
- Imported both CSVs and joined on `customer_id` / `CUST_ID` using an inner merge to create a single unified dataframe (1,085 rows × 15 columns).

### 2. Data Audit & Type Corrections
- Identified and fixed datatype mismatches — `claim_date` and `DateOfBirth` converted to `datetime`, `Contact` cleaned of hyphens, `claim_amount` stripped of `$` and cast to `float`.

### 3. Alert Flag for Unreported Injury Claims
- Created a binary flag (`Alert_Flag = 1`) for all injury-only claims where no police report was filed.

### 4. Deduplication
- Identified customers with multiple claims and retained only the most recent claim record per customer based on `claim_date`.

### 5. Missing Value Imputation
- `claim_amount` → imputed with **mean**
- `total_policy_claims` → imputed with **mode**

### 6. Age Calculation & Categorization
- Computed age from `DateOfBirth` relative to the latest claim date.
- Categorized into:

| Group | Age Range |
|---|---|
| Children | < 18 |
| Youth | 18–30 |
| Adult | 30–60 |
| Senior | > 60 |

---

## 📈 Key Business Questions Answered

| # | Question |
|---|---|
| Q8 | Average claim amount by customer segment |
| Q9 | Total claim amount by incident cause for claims filed ≥ 20 days before Oct 1, 2018 |
| Q10 | Count of adults from TX, DE, AK who claimed for driver-related issues |
| Q11 | Pie chart — claim amount share by gender and segment |
| Q12 | Bar chart — driver-related claims by gender |
| Q13 | Bar chart — fraudulent claims by age group |
| Q14 | Line chart — monthly trend of total claim amounts (chronological order) |
| Q15 | Faceted bar chart — average claim amount by gender and age group (fraud vs. non-fraud) |

---

## 🧪 Hypothesis Testing

All tests conducted at **95% confidence interval (α = 0.05)**.

| # | Hypothesis | Test Used | Result |
|---|---|---|---|
| H1 | Similarity in claim amounts between males and females | Independent samples t-test | **Fail to reject H₀** — No significant difference |
| H2 | Relationship between age category and customer segment | Chi-square test of independence | **Fail to reject H₀** — No significant relationship |
| H3 | Current year claim amounts significantly greater than ₹10,000 (2016–17 avg) | One-sample t-test (one-tailed) | **Reject H₀** — Current year amounts are significantly higher |
| H4 | Difference in claim amounts between Adults and Youth | Independent samples t-test | **Fail to reject H₀** — No significant difference |
| H5 | Relationship between total policy claims and claimed amount | Pearson correlation | **Fail to reject H₀** — No significant linear relationship |

---

## 🛠️ Tech Stack

- **Python 3.x**
- **pandas** — data manipulation and aggregation
- **NumPy** — vectorized operations and conditional flags
- **Matplotlib / Seaborn** — visualizations
- **SciPy (scipy.stats)** — hypothesis testing (t-test, chi-square, Pearson correlation)
