# PBI
Row context: applies when a formula is evaluated row-by-row (calculated columns, iterators like SUMX). You can reference the current row's columns directly.
Filter context: the set of filters that apply to a calculation (slicer selections, visual filters, CALCULATE adjustments). Measures run in filter context.
Calculated column vs Measure:
Calculated column: stored per row in a table, evaluated during data refresh; has row context.
Measure: dynamic aggregation evaluated in visuals, depends on filter context; recommended for aggregations.
CALCULATE: changes filter context for an expression. Fundamental for measures that apply conditional filters.
FILTER: returns a table of rows that meet a condition — often used inside CALCULATE or iterators.
ALL / ALLEXCEPT / VALUES / DISTINCT: control which filters are applied or removed.
RELATED / RELATEDTABLE: traverse relationships to bring related columns or tables into scope.
VAR / RETURN: define intermediate variables inside measures for clarity and performance.
Common basic examples (use these as measures or calculated columns depending on context)

Simple aggregation (measure) Total Sales = SUM(Sales[SalesAmount])

Using SUMX (row-level calculation across a table) Total Revenue = SUMX(Sales, Sales[Quantity] * Sales[UnitPrice])

Calculated column example (concatenate names) Full Name = Customers[FirstName] & " " & Customers[LastName]

Use RELATED in a calculated column on Sales to get product category Product Category = RELATED(Product[Category])

CALCULATE with a direct filter Sales West = CALCULATE([Total Sales], Geography[Region] = "West")

CALCULATE with FILTER (useful for complex conditions) Large Orders Sales = CALCULATE( [Total Sales], FILTER(Sales, Sales[SalesAmount] > 1000) )

Remove filters with ALL Total Sales (All Products) = CALCULATE([Total Sales], ALL(Product))

Time intelligence Sales YTD = TOTALYTD([Total Sales], 'Date'[Date]) Sales LY = CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date'[Date]))

Use VAR to simplify and avoid repeated work Average Price = VAR Rev = SUM(Sales[SalesAmount]) VAR Qty = SUM(Sales[Quantity]) RETURN DIVIDE(Rev, Qty)

Safe division Profit Margin % = DIVIDE([Gross Profit], [Total Sales], 0)

Distinct counts and per-customer metrics Customers Count = DISTINCTCOUNT(Customer[CustomerKey]) Sales per Customer = DIVIDE([Total Sales], [Customers Count])

Basic DAX Query (returns a table — used in DAX Studio / SSMS) EVALUATE TOPN( 10, SUMMARIZE( Sales, Customer[CustomerKey], "TotalSales", SUM(Sales[SalesAmount]) ), [TotalSales], DESC )
