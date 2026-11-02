# Key Findings: DataCo Supply Chain (2017)

**Dashboard:** [`SupplyChain.pbix`](../SupplyChain.pbix)
**Period analysed:** calendar year 2017, the most recent full year in the data (selected with the Year slicer).

| 2017 at a glance | |
|---|---|
| Order lines | 53,196 |
| Orders | 21,866 |
| Gross revenue | $11.81M |
| Profit | $1.30M (11.0% margin) |
| Discounts given | $1.20M (average rate 10.2%) |
| Net sales (after discount) | $10.61M |
| Lines delivered late | **30,436 (57.2%)** |
| Revenue on late lines | **$6.73M (57.0%)** |

> **A note on "orders".** The dataset has one row per **order line** (product within an order). The dashboard's `Total_Orders` measure counts rows (`COUNTROWS`), so it counts order lines. 2017 has 53,196 lines across 21,866 distinct orders. Percentages such as late rate are the same either way, but counts should be read as order lines.

---

## 1. Financial performance

- **Two markets carry the business:** LATAM ($5.21M) and Europe ($5.20M) together produce **88% of revenue**. Pacific Asia adds $1.13M; Africa and USCA are very small in 2017.
- **Consumers are the largest segment** at 51.7% of revenue, followed by Corporate (30.6%) and Home Office (17.7%).
- **Fishing is the top category** at $1.90M. It's a single product, a $399.98 gun safe, so its high price drives the revenue. Cleats ($1.20M) and Camping & Hiking ($1.16M) follow.
- **Standard Class carries 60.4% of revenue**, then Second Class (19.2%), First Class (15.2%) and Same Day (5.1%).

## 2. Order performance

| Order status | Share of lines |
|---|---|
| Complete | 32.6% |
| Pending payment | 22.0% |
| Processing | 12.3% |
| Pending | 11.7% |
| Closed | 10.7% |
| On hold | 5.2% |
| Suspected fraud | 2.4% |
| Cancelled | 2.0% |
| Payment review | 1.1% |

- Only a third of lines are fully complete. Over a fifth are **waiting for payment**, which is a cash-flow risk worth tracking.
- Cancellation is low overall (2.0%). By category it ranges from 0% to 4.4%.
- **Every suspected-fraud order was paid by bank transfer.** In 2017, 506 orders ($285K of revenue) were flagged, and all used the TRANSFER payment type. The same is true across all years (4,062 of 4,062 lines). Transfers are the only payment method that triggers fraud flags, so they're the place to focus fraud checks.

## 3. Late delivery

### What is happening?

**57.2% of order lines (30,436 of 53,196) were delivered late**, and those lines carry **$6.73M, or 57% of 2017 revenue**. Late lines took 4.08 days to ship on average, arriving 1.61 days after the promised date. Both figures are flat compared with 2016 (4.09 days and 1.62 days), so nothing has improved year on year. Late delivery isn't an isolated issue. It's how most orders are delivered.

### Where is the problem? The shipping mode, not the region or product

Late order counts are highest in **Central America (8,245)** and **Western Europe (7,219)**, and in the **Fan Shop (10,699)** and **Apparel (8,277)** departments. But these are simply the largest regions and departments. **The late rate is almost identical everywhere:**

| Breakdown | Late rate range |
|---|---|
| Order regions (top 8 by volume) | 54.9% – 58.4% |
| Departments | 56.1% – 58.4% |
| Customer segments | 56.6% – 58.3% |
| Markets (excluding USCA, 118 lines) | 52.5% – 57.8% |

Central America and Western Europe account for 50.8% of late lines because they account for 50.3% of all lines. They are no worse than anywhere else.

**The one factor that changes the late rate is shipping mode:**

| Shipping mode | Promised days | Actual days | Early | On time | Late | Late rate |
|---|---|---|---|---|---|---|
| First Class | 1 | **always 2** | 0 | 0 | 8,021 | **100%** |
| Second Class | 2 | 2 – 6 (avg 3.98) | 0 | 2,147 | 8,209 | **79.3%** |
| Same Day | 0 | 0 – 1 | 0 | 1,347 | 1,548 | 53.5% |
| Standard Class | 4 | 2 – 6 (avg 3.99) | 12,795 | 6,471 | 12,658 | 39.7% |

Three things stand out:

1. **First Class is late 100% of the time.** It promises 1 day but always takes exactly 2. Its average delay is the smallest (1 day), so the "average days late" measure alone hides the problem: these customers pay for speed and never get it.
2. **Second Class is no faster than Standard Class.** Both take 2–6 days (average about 4), but Second Class promises 2 days, so 79% of its lines are late. Customers pay extra for a service that performs the same as the cheapest option.
3. **Standard Class has the best on-time record only because its promise (4 days) matches its average.** It's still unpredictable: 40% early and 40% late.

