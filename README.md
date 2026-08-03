# 🛡️ Global Shield Insurance - Portfolio Performance & Claim Analytics

An end-to-end interactive Power BI dashboard built for **Global Shield Insurance** to evaluate portfolio profitability, analyze loss ratios across policy lines, and identify claim processing bottlenecks.

## 📌 Executive Summary & Business Problem

Insurance providers collect premiums while disbursing payouts for legitimate customer claims. Managing profitability requires tight tracking of financial performance alongside operational speed.

**Core Business Challenges Solved:**

1. **Executive Oversight:** Leadership lacked a unified view of net loss ratios, incoming premiums, and payouts across policy types (Auto, Health, Property).
2. **Operational Bottlenecks:** Operations managers struggled to isolate claims taking longer than 10 days to settle, leading to processing backlogs and customer dissatisfaction.

## 📊 Dashboard Overview

The solution consists of a **two-page interactive Power BI dashboard**:

### Page 1: Executive Overview

Designed for C-suite leadership to evaluate financial risk and portfolio health.

* **KPI Scorecards:** Total Premiums, Total Claims Paid, Loss Ratio %, Avg Settlement Days.
* **Monthly Financial Trends:** Visualized incoming premium vs. outgoing claim payouts over time.
* **Policy Loss Ratio Breakdown:** Identified high-risk policy categories (e.g., Health vs. Property).

### Page 2: Operations & Bottlenecks

Designed for Operations Managers to streamline claim turnaround times.

* **Settlement Speed Tiers:** Categorized claims into Fast ($\le 10$ days), Delayed ($> 10$ days), and Pending Review.
* **Bottleneck Analysis:** Highlighted policy lines with the highest average handling days.
* **Claim Audit Table:** Detailed granular view for auditing specific delayed claim records.

## 🏗️ Data Architecture & Star Schema Model

The raw data was modeled into a **Star Schema** to ensure optimal performance, dynamic filtering, and strict reporting integrity.

* **Fact Tables:** `Fact_claims`, `Facts_policies`
* **Dimension Tables:** `Dim_customer`, `Dim_agent`, `Dim_Date`
* **Relationships:** One-to-Many ($1:*$) configured with single-direction filtering via primary keys (`CustomerID`, `PolicyID`, `AgentID`, `DateKey`).


## 📐 Key DAX Measures & Calculated Columns

### 1. Explicit DAX Measures (Stored in `_Measures` Table)

Total Premium = SUM(Facts_policies[PremiumAmount])

Total Claims Paid = SUM(Fact_claims[ClaimAmount])

Loss Ratio % = DIVIDE([Total Claims Paid], [Total Premium], 0)

Avg Settlement Days = AVERAGE(Fact_claims[SettlementDays])

Settlement Rate % = DIVIDE(COUNTROWS(FILTER(Fact_claims, Fact_claims[SettlementDays] > 0)), COUNTROWS(Fact_claims), 0)

### 2. Calculated Columns (Row-Level Categorization)

SettlementDays = DATEDIFF(Fact_claims[ClaimDate], Fact_claims[SettlementDate], DAY)

SettlementSpeed = IF(ISBLANK(Fact_claims[SettlementDate]), "Pending Review", IF(Fact_claims[SettlementDays] <= 10, "Fast Settlement (<=10 Days)", "Delayed Settlement (>10 Days)"))


## 💡 Key Business Insights

1. **High Loss Ratio Warning:** Certain policy lines exhibit higher loss ratios, highlighting areas where underwriting guidelines should be reviewed.
2. **Operational Efficiency:** Most claims settle within the 10-day target, but delayed claims ($>10$ days) concentrate in complex policy categories.
3. **Turnaround Benchmark:** Tracks turnaround timelines across all portfolios to help management reduce settlement lag.


## 🛠️ Tools & Technologies Used

* **Business Intelligence:** Power BI Desktop
* **Data Transformation & Modeling:** Power Query, Star Schema Architecture
* **Analytics & Calculations:** Data Analysis Expressions (DAX)
* **Version Control & Portfolio:** Git & GitHub
