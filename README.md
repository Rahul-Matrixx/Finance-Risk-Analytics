# Finance Risk Analytics

### End-to-End Data Analytics Project | Power Query | Power BI | DAX

---

## Dashboard Preview

<img src="Finance%20Risk%20Analytics%20-%20Executive%20Summary.png" width="600">

---

<img src="Finance%20Risk%20Analytics%20-%20Risk%20Analysis.png" width="600">

---

<img src="Finance%20Risk%20Analytics%20-%20Loan%20Details.png" width="600">

---

## Project Overview

A complete Finance Risk Analytics pipeline that transforms raw quarterly loan data into an interactive Power BI dashboard. The project simulates a real-world banking analytics use case where loan portfolio data is cleaned, transformed, analyzed, and visualized to identify financial risk patterns and generate business insights.

**Key Finding:** Bad Loan Rate = **37.5%**

---

## Tech Stack

| Tool                         | Usage                          |
| ---------------------------- | ------------------------------ |
| **Power Query (M Language)** | ETL — Extract, Transform, Load |
| **Power BI Desktop**         | Dashboard & Visualization      |
| **DAX**                      | KPI Measures & Calculations    |
| **Excel / CSV**              | Data Source                    |

---

## Project Architecture

```text
Raw CSV Files (Q1, Q2, Q3)
        ↓
Phase 1: Data Cleaning (Power Query)
        ↓
Phase 2: Data Transformation (10 New Columns)
        ↓
Phase 3: Analysis Preparation (5 Summary Tables + 22 DAX Measures)
        ↓
Phase 4: Power BI Dashboard (3 Pages)
```

---

## Phase 1 — Data Cleaning

### What was done

* Appended 3 quarterly CSV files into one Master Table
* Handled missing values using average imputation for selected numerical fields
* Cleaned interest-rate values and standardized decimal formats
* Applied `en-IN` date formatting for `Issue_Date`
* Standardized text fields using `Text.Proper()`
* Removed hidden spaces using `Text.Trim()` and `Text.Clean()`
* Prepared the dataset for downstream analysis and visualization

---

## Phase 2 — Data Transformation

### 10 New Columns Added

| Column               | Logic                      | Type    |
| -------------------- | -------------------------- | ------- |
| Quarter              | Month-based Q1/Q2/Q3/Q4    | Text    |
| Month_Name           | `Date.MonthName`           | Text    |
| Month_Number         | `Date.Month`               | Int64   |
| Year                 | `Date.Year`                | Int64   |
| Loan_Age_Days        | Duration from Issue_Date   | Int64   |
| Risk_Category        | Credit Score bands         | Text    |
| Is_Bad_Loan          | Defaulted or NPA indicator | Text    |
| Loan_Amount_In_Lakhs | Amount / 100000            | Decimal |
| Risk_Score           | Numeric 1–4 for sorting    | Int64   |
| EMI                  | Standard EMI calculation   | Decimal |

### EMI Calculation

```m
each
  let
    P = [Loan_Amount],
    r = [Interest_Rate_%] / 100 / 12,
    n = [Tenure_Months],
    EMI = Number.Round(
      P * r * Number.Power(1 + r, n) /
      (Number.Power(1 + r, n) - 1),
      2
    )
  in
    EMI
```

---

## Phase 3 — Analysis Preparation

### 5 Aggregation Tables

* `Region_Summary`
* `LoanType_Summary`
* `Quarter_Summary`
* `Risk_Summary`
* `Status_Summary`

### 22 DAX Measures

The dashboard uses DAX measures with functions such as `COALESCE`, `DIVIDE`, `CALCULATE`, `FILTER`, and `COUNTROWS`.

### Example Measures

```dax
Bad_Loan_Rate_% =
ROUND(
    DIVIDE(
        [Bad_Loan_Count],
        [Total_Loans],
        0
    ) * 100,
    2
)
```

```dax
NPA_Amount =
COALESCE(
    CALCULATE(
        SUM(Row_Data[Loan_Amount]),
        Row_Data[Status] = "NPA"
    ),
    0
)
```

