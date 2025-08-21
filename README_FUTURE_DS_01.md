# FUTURE_DS_01 — Business Sales Dashboard (E‑Commerce)

This repository contains my Task 1 submission for the **Future Interns – Data Science & Analytics** internship.

## 🎯 Objective
Build an interactive dashboard to analyse:
- Best‑selling products
- Monthly/weekly sales trends
- High‑revenue categories
- Profitability and discount impact

## 📁 Files
- `ecommerce_sales_task1.csv` — sample dataset (generated)
- `screenshots/` — exported images of Power BI report (add after building)
- `FUTURE_DS_01.pbix` — Power BI file (add after building)
- `insights.md` — short business insights (add after building)

## 🧹 Data Dictionary
- `Order_ID`: Unique ID for each order or return (RET* = return)
- `Order_Date`: Date of order
- `Product`: Product name
- `Category`, `Subcategory`: Product grouping
- `Customer_ID`: Pseudonymous customer ID
- `Region`, `City`: Geography
- `Channel`: Online / Mobile App / Store
- `Unit_Price`: List price
- `Quantity`: Units; negative for returns
- `Discount`: Fraction of list price discounted (0–0.4)
- `Sales`: Realised revenue (post discount); negative for returns
- `COGS`: Cost of goods sold
- `Profit`: Sales − COGS
- `Payment_Method`: UPI, Card, etc.

## 🛠️ How to Reproduce in Power BI
1. **Get Data → Text/CSV** and pick `ecommerce_sales_task1.csv`
2. In **Power Query**: ensure data types (Date/Decimal/Whole), remove obvious errors, keep negative rows (they are returns)
3. Create a **Date table** (DAX below) and mark it as date table
4. Build measures (DAX below) and visuals (see `insights.md` template)
5. Export screenshots and push everything to GitHub

### 📅 Date table (DAX)
```DAX
Date = 
ADDCOLUMNS (
    CALENDAR ( DATE(2024,1,1), DATE(2025,12,31) ),
    "Year", YEAR([Date]),
    "MonthNum", MONTH([Date]),
    "Month", FORMAT([Date], "MMM"),
    "YearMonth", FORMAT([Date], "YYYY-MMM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "DayName", FORMAT([Date], "DDD")
)
```

### 📐 Core Measures (DAX)
```DAX
Total Sales = SUM('Sales'[Sales])
Total Quantity = SUM('Sales'[Quantity])
Total Profit = SUM('Sales'[Profit])
AOV = DIVIDE([Total Sales], DISTINCTCOUNT('Sales'[Order_ID]))
Return Rate = DIVIDE(ABS(CALCULATE([Total Sales], FILTER('Sales', 'Sales'[Sales] < 0))), ABS([Total Sales]))
Sales LY = CALCULATE([Total Sales], DATEADD('Date'[Date], -1, YEAR))
YoY % = DIVIDE([Total Sales] - [Sales LY], [Sales LY])
```
> Replace `'Sales'` with your table name if different.

## 🧾 License
Sample data generated for educational use.
