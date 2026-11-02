# DAX Measures & Data Model

All measures, calculated columns and Power Query steps used in [`SupplyChain.pbix`](../SupplyChain.pbix).

## Data model

| Table | Purpose |
|---|---|
| `ataCoSupplyChainDataset` | Fact table: one row per order line (180,519 rows, 2015–2018) |
| `Date_Table` | Calendar table (2015-01-01 to 2018-12-31) with Year, Month, Months, Quarter and Weekday columns, used by the Year slicer and the previous-year measures |
| `Parameter` | Field parameter that lets the user switch the Financial Performance charts between **Gross Revenue** and **Profit** |

## Power Query (data preparation)

1. Load `DataCoSupplyChainDataset.csv` and promote headers.
2. Set data types for all 53 columns (whole numbers, decimals, text, date/time).
3. **Remove sensitive columns:** `Customer Email` and `Customer Password`.
4. Convert `order date (DateOrders)` from date/time to date.
5. **Remove columns not needed for analysis:** `Order Zipcode` (86% empty), `Product Description` (100% empty), `Product Image` (web links).
6. Filter out rows with a blank `Sales` or `Sales per customer`.
7. Reorder columns and sort by order date.

## Calculated columns

```dax
shipping_delay_days =
    ataCoSupplyChainDataset[Days for shipping (real)]
    - ataCoSupplyChainDataset[Days for shipment (scheduled)]
```

```dax
shipping Flag =
    IF ( ataCoSupplyChainDataset[shipping_delay_days] > 0, "Late",
    IF ( ataCoSupplyChainDataset[shipping_delay_days] = 0, "On Time",
    IF ( ataCoSupplyChainDataset[shipping_delay_days] < 0, "Early" ) ) )
```

Date table columns: `Year = YEAR([Date])`, `Month = MONTH([Date])`, `Months = FORMAT([Date], "MMM")`, `Quarter = "Q" & FORMAT([Date], "Q")`, `Weekday = FORMAT([Date], "dddd")`.

## Measures

### Revenue & profit

```dax
Gross_Revenue =
    SUM(ataCoSupplyChainDataset[Sales])
```

```dax
Net_Revenue =
    SUM(ataCoSupplyChainDataset[Sales per customer])
```

```dax
Profit =
    SUM(ataCoSupplyChainDataset[Benefit per order])
```

```dax
Total Loss =
    ABS(
        CALCULATE(
            SUM('ataCoSupplyChainDataset'[Benefit per order]),
            'ataCoSupplyChainDataset'[Benefit per order] < 0
        )
    )
```

```dax
Ttoal_Discount =
    SUM(ataCoSupplyChainDataset[Order Item Discount])
```

```dax
Avg_Discount_Rate =
    AVERAGE(ataCoSupplyChainDataset[Order Item Discount Rate])
```

```dax
Avg_ProfitRatio =
    AVERAGE(ataCoSupplyChainDataset[Order Item Profit Ratio])
```

```dax
AVG_Order_profit_perorder =
    AVERAGE(ataCoSupplyChainDataset[Order Profit Per Order])
```

### Orders

```dax
Total_Orders =
    COUNTROWS(ataCoSupplyChainDataset)
```

```dax
Complete_Order_% =
    DIVIDE( CALCULATE(COUNTROWS(ataCoSupplyChainDataset),ataCoSupplyChainDataset[Order Status] = "COMPLETE"),
                            COUNTROWS(ataCoSupplyChainDataset),0)
```

```dax
Cancle_Order_% =
    DIVIDE( CALCULATE(COUNTROWS(ataCoSupplyChainDataset),ataCoSupplyChainDataset[Order Status] = "CANCELED"),
                            COUNTROWS(ataCoSupplyChainDataset),0)
```

```dax
% of Orders by Status =
    DIVIDE(
        COUNTROWS('ataCoSupplyChainDataset'),
        CALCULATE(
            COUNTROWS('ataCoSupplyChainDataset'),
            ALLEXCEPT('ataCoSupplyChainDataset', 'ataCoSupplyChainDataset'[Order Item Quantity])
        )
    )
```

### Shipping

```dax
Late_Percentage =
    DIVIDE(
        CALCULATE(
            COUNTROWS('ataCoSupplyChainDataset'),
            'ataCoSupplyChainDataset'[Shipping Flag] = "Late"
        ),
        COUNTROWS('ataCoSupplyChainDataset'),
        0
    )
```

```dax
On_Time Percentage =
    DIVIDE( CALCULATE(COUNTROWS(ataCoSupplyChainDataset),ataCoSupplyChainDataset[shipping Flag] = "On Time"),
                            COUNTROWS(ataCoSupplyChainDataset),0)
```

