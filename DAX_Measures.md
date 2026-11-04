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

### Discount band

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

### Customer Rank

```DAX
CUSTOMER RANK = 
    IF(ISINSCOPE(Customers[CustomerID]),RANKX(
        ALL(Customers[CustomerID],Customers[CustomerName]),
        [Total Sales],,
        DESC,
        Dense       
    ))
```

### Rank Bar

```DAX
RANK BAR = 
    IF(ISINSCOPE(Customers[CustomerID]),SWITCH(
        TRUE(),
        Customers[CUSTOMER RANK] = 1, 10,
        Customers[CUSTOMER RANK] = 2, 9,
        Customers[CUSTOMER RANK] = 3, 8,
        Customers[CUSTOMER RANK] = 4, 7,
        Customers[CUSTOMER RANK] = 5, 6,
        Customers[CUSTOMER RANK] = 6, 5,
        Customers[CUSTOMER RANK] = 7, 4,
        Customers[CUSTOMER RANK] = 8, 3,
        Customers[CUSTOMER RANK] = 9, 2,
        1
    ))
```

### Return Reason %

```DAX
RETURN REASON % = 
    DIVIDE(
        [RETURN_AMOUNT],
        CALCULATE(
            [Total Sales], ALL(Returns[ReturnReason])
        )
    )
```

### Online Share %

```DAX
ONLINE SHARE % = 
    DIVIDE([Total Sales],
        CALCULATE([Total Sales],
        ALL(Sales[Channel])
                )
    )
```

### Return Rate % 

```DAX
RETURN_RATE % = 
    DIVIDE([RETURN_AMOUNT], 
    [Total Sales])
```