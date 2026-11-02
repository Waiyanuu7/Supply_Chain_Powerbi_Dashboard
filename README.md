# Supply Chain Performance Dashboard (Power BI)

An interactive Power BI dashboard analysing **180,519 order lines** from DataCo, a global sports and outdoor retailer (2015–2018). It covers financial results, order status, shipping performance and late deliveries, with a drill-through into the category with the largest losses.

> **Headline finding:** 57% of 2017 order lines, carrying **$6.73M of revenue**, were delivered late. The cause isn't regional or product-related. The late rate is about 57% in every region and department. The cause is **delivery promises that don't match reality**: First Class promises 1 day but always takes 2 (100% late), and Second Class promises 2 days but is no faster than Standard Class (79% late).

![Financial Performance](images/01_financial_performance.png)

## Business questions

1. How much revenue and profit does the business make, and where does it come from (market, category, segment, shipping mode)?
2. How healthy is the order pipeline: how many orders complete, cancel, or are flagged for fraud?
3. How reliable is delivery, and what actually causes late shipments?
4. Why does the Fishing category have the largest losses, and is it a problem?

## Dashboard pages

| Page | What it shows |
|---|---|
| **Financial Performance** | Gross revenue, profit, discounts and net revenue; revenue by category, market, shipping mode and customer segment; monthly revenue vs discount rate; total loss by category. A field parameter switches the charts between Revenue and Profit. |
| **Order Performance** | Order lines, completion and cancellation rates, average profit per order, discount rate; orders by status, region, category and segment; cancellation rate by category |
| **Shipping Performance** | Late %, on-time %, average shipping days, Same Day volume; early / on-time / late split by shipping mode, department, market and order size; a **decomposition tree** to explore late % by shipping mode, category and region |
| **Late Shipping** | Late orders and revenue by region, shipping mode, month, department, and category → product (with conditional colour formatting) |
| **Fishing Category Loss Detail** | Drill-through page: profit vs loss, loss by market and region, loss vs discount rate, monthly orders and discount rate |
| **Key Findings** | Written summary of insights and recommended actions |

Every page has a **Year slicer**, page navigation buttons, and previous-year comparisons on the KPI cards.

| | |
|---|---|
| ![Order Performance](images/02_order_performance.png) | ![Shipping Performance](images/03_shipping_performance.png) |
| ![Late Shipping](images/04_late_shipping.png) | ![Fishing Loss Detail](images/05_fishing_loss_detail.png) |

## Key findings (2017)

| Metric | Value |
|---|---|
| Gross revenue | $11.81M |
| Profit | $1.30M (11.0% margin) |
| Order lines / orders | 53,196 / 21,866 |
| Late delivery rate | 57.2% |
| Revenue on late deliveries | $6.73M (57%) |

1. **Lateness is driven by shipping mode alone.** The late rate is 55–58% across every region, department and customer segment. By shipping mode it ranges from **39.7% (Standard Class) to 100% (First Class)**.
2. **First Class is late every time.** It promises 1 day and always takes 2, so customers paying for speed ($1.79M of revenue) never get what they paid for.
3. **Second Class is as slow as Standard Class** (about 4 days on average) but promises 2 days, so 79% of its lines are late.
4. **The Q4 2017 "drop" in late orders isn't an improvement.** From October each order has one line instead of three, so row counts fall. The number of orders actually rises, and the late rate stays at 56–58%.
5. **Fishing is the most profitable category, not a loss-maker.** It makes $216K net profit (11.4% margin). Its $191K total loss is large only because it's the highest-revenue category. Its share of loss-making lines (18.2%) matches the company-wide rate (18.5%), and discounting doesn't drive the losses.
6. **LATAM and Europe produce 88% of revenue.** Consumers are 52% of revenue.
7. **All suspected-fraud orders are paid by bank transfer.** That's 506 orders ($285K) in 2017.

📄 **Full analysis, evidence tables and recommendations:** [docs/key_findings.md](docs/key_findings.md)

## Recommendations

1. Change the First Class promise to 2 days, or find a carrier that can deliver in 1 day.
2. Make Second Class genuinely faster than Standard Class, or remove it.
3. Make Standard Class delivery times more predictable (40% early, 40% late).
4. Roll out shipping fixes by revenue (Central America and Western Europe first), since every region has the same late rate.
5. Review the pricing or costing rule behind the 18.5% of loss-making lines company-wide, rather than targeting one product.
6. Add extra checks to bank-transfer payments.

## Data preparation & modelling

- **Power Query:** set data types; removed sensitive columns (customer email and password); removed empty or unused columns (product description, product image, order zipcode); filtered out rows with blank sales.
- **Calculated columns:** `shipping_delay_days` (actual − scheduled days) and `shipping Flag` (Early / On Time / Late).
- **Date table** linked to order date, for the Year slicer and time intelligence.
- **37 DAX measures**, including KPIs, `CALCULATE` filters, `DIVIDE`, `ALLEXCEPT` for percent-of-total, previous-year comparisons (`DATEADD`, `PREVIOUSYEAR`, `SAMEPERIODLASTYEAR`), and a conditional-formatting measure.

📄 **All measures and steps:** [docs/dax_measures.md](docs/dax_measures.md)

## Tools & skills

Power BI Desktop · Power Query · DAX · data modelling · time intelligence · field parameters · drill-through · decomposition tree · conditional formatting · data storytelling

## Dataset

**DataCo Smart Supply Chain for Big Data Analysis**: 180,519 rows × 53 columns of orders, customers, products and shipping, January 2015 – January 2018.

- Source: [Mendeley Data](https://data.mendeley.com/datasets/8gx2fvg2k6/5) · [Kaggle](https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis)
- Column descriptions: [data/DescriptionDataCoSupplyChain.csv](data/DescriptionDataCoSupplyChain.csv)
- The raw CSV (96 MB) isn't stored in this repository because it's too large. Download it from the links above.

## How to open

1. Install [Power BI Desktop](https://www.microsoft.com/power-bi/desktop) (free, Windows).
2. Download `DataCoSupplyChainDataset.csv` from the source above.
3. Open `SupplyChain.pbix`. If asked, go to **Transform data → Data source settings** and point the source to your CSV.

## Repository structure

```
├── README.md
├── SupplyChain.pbix                      # Power BI dashboard
├── data/
│   └── DescriptionDataCoSupplyChain.csv  # column descriptions
├── docs/
│   ├── key_findings.md                   # full analysis & recommendations
│   └── dax_measures.md                   # data model, Power Query, DAX
└── images/                               # dashboard screenshots
```