Late delivery is therefore a **delivery-promise problem**: the promised times for the faster modes don't match what the carriers actually achieve. It isn't a regional, warehouse or product problem.

### Monthly pattern: the Q4 "drop" isn't real

Late order lines fall from about 3,000 a month (January–September) to about 1,200 a month (October–December). **This isn't an improvement.** From October 2017 the data changes structure: orders go from about 3 lines each to 1 line each, so there are fewer rows. The number of **orders** actually rises in Q4 (about 2,100 a month vs about 1,750), and the **late rate stays at 56–58% every month**.

| Month (2017) | Order lines | Orders | Lines per order | Late rate |
|---|---|---|---|---|
| Jan – Sep (monthly avg) | ~5,200 | ~1,730 | ~3.0 | 56.0% – 58.6% |
| Oct | 2,255 | 2,101 | 1.07 | 56.1% |
| Nov | 2,055 | 2,055 | 1.00 | 57.8% |
| Dec | 2,124 | 2,124 | 1.00 | 58.1% |

## 4. Fishing category: loss drill-through

### What is happening?

Fishing contains a single product (Field & Stream Sportsman 16 Gun Fire Safe, $399.98). In 2017:

| | Amount |
|---|---|
| Profit from profitable lines | $407.2K |
| Loss from loss-making lines (865 of 4,751 lines) | **-$190.9K** |
| **Net profit** | **$216.3K** |
| Profit margin | 11.4% |

The dashboard's `Profit` measure is net profit ($216.3K, 53%), and `Total Loss` is the loss from loss-making lines ($190.9K, 47%). **Fishing is the most profitable category in the business**, and it also has the largest total loss. Both are true because it's the highest-revenue category.

### Is Fishing worse than other categories?

No. **18.2% of Fishing lines lose money, almost exactly the company-wide rate of 18.5%.** The share of loss-making lines is also flat across every other cut of the data:

| Breakdown | Loss-making line rate |
|---|---|
| Markets | 14.4% (USCA) – 18.8% |
| Shipping modes | 17.8% – 19.2% |
| Delivery (early / on time / late) | 18.0% – 18.9% |
| Customer segments | 18.2% – 18.8% |

### Where is the loss?

LATAM ($108.0K) and Europe ($72.0K) produce **94% of Fishing's loss**, because they also produce **95% of its sales** (4,511 of 4,751 lines). The loss follows volume, not a market-specific problem.

### Is discounting the cause?

No. Each discount level from 0% to 25% has the same number of lines (264), and the share of loss-making lines stays between 15% and 25% with **no upward trend** as discounts grow. Deeper discounts don't create more losses.

### What about October?

Only **17 Fishing lines** were sold in October 2017, and none in November or December. The product disappears when the product range changes in Q4 2017 (see the monthly pattern above). The October "discount spike" (10.4% vs about 10.2%) is an average of 17 lines, which is too few to mean anything.

## 5. Recommended actions

1. **Fix the First Class promise first.** It's late 100% of the time by exactly one day. Either change the promise to 2 days or find a carrier that can deliver in 1 day. This affects $1.79M of revenue from customers who pay for faster shipping.
2. **Rethink Second Class.** It's as slow as Standard Class but promises 2 days. Either make it genuinely faster, or drop it and offer Standard Class with an accurate promise.
3. **Make Standard Class more predictable.** It's 40% early and 40% late, so delivery times vary widely. Setting expectations by distance or carrier would cut the late rate without changing speed.
4. **Don't target specific regions or departments for lateness.** The late rate is the same everywhere (about 57%). Use regions only to prioritise roll-out by revenue: Central America and Western Europe first, because they're half the volume.
5. **Investigate loss-making lines company-wide, not Fishing alone.** About 18.5% of all lines lose money at the same rate in every category, market and discount level. That points to a pricing or costing rule that applies everywhere rather than to any single product.
6. **Add extra checks to bank-transfer payments.** All suspected fraud comes from transfers.
7. **Track payment collection.** 22% of lines are pending payment.

## 6. Notes on the data

- The data covers January 2015 – January 2018. 2018 has only one month, so 2017 is used as the latest full year.
- From October 2017 the data changes: each order has a single line, and the product range is different (Video Games, Children's Clothing, Cameras and other new categories). Compare Q4 2017 with earlier months by **order count or late rate**, not by row count.
- Revenue (`Sales`) is before discount; net sales (`Sales per customer`) is after discount. Profit is `Benefit per order`, which is recorded per order line.

**Data source:** [DataCo Smart Supply Chain for Big Data Analysis](https://data.mendeley.com/datasets/8gx2fvg2k6/5) (Mendeley Data), also on [Kaggle](https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis).
