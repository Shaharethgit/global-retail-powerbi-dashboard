# DAX Measures

## 1. Base Measures

```DAX
Total Sales =
SUM(Sales[SalesAmount])

Total Profit =
SUM(Sales[Profit])

Orders =
DISTINCTCOUNT(Sales[OrderID])
```

## 2. Page 1 – Executive Overview

### Year-over-Year Analysis

```DAX
Total Sales YoY % =
VAR Sales_LY =
    CALCULATE(
        [Total Sales],
        SAMEPERIODLASTYEAR(DateTable[Date])
    )
RETURN
    DIVIDE([Total Sales] - Sales_LY, Sales_LY)
```