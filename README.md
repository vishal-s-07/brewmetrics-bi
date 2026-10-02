# BrewMetrics BI

Business Intelligence solution for BrewMetrics Coffee Co. The project models coffee shop sales from `brewmetrics_sales.csv` in a Power BI semantic model and presents sales trends, cumulative sales, item performance, and transaction value on a single report page.

## Data Model

The model uses `Fact_Sales` as the transactional fact table and three dimensions:

- **Fact_Sales**: One row per sale, with `sale_id`, `date`, `city`, `store_format`, `category`, `item`, `quantity`, `unit_price`, and `sales_amount`.
- **Dim_Date**: A date dimension containing `Date`, `Year`, `Month Name`, and `Month`. `Month Name` is sorted by `Month`.
- **Dim_City**: A distinct list of cities from the sales data.
- **Dim_Product**: A distinct item list with `item`, `category`, and `unit_price`.

Relationships connect `Fact_Sales` to `Dim_Date` by date, to `Dim_City` by city, and to `Dim_Product` by item.

## DAX Measures

### MoM Sales Growth %

```DAX
MoM Sales Growth % =
VAR CurrentSales = SUM(Fact_Sales[sales_amount])
VAR PreviousMonthSales =
	CALCULATE(
		SUM(Fact_Sales[sales_amount]),
		DATEADD(Dim_Date[Date], -1, MONTH)
	)
RETURN
	DIVIDE(CurrentSales - PreviousMonthSales, PreviousMonthSales)
```

Calculates the percentage change in sales compared with the previous month.

### Running Total Sales

```DAX
Running Total Sales =
CALCULATE(
	SUM(Fact_Sales[sales_amount]),
	FILTER(
		ALL(Dim_Date[Date]),
		Dim_Date[Date] <= MAX(Dim_Date[Date])
	)
)
```

Calculates cumulative sales through the current date context.

### Item Sales Rank

```DAX
Item Sales Rank =
RANKX(
	ALL(Dim_Product[item]),
	CALCULATE(SUM(Fact_Sales[sales_amount])),
	,
	DESC,
	DENSE
)
```

Ranks items by total sales in descending order, with tied items sharing a rank without gaps.

### Average Transaction Value

```DAX
Average Transaction Value =
DIVIDE(
	SUM(Fact_Sales[sales_amount]),
	DISTINCTCOUNT(Fact_Sales[sale_id])
)
```

Calculates total sales divided by the distinct number of sales transactions.

## Dashboard

The Power BI report provides interactive views for analyzing BrewMetrics sales performance.

The dashboard includes:

- A line chart showing the Cold Brew seasonal sales trend across the available dates.
- A card showing Average Transaction Value.
- A bar chart comparing total sales performance across cities.
- A chart showing sales by Store Format.
- A City slicer for interactively filtering the dashboard.
- A City → Store Format drill-down hierarchy for exploring sales at different levels.

## Dashboard Insights

Based on the dashboard:

- Cold Brew shows stronger sales activity during April and May compared with the later period shown in the dashboard.
- Bengaluru has the highest total sales among the cities displayed, while Coimbatore has the lowest.
- The Store Format analysis shows different sales contributions from Flagship, Drive-Thru, and Kiosk formats.
