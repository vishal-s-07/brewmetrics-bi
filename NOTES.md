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