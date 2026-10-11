
# Retail Banking Customer Churn & Retention Analytics
### An Applied Econometric & Business Intelligence Framework

[![R](https://img.shields.io/badge/R-4.x-blue.svg)](https://www.r-project.org/)
[![PowerBI](https://img.shields.io/badge/Power_BI-Desktop-F2C811.svg)](https://powerbi.microsoft.com/)
[![EViews](https://img.shields.io/badge/EViews-12%2F13-red.svg)](https://www.eviews.com/)
[![Excel](https://img.shields.io/badge/Microsoft_Excel-365-217346.svg)](https://www.microsoft.com/excel)

---

## 1. Project Context
Customer attrition (churn) represents a critical operational risk and revenue leakage channel for commercial banks. Acquiring new banking depositors incurs significantly higher customer acquisition costs (CAC) than retaining existing account holders. 

This project analyses a multinational retail banking dataset covering **10,000 retail banking clients** across three primary European operating regions (France, Germany, and Spain). Rather than using "black-box" machine learning algorithms, this repository applies **classical discrete choice econometrics (Binary Logit/Probit)** and **business intelligence analytics** to isolate the causal determinants of account closure, measure marginal attrition elasticities, and provide actionable customer retention strategies.

---

## 2. Data Dictionary
The underlying dataset (`Churn_Modelling.xlsx`) contains 10,000 customer records across 14 transactional, demographic, and behavioural attributes.

| Variable Name | Data Type | Classification | Description & Value Domain |
|---|---|---|---|
| `RowNumber` | Integer | System Index | Row identifier ($1$ to $10,000$). |
| `CustomerId` | Integer | Primary Key | Unique, non-sensitive client identifier ($15,565,701$ to $15,815,690$). |
| `Surname` | Text | Personal Identifier | Customer family name ($2,932$ unique surnames). |
| `CreditScore` | Integer | Continuous Metric | Bureau credit score ($350$ to $850$, $\mu = 650.53$). |
| `Geography` | Text | Nominal Categorical | Operating market: `France` ($50.1\%$), `Germany` ($25.1\%$), `Spain` ($24.8\%$). |
| `Gender` | Text | Binary Nominal | Client gender: `Female` ($45.4\%$), `Male` ($54.6\%$). |
| `Age` | Integer | Discrete Metric | Client age in years ($18$ to $92$, $\mu = 38.92$). |
| `Tenure` | Integer | Discrete Metric | Number of years the customer has banked with the institution ($0$ to $10$). |
| `Balance` | Numeric (Float) | Continuous Financial | Available account balance in EUR ($0.00$ to $250,898.09$, $\mu = 76,485.89$). |
| `NumOfProducts` | Integer | Discrete Metric | Number of distinct banking products held ($1$ to $4$). |
| `HasCrCard` | Binary (0/1) | Nominal Flag | Credit card ownership flag: `1` = Possesses credit card, `0` = Does not. |
| `IsActiveMember` | Binary (0/1) | Nominal Flag | Bank activity status flag: `1` = Active user, `0` = Inactive user. |
| `EstimatedSalary` | Numeric (Float) | Continuous Financial | Annual estimated customer salary in EUR ($11.58$ to $199,992.48$). |
| **`Exited`** | **Binary (0/1)** | **Dependent Variable** | **Customer churn status: `1` = Closed account, `0` = Retained.** |

---

## 3. Problem Statement & Research Objectives
* **Visualising Attrition Patterns:** Design an executive business intelligence interface mapping churn rate across geographical markets, product counts, and customer age bands.
* **Econometric Model Specification (No Machine Learning):** Formulate and optimise a **Binary Logit Discrete Choice Model** via Maximum Likelihood Estimation (MLE) in EViews/R to quantify the statistical significance and marginal effects ($dY/dX$) of each behavioural attribute.
* **Deriving Commercial Recommendations:** Translate empirical log-odds and elasticity coefficients into balance retention thresholds and product cross-selling policies.

---

## 4. Analytical Stack & Libraries
* **Microsoft Excel:** Initial data audit, structural verification, and preliminary schema inspection.
* **R Programming (`tidyr`, `dplyr`, `ggplot2`):**
  * `dplyr`: Relational data wrangling, group summaries, custom feature derivation, and outlier filtering.
  * `tidyr`: Reshaping tabular panels, pivoting wide-to-long structures for cross-tab distributions.
* **EViews (Econometric Modelling & Forecasting):**
  * Binary Dependent Variable estimation (`LOGIT` / `PROBIT`).
  * Maximum Likelihood covariance adjustments (Huber-White robust standard errors).
  * In-sample and out-of-sample conditional probability forecasting.
* **Microsoft Power BI & DAX:**
  * Relational star-schema data modeling.
  * Custom DAX measures for dynamic customer churn rates, lost balance exposure, and active member ratios.

---

## 5. Data Ingestion & Data Hygiene Audit
* **Dimensionality:** Exactly $10,000$ rows and $14$ columns.
* **Missing Value Analysis:** $0$ missing values detected across all columns.
* **Structural Characteristics:**
  * High proportion of zero-balance accounts in France ($2,418$ clients) and Spain ($1,199$ clients), whereas Germany exhibits zero clients with a null balance.
  * Uniform distribution observed in `EstimatedSalary`.
  * Overall baseline churn rate: **$20.37\%$** ($2,037$ churned vs $7,963$ retained).

---

## 6. Exploratory Data Analysis (EDA)
* **Univariate Distributions:**
  * `Age`: Right-skewed distribution centred at $37$ years.
  * `CreditScore`: Bell-shaped distribution centred at $650$.
  * `NumOfProducts`: $50.8\%$ hold $1$ product; $45.9\%$ hold $2$ products; only $3.3\%$ hold $\ge 3$ products.
* **Bivariate Associations:**
  * **Geography vs Churn:** Germany displays a disproportionate churn rate of **$32.44\%$**, compared to France ($16.15\%$) and Spain ($16.67\%$).
  * **Product Paradox:** Customers holding $2$ products exhibit the lowest churn (**$7.58\%$**), whereas churn escalates to **$27.71\%$** for $1$ product, **$82.71\%$** for $3$ products, and **$100.00\%$** for $4$ products.
  * **Age Cohorts:** Attrition accelerates sharply in the $41$--$50$ age bracket ($33.97\%$) and peaks in the $51$--$60$ bracket ($56.21\%$).
* **Correlation Analysis:**
  * Negative association between `IsActiveMember` and `Exited` ($r \approx -0.16$).
  * Strong positive correlation between `Age` and `Exited` ($r \approx +0.29$).

---

## 7. Data Pre-Processing Pipeline
* **Feature Pruning:** Removal of non-behavioural administrative identifiers (`RowNumber`, `CustomerId`, `Surname`) to eliminate noise and maintain data privacy.
* **Transformation & Categorical Encoding:**
  * Creation of dummy variables for categorical regressors (`Germany`, `Spain` using `France` as base reference; `Female` using `Male` as base reference).
  * Generation of balance indicator flag (`ZeroBalanceFlag` = $1$ if `Balance` $= 0$).
  * Age cohort segmentation (`18–30`, `31–40`, `41–50`, `51–60`, `60+`) for non-linear econometric diagnostics.

---

## 8. Analytical Architecture & Workflow Diagram

```text
  [ Raw Excel Ingestion ] 
             │
             ▼
  [ R Data Pre-processing (tidyr / dplyr) ]
   ├── Drop PII (CustomerId, Surname)
   ├── Create Regional & Activity Dummy Variables
   └── Export Cleaned Matrix
             │
   ┌─────────┴────────────────────────┐
   ▼                                  ▼
[ EViews Econometric Pipeline ]     [ Power BI / DAX Semantic Layer ]
 ├── Binary Logit / Probit MLE       ├── Relational Star-Schema
 ├── Marginal Effects (Odds Ratios)  ├── Core KPI Measures
 └── Conditional Forecast Probs      └── Interactive Executive View
             │                                  │
             └───────────────┬──────────────────┘
                             ▼
              [ Business Strategy & Insights ]
