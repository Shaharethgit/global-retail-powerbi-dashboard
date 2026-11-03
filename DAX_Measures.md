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

### AOV

```DAX
AOV = 
    DIVIDE(SUM(Sales[SalesAmount]), 
    DISTINCTCOUNT(Sales[OrderID]))
```
### Profit margine

```DAX
Profit margine = 
       DIVIDE(Sales[Total Profit],
        Sales[Total Sales])
```

## 2. Page 2 – Products and Discounts

### Average discount %
```DAX
AVERAGE DISCOUNT % = 
    DIVIDE([GROSS SALES] - [Total Sales],
     [GROSS SALES])
```

### DISCOUNT BAND

```DAX
Discount band = SWITCH(
    TRUE(),
    Sales[Discount] = 0, "0%",
    Sales[Discount] <= 0.10, "UNDER 10%",
    Sales[Discount] <= 0.20, "11 - 20%",
    "ABOVE 20%"    
)
```
### Profit margine (CATEGORY)
```DAX
P/Margine = if(ISINSCOPE(Products[Category]),
     [Profit margine])
```

## 2. Page 3 – Stores and Targets

### Month start

```DAX
MONTHSTART = DATE(YEAR(DateTable[Date]),
     MONTH(DateTable[Date]),1)
```

### Target Sale

```DAX
TARGET_SALE = 
    CALCULATE(
        SUM(Targets[TargetSales]),
        TREATAS(
            VALUES(DateTable[Date]),
            Targets[Month]
        )
    )
```

### Achievement %
```DAX
ACHIEVEMENT % = 
    DIVIDE([Total Sales],
     [TARGET_SALE])
```




## 2. Page 4 – Customers and Returns

### CUSTOMER RANK

```DAX
CUSTOMER RANK = 
    IF(ISINSCOPE(Customers[CustomerID]),RANKX(
        ALL(Customers[CustomerID],Customers[CustomerName]),
        [Total Sales],,
        DESC,
        Dense       
    ))
```



