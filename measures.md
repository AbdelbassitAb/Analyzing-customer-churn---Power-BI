# DAX Documentation — Calculated Columns & Measures (Customer Churn)

This file lists all **calculated columns** and **measures** used in the Customer Churn Power BI report.

Table: `Databel - Data` (single-table model)

---

## 1) Calculated Columns

### Churned
Binary churn flag used for counting churned customers.
```DAX
Churned =
IF('Databel - Data'[Churn Label] = "Yes", 1, 0)
```

### Contract Category
Simplifies contract types into broader categories.
```DAX
Contract Category =
SWITCH(
    'Databel - Data'[Contract Type],
    "One Year", "Yearly",
    "Two Year", "Yearly",
    "Monthly"
)
```

### Demographics
Creates a simplified demographic segmentation.
```DAX
Demographics =
IF(
    'Databel - Data'[Senior] = "Yes",
    "Senior",
    IF('Databel - Data'[Under 30] = "Yes", "Under 30", "Other")
)
```

### Grouped Consumption
Bins data consumption into three categories based on average monthly GB download.
```DAX
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

## 2) Measures

### Number of Customers
Counts customer records.
```DAX
Number of Customers =
COUNT('Databel - Data'[Customer ID])
```

### Number of Unique Customers
Counts unique customers.
```DAX
Number of Unique Customers =
DISTINCTCOUNT('Databel - Data'[Customer ID])
```

### Number of Churned Customers
Counts churned customers using the `Churned` flag.
```DAX
Number of Churned Customers =
SUM('Databel - Data'[Churned])
```

### Churn Rate
Churned customers divided by total customers.
```DAX
Churn Rate =
DIVIDE([Number of Churned Customers], [Number of Customers])
```

### Avg Customer Service Calls
Average number of customer service calls.
```DAX
Avg Customer Service Calls =
AVERAGE('Databel - Data'[Customer Service Calls])
```

### Avg Extra Data Charges
Average extra data charges per customer.
```DAX
Avg Extra Data Charges =
DIVIDE(
    SUM('Databel - Data'[Extra Data Charges]),
    [Number of Customers]
)
```

### Avg Extra International Charges
Average extra international charges per customer.
```DAX
Avg Extra International Charges =
DIVIDE(
    SUM('Databel - Data'[Extra International Charges]),
    [Number of Customers]
)
```

---

## Notes
- The report uses a single table, so all measures depend on the filter context from visuals and slicers.
- `DIVIDE()` is used to avoid division-by-zero errors.
