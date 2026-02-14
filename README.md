# Customer Churn Analysis — Power BI Dashboard

## Dashboard Preview (Overview Page)

![Overview dashboard page](screenshots/overview.png)

If you want to explore the rest of the report pages, check the `screenshots/` folder for all dashboard views.

---

## 1. Project Overview

This project analyzes customer churn behavior using a single dataset (`Databel - Data`).  
The objective is to identify the main drivers of churn and provide actionable business insights.

- Total Customers: 6,687  
- Churned Customers: 1,796  
- Overall Churn Rate: 26.86%  

The report focuses on contract type, demographics, service usage, charges, and customer behavior patterns.

---

## 2. Dataset

Source: DataCamp  
Table Used: `Databel - Data`  

No Power Query transformations were applied.  
The dataset was used as provided.

The table contains customer-level information such as:
- Contract type
- Monthly charges
- Customer service calls
- Data consumption
- International calls
- Demographics
- Churn label and churn reason

---

## 3. DAX Measures

### Core Measures

```DAX
Number of Customers =
COUNT('Databel - Data'[Customer ID])

Number of Unique Customers =
DISTINCTCOUNT('Databel - Data'[Customer ID])

Churned =
IF('Databel - Data'[Churn Label] = "Yes", 1, 0)

Number of Churned Customers =
SUM('Databel - Data'[Churned])

Churn Rate =
DIVIDE([Number of Churned Customers], [Number of Customers])
```

### Averages & Charges

```DAX
Avg Customer Service Calls =
AVERAGE('Databel - Data'[Customer Service Calls])

Avg Extra Data Charges =
DIVIDE(
    SUM('Databel - Data'[Extra Data Charges]),
    [Number of Customers]
)

Avg Extra International Charges =
DIVIDE(
    SUM('Databel - Data'[Extra International Charges]),
    [Number of Customers]
)
```

### Calculated Columns

```DAX
Contract Category =
SWITCH(
    'Databel - Data'[Contract Type],
    "One Year", "Yearly",
    "Two Year", "Yearly",
    "Monthly"
)

Demographics =
IF(
    'Databel - Data'[Senior] = "Yes",
    "Senior",
    IF('Databel - Data'[Under 30] = "Yes", "Under 30", "Other")
)

Grouped Consumption =
IF(
    'Databel - Data'[Avg Monthly GB Download] < 5,
    "Less than 5 GB",
    IF(
        'Databel - Data'[Avg Monthly GB Download] < 10,
        "Between 5 and 10 GB",
        "10 or more GB"
    )
)
```

---

## 4. Dashboard Pages

### Overview
- Total customers and churn rate
- Churn reasons distribution
- Customers by contract type
- Churn rate by state

### Churn Demographics
- Churn rate by age groups
- Churn rate by senior status
- Gender comparison
- Churn distribution by demographic segment

### Groups and Categories
- Churn by grouped consumption
- Monthly charge vs churn
- Customer group segmentation

### Unlimited Plan
- Churn rate by unlimited data plan
- Consumption comparison
- Plan impact analysis

### International Calls
- Churn rate by international activity
- International charges comparison

### Contract Type
- Churn rate by contract type
- Account length vs churn
- Contract duration impact

### Payment and Contract
- Payment method vs churn
- Contract category comparison
- Account length relationship

### Extra Charges
- Extra data charges vs churn
- Extra international charges vs churn

---

## 5. Key Business Insights

1. Contract type is the strongest churn driver.
   - Monthly contracts: 46.29% churn rate
   - Yearly contracts: 6.62% churn rate

2. Churn decreases significantly as account length increases.
   - New customers show churn above 40–50%
   - Long-term customers drop below 10%

3. Customers with lower data usage (<5 GB) show higher churn.

4. Seniors show higher churn rates compared to other groups.

5. Higher customer service call frequency is correlated with churn.

6. Main churn reasons:
   - Competitor offers (largest share)
   - Attitude of support
   - Price dissatisfaction

---

## 6. What This Project Demonstrates

- End-to-end Power BI dashboard creation
- KPI design and churn rate calculation
- Customer segmentation logic using DAX
- Business storytelling with visual analytics
- Ability to extract actionable insights from a single-table dataset

---

## 7. Repository Structure

```
Customer-Churn-PowerBI/
│
├── README.md
├── dashboard/
│   └── Analyzing customer churn.pbix
├── screenshots/
└── measures.md
```
