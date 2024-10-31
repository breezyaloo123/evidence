---
title: SUPERMARKET ANALYSIS
---

## Tableau de bord



```sql categories
  select
      category
  from needful_things.orders
  group by category
```

```sql orders
select 
    count(invoice_id) AS orders
from SalesData.Sales

```

```sql gross_profit
select 
    (sum(total) - sum(cogs)) as gross_profit
from SalesData.Sales

```

```sql revenue
select 
   sum(total) as revenue
from SalesData.Sales

```
```sql cogs
select 
   sum(cogs) as cogs
from SalesData.Sales
```

```sql monthly_orders_revenue
select 
   date_trunc('month', cast(date as date)) as month,
   count(*) as orders,
   sum(total) as revenue,
   product_line
from SalesData.Sales
where product_line like '${inputs.product.value}'
group by all
```
<!-- and date between '${inputs.date_range_from_query.start}' and '${inputs.date_range_from_query.end}' -->
```sql products
select 
    product_line
from SalesData.Sales
```
```sql Orderdate
  select date as orderdate
  from SalesData.Sales
```

```sql payment
  select payment, sum(total) as revenue,
  count(invoice_id) as orders
  from SalesData.Sales
   group by all
```

```sql orders_revenue_prd_ctg
with aov as (
  select 
   date_trunc('month', cast(date as date)) as month,
   count(*) as orders,
   sum(total) as revenue
from SalesData.Sales
group by 1
)
select *, (revenue/orders) as AOV from aov
```

```sql gender_revenue
  select gender, sum(total) as revenue
  from SalesData.Sales
   group by all
```
```sql gender_order_revenue
  select gender, 
  count(invoice_id) as orders,
  sum(total) as revenue
  from SalesData.Sales
   group by all
```
```sql prd_revenue
  select
  sum(total) as revenue,
  product_line as product
  from SalesData.Sales
   group by all
```
```sql avg_ratung
  select
  avg(rating) as rating,
  product_line as product
  from SalesData.Sales
   group by all
```
**<BigValue 
  data={orders} 
  value=orders
  title="# of Orders"
  fmt="num"
/>**
**<BigValue 
  data={revenue} 
  value=revenue
  title="Revenue"
  fmt="usd"
/>**
**<BigValue 
  data={cogs} 
  value=cogs
  title="COGS"
  fmt="usd"
/>**
**<BigValue 
  data={gross_profit} 
  value=gross_profit
  title="Gross Profit"
  fmt="usd"
/>**

<Dropdown data={products} name=product value=product_line title="Select a Product"
/> 
<!-- <DateRange
    name=date_range_from_query
    start=2019-01-01
    end=2019-12-31
    title="Select a Date Range"
/> -->


<BarChart
  data={monthly_orders_revenue}
  x=month
  y=revenue
  yFmt=usd0k
  labels=true
  labelSize=14
  downloadableImage=true
  title="Monthly Revenue by {inputs.product.value}"
/>
<BarChart
  data={prd_revenue}
  x=product
  y=revenue
  swapXY=true
  yFmt=usd0k
  labels=true
  downloadableImage=true
  labelSize=14
  title="Revenue by Product Category"
/>
<BarChart
  data={avg_ratung}
  x=product
  y=rating
  swapXY=true
  labels=true
  downloadableImage=true
  labelSize=14
  title="Average Rating by Product Category"
/>

<BarChart
  data={monthly_orders_revenue}
  x=month
  y=orders
  yFmt=num
  labels=true
  downloadableImage=true
  labelSize=14
  title="Monthly Orders by {inputs.product.value} Category"
/>

<BarChart
  data={orders_revenue_prd_ctg}
  x=month
  y=AOV
  yFmt=usd
  labels=true
  labelSize=14
  downloadableImage=true
  title="Monthly Average Order Value"
/>


<BarChart
  data={payment}
  x=payment
  y=revenue
  y2=orders
  yFmt=usd
  y2Fmt=num
  yAxisTitle=false
  downloadableImage=true
  y2AxisTitle=false
  labels=true
  labelSize=14
  title="Revenue, Orders by Wallet"
/>
<BarChart
  data={gender_order_revenue}
  x=gender
  y=revenue
  yFmt=usd
  yAxisTitle=false
  y2AxisTitle=false
  y2=orders
  labels=true
  downloadableImage=true
  labelSize=14
  title="Revenue, Orders by Gender"
/>






## Insights

- **Food and Beverages** are the top performing category,contributing significantly to total revenue.
- **Cash Wallet** is the most type of payment method used by customers with a total revenue of $112K
- Most of the orders are done by **females** (501) which produce a total of $167K
- **Food and Beverages** has the highest rating (7,11), followed by Fashion accessories(7,03) then Health and Beauty (7,00)

## Actions

- Invest in Marketing and promotions for **Food and Beverages** to further increase revenue.


## Follow me
- On Linkedin for further projects