# Copilot-Assisted DAX Development

## 1. MoM Sales Growth %

### Copilot suggestion

Copilot generated the following DAX measure:

```DAX
MoM Sales Growth % =
VAR CurrentSales =
    SUM(Fact_Sales[sales_amount])
VAR PreviousMonthSales =
    CALCULATE(
        SUM(Fact_Sales[sales_amount]),
        DATEADD(Dim_Date[Date], -1, MONTH)
    )
RETURN
    DIVIDE(
        CurrentSales - PreviousMonthSales,
        PreviousMonthSales
    )


## 2. Running Total Sales

### Copilot suggestion

Copilot generated a running total measure using `CALCULATE`, `FILTER`, `ALL(Dim_Date[Date])`, and `Fact_Sales[sales_amount]`.

### Measure

```DAX
Running Total Sales =
CALCULATE(
    SUM(Fact_Sales[sales_amount]),
    FILTER(
        ALL(Dim_Date[Date]),
        Dim_Date[Date] <= MAX(Dim_Date[Date])
    )
)


## 3. Item Sales Rank

### Copilot suggestion

Copilot generated a RANKX measure using `ALL(Dim_Product[item])` and total sales from `Fact_Sales[sales_amount]`.

### Measure

```DAX
Item Sales Rank =
RANKX(
    ALL(Dim_Product[item]),
    CALCULATE(SUM(Fact_Sales[sales_amount])),
    ,
    DESC,
    DENSE
)

## 4. Average Transaction Value

### Copilot suggestion

Copilot generated a custom measure that calculates total sales divided by the distinct number of sales transactions. The `DIVIDE` function safely handles empty or zero-transaction cases.

### Measure

```DAX
Average Transaction Value =
DIVIDE(
    SUM(Fact_Sales[sales_amount]),
    DISTINCTCOUNT(Fact_Sales[sale_id])
)