```dax
Recovery_Rate_% =
ROUND(
    DIVIDE(
        COALESCE(
            COUNTROWS(
                FILTER(
                    Row_Data,
                    Row_Data[Status] = "Closed"
                )
            ),
            0
        ),
        [Total_Loans],
        0
    ) * 100,
    2
)
```

---

## Phase 4 — Power BI Dashboard

The Power BI solution contains **3 analytical pages**.

### Page 1 — Executive Summary

* KPI Cards for Total Portfolio
* Total Loans
* Bad Loan Rate %
* Average Credit Score
* Recovery Rate %
* Donut Chart for Loan Type Distribution
* Bar Chart for Region-wise Portfolio
* Line Chart for Quarterly Trends
* Risk monitoring visualization

### Page 2 — Risk Analysis

* NPA Amount
* Defaulted Amount
* Very High Risk Count
* Risk Category Distribution
* Loan Status Distribution
* Bad Loans by Region
* Risk-focused portfolio analysis

### Page 3 — Loan Details

* Detailed loan-level data
* Conditional formatting
* Top High-Risk Customers
* Quarterly loan amount comparison
* Growth analysis across quarters

---

## Key Insights

* The analyzed portfolio shows a **37.5% Bad Loan Rate**
* North region has the highest portfolio exposure
* Regional analysis highlights differences in bad-loan concentration
* Personal Loans show comparatively higher default exposure
* Quarterly analysis shows Q2 as the peak disbursement period
* Credit-score-based categorization helps identify higher-risk customer segments
* Recovery metrics provide visibility into repayment performance

---

## My Contribution

Contributed to the development of the financial risk analytics solution, including

* Worked on data preparation and transformation using **Power Query**
* Contributed to **Power BI dashboard development**
* Worked with **DAX measures and financial KPIs**
* Contributed to loan portfolio and risk analysis
* Worked on regional, loan-type, and customer-level analysis
* Supported the transformation of raw quarterly loan data into an interactive analytics dashboard

---

## Project Structure

```text
Finance-Risk-Analytics/
│
├── Finance Risk Analytic Project.pbix
│
├── Finance Risk Analytics/
│   ├── Data/
│   │   ├── Loan_Data_Q1 (Jan–Mar 2024).csv
│   │   ├── Loan_Data_Q2 (Apr–Jun 2024).csv
│   │   └── Loan_Data_Q3 (Jul–Sep 2024).csv
│   │
│   └── Output/
│       ├── Finance Risk Analytics - Executive Summary.png
│       ├── Finance Risk Analytics - Loan Details.png
│       └── Finance Risk Analytics - Risk Analysis.png
│
├── Finance Risk Analytics - Executive Summary.png
├── Finance Risk Analytics - Loan Details.png
├── Finance Risk Analytics - Risk Analysis.png
├── Images/
│   └── dashboard.png
│
└── README.md
```

---

## Skills Demonstrated

* ETL Pipeline Design
* Power Query M Language
* Data Cleaning & Transformation
* Missing-Value Handling
* DAX Measures
* Financial KPI Analysis
* Power BI Dashboard Development
* Data Modeling
* Risk Analysis
* Loan Portfolio Analysis
* NPA Analysis
* Bad Loan Analysis
* Recovery Rate Analysis
* Regional & Loan-Type Analysis
* Conditional Formatting
* Interactive Data Visualization

---

## How to Run

1. Clone the repository

```bash
git clone https://github.com/Rahul-Matrixx/Finance-Risk-Analytics.git
```

2. Open the Power BI file

```text
Finance Risk Analytic Project.pbix
```

3. If required, update the local data source path in Power Query

```text
Transform Data → Data Source Settings
```

4. Refresh the dataset.

5. Explore the three dashboard pages and apply the available filters.

---

## Future Enhancements

* Integrate SQL as an additional data source
* Add Python-based exploratory data analysis
* Implement predictive credit-risk modeling
* Add anomaly detection
* Introduce automated risk alerts
* Connect the dashboard to Power BI Service
* Enable automated data refresh

---

## Author

**Rahul-Matrixx**

[GitHub](https://github.com/Rahul-Matrixx/Finance-Risk-Analytics)

---

*Portfolio project demonstrating end-to-end Data Analytics, financial risk analysis, ETL, DAX, data modeling, and Power BI dashboard development.*
