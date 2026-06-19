# Sales-Promotions-Dashboard

### Dashboard Link : [[Add your Power BI Service report link here]](https://app.powerbi.com/groups/dfc7ff87-aad0-4ae5-9a97-2c713551f2b2/reports/ac095f29-69f3-45de-820f-84b91e58b766/af2d0f6290058ea94bd8?experience=power-bi)

## Problem Statement

This dashboard helps the business understand its sales and promotional performance across products, customers, and regions. It helps identify which products are top/bottom performers by sales, units sold, and profit, so the business can focus on what's working and investigate what isn't. It also tracks how promotions and discounts are affecting net sales, helping evaluate whether specific promotions are actually driving profitable revenue or just eating into margins through discounting.

Through the geographic map and city-level sales view, the business can also identify which regions are underperforming and may need more focused sales or marketing effort.

Since discount percentage and promotion details are tracked at the transaction level, the dashboard lets the business connect specific promotions to their actual impact on net sales and profit, rather than assuming promotions are working.

### Steps followed

- Step 1: Loaded data into Power BI Desktop. The data model follows a star schema with one **Fact Table** (transactions) connected to three dimension tables — **Dim Customers**, **Dim Product**, and **Dim Promotion** — plus two date tables.
- Step 2: Built relationships connecting the Fact Table to each dimension table (`CustomerID`, `Product ID`, `PromotionID`), and connected the Fact Table to both date tables on `Date (dd/mm/yyyy)`.
- Step 3: Since a single fact table can only have one **active** relationship to a given date table, **Date Table 1** was set as the active relationship and **Date Table 2** was added as a second, inactive relationship for alternate date-based analysis.
- Step 4: Created a separate **Measurement Table** to hold all DAX measures, keeping them organized and separate from the data tables.
- Step 5: Wrote DAX measures to calculate sales figures while overriding the default date filter context — using `USERELATIONSHIP` to activate Date Table 2 when needed, and `ALL('Date Table 1')` to remove the default filter from the active date table:

```dax
Sum of net sales =
CALCULATE(
    SUM('Fact Table'[Total sales]),
    ALL('Date Table 1'),
    USERELATIONSHIP('Date Table 2'[Date], 'Fact Table'[Date (dd/mm/yyyy)])
)

total profit =
CALCULATE(
    SUM('Fact Table'[Total sales]),
    ALL('Date Table 1'),
    USERELATIONSHIP('Date Table 2'[Date], 'Fact Table'[Date (dd/mm/yyyy)])
)
```

- Step 6: Built the **Overview** page with a sales trend line chart, a net sales vs. profit scatter chart, a discount % by promotion bar chart, a city-level sales map, and an order count card.
- Step 7: Built the **Top and Bottom** page using bar charts to show the Top 5 and Bottom 5 products by Net Sales, Units Sold, and Profit — six visuals in total, making it easy to instantly spot best and worst performers.
- Step 8: Built the **Sales & Profit** page with clustered column charts comparing Net Sales vs. Total Sales and Profit vs. Total Profit, with date slicers added for flexible period-based comparison.
- Step 9: Built the **Promotions** page with a full transaction-level table (Customer ID, Date, Product ID, Promotion ID, Discount %, Discount Value, Price per Unit, Units Sold, Net Sales, Total Sales, Profit) along with slicers for Date, Customer Name, Product Name, and Promotion Name, allowing detailed drill-down into individual promotions and customers.
- Step 10: The report was then published to Power BI Service.

## Data Model

**Fact Table** — `CustomerID`, `Date (dd/mm/yyyy)`, `Discount percentage`, `Discount value`, `Net sales`, `order id`, `Price per unit`, `Product ID`, `Profit`, `PromotionID`, `Total sales`, `Units Sold`

**Dim Customers** — `Customer ID`, `Customer Name`, `City`, `State`, `Pincode`, `EmailID`, `Phone Number`

**Dim Product** — `ProductID`, `Product Name`, `Product Line`, `Price per unit`

**Dim Promotion** — `PromotionID`, `Promotion Name`, `Ad Type`, `Coupon Code`, `percentage`, `Price Reduction Type`

**Date Table 1** — primary active date table

**Date Table 2** — secondary date table, connected via an inactive relationship, activated in specific measures via `USERELATIONSHIP`

**Measurement Table** — holds the model's DAX measures (`Sum of net sales`, `total profit`)

### Relationships
- `Fact Table[CustomerID]` → `Dim Customers[Customer ID]`
- `Fact Table[Product ID]` → `Dim Product[ProductID]`
- `Fact Table[PromotionID]` → `Dim Promotion[PromotionID]`
- `Fact Table[Date (dd/mm/yyyy)]` → `Date Table 1[Date]` *(active)*
- `Fact Table[Date (dd/mm/yyyy)]` → `Date Table 2[Date]` *(inactive)*

> **Note:** `total profit` currently sums `Total sales`, not `Profit` — worth double-checking this is intentional and not a copy-paste leftover from `Sum of net sales`.

# Report Pages / Snapshots

*(Add screenshots of each page here — Overview, Top and Bottom, Sales & Profit, Promotions)*

## Overview
![Overview](add-screenshot-link-here)

## Top and Bottom
![Top and Bottom](add-screenshot-link-here)

## Sales & Profit
![Sales & Profit](add-screenshot-link-here)

## Promotions
![Promotions](add-screenshot-link-here)

# Insights

A four-page report was created in Power BI Desktop and published to Power BI Service.

The following can be explored through the dashboard (fill in with your actual numbers once finalized):

### [1] Top/Bottom Performers
- Top and bottom 5 products by Net Sales, Units Sold, and Profit are surfaced on the **Top and Bottom** page, making it easy to identify best sellers and underperformers at a glance.

### [2] Sales Trends
- The **Overview** page tracks net sales over time, helping identify seasonal patterns or periods of growth/decline.

### [3] Geographic Performance
- The city-level map on the **Overview** page highlights which locations are generating the most total sales.

### [4] Promotions Impact
- The **Promotions** page allows filtering by individual promotions, products, and customers to evaluate how discounting is affecting net sales and profit on a transaction level.

### [5] Profitability vs. Sales
- The scatter chart (Net Sales vs. Profit) on the **Overview** page, and the clustered comparisons on the **Sales & Profit** page, help identify whether higher sales are translating into proportionally higher profit.

---

## Tools & Technologies
- Power BI Desktop
- Power Query
- DAX
- Power BI Service

## Getting Started
1. Clone this repository
2. Open `project_1.pbix` in [Power BI Desktop](https://powerbi.microsoft.com/desktop/)
3. If using a live data connection, update credentials under **Transform Data → Data Source Settings**
4. Refresh data and explore

## License
Add your preferred license here (e.g., MIT).