```dax
LateTotalOrders =
    CALCULATE(COUNTROWS(ataCoSupplyChainDataset),ataCoSupplyChainDataset[shipping Flag] = "Late")
```

```dax
LateAverage_Day =
    ABS(CALCULATE( AVERAGE(ataCoSupplyChainDataset[shipping_delay_days]), 'ataCoSupplyChainDataset'[shipping Flag] = "Late"))
```

```dax
AVG_daysShipping =
    AVERAGE(ataCoSupplyChainDataset[Days for shipping (real)])
```

```dax
SameDay =
    CALCULATE(COUNTROWS(ataCoSupplyChainDataset),ataCoSupplyChainDataset[Shipping Mode] = "Same Day")
```

```dax
Avg_Delivery_Risk =
    AVERAGE(ataCoSupplyChainDataset[Late_delivery_risk])
```

```dax
Avg_DeliveryRisk =
    AVERAGE(ataCoSupplyChainDataset[Late_delivery_risk])
```

### Previous-year comparisons (time intelligence)

```dax
Last_Year_TotalOrders =
    VAR _CY = [Total_Orders]
    VAR _LY = 
    CALCULATE([Total_Orders],
              DATEADD(Date_Table[Date],-1,YEAR)
              )
    RETURN 
        _LY
```

```dax
Total_Orders_Last_Year =
    CALCULATE(
        [Total_Orders],
        PREVIOUSYEAR('Date_Table'[Date])
    )
```

```dax
Last_Year_Gross_Revenue =
    VAR _LY = 
    CALCULATE([Gross_Revenue],
              DATEADD(Date_Table[Date],-1,YEAR)
              )
    RETURN 
        _LY
```

```dax
Last_Year_NetRevenue =
    VAR _LY = 
    CALCULATE([Net_Revenue],
              DATEADD(Date_Table[Date],-1,YEAR)
              )
    RETURN 
        _LY
```

```dax
Last_Year_TotalDiscount =
    VAR _LY = 
    CALCULATE([Ttoal_Discount],
              DATEADD(Date_Table[Date],-1,YEAR)
              )
    RETURN 
        _LY
```

```dax
Last_Year_TotalProfit =
    VAR _LY = 
    CALCULATE([Profit],
              DATEADD(Date_Table[Date],-1,YEAR)
              )
    RETURN 
        _LY
```

```dax
Last_Year_CancleOrder% =
    CALCULATE(
        [Cancle_Order_%],
        PREVIOUSYEAR('Date_Table'[Date])
    )
```

```dax
Last_Year_Avg_Profit per Order =
    CALCULATE(
        [AVG_Order_profit_perorder],
        PREVIOUSYEAR('Date_Table'[Date])
    )
```

```dax
Last_Year_DiscountRate =
    CALCULATE(
        [Avg_Discount_Rate],
        PREVIOUSYEAR('Date_Table'[Date])
    )
```

```dax
Last_Year_Complete Order =
    CALCULATE(
        [Complete_Order_%],
        PREVIOUSYEAR('Date_Table'[Date])
    )
```

```dax
Last_Year_Late_Shipping =
    CALCULATE(
        [Late_Percentage],
        DATEADD('Date_Table'[Date], -1, YEAR)
    )
```

```dax
Last_Year_OnTime =
    CALCULATE(
        [On_Time Percentage],
        DATEADD('Date_Table'[Date], -1, YEAR)
    )
```

```dax
Last_Year_AvgDaysShipping =
    CALCULATE(
        [AVG_daysShipping],
       SAMEPERIODLASTYEAR('Date_Table'[Date])
    )
```

```dax
Last_Year_SameDay =
    CALCULATE(
        [SameDay],
        SAMEPERIODLASTYEAR('Date_Table'[Date])
    )
```

```dax
Last_Year_Avg_Late_Day =
    CALCULATE(
        [LateAverage_Day],
        PREVIOUSYEAR('Date_Table'[Date])
    )
```

### Formatting & utility

```dax
BG Color Rule =
    VAR CurrentValue = 
        IF(
            ISINSCOPE('ataCoSupplyChainDataset'[Product Name]),
            [Total_Orders],  -- product level
            CALCULATE([Total_Orders], ALLEXCEPT('ataCoSupplyChainDataset', 'ataCoSupplyChainDataset'[Category Name]))  -- category level
        )
    RETURN
        "rgba(239, 68, 68, " & MIN(CurrentValue / 4000, 1) & ")"
```

```dax
Last Refresh =
    "Last Refreshed: " & FORMAT(NOW(), "DD MMM YYYY, HH:MM")
```
