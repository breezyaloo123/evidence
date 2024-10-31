---
title: Customers
---

<Details title='Insights about Customers Analysis'>
</Details>


```sql sales_data
select 
    *
from SalesData.Sales

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

```sql products
select 
    product_line
from SalesData.Sales
```

```sql Orders_by_products
select 
   date_trunc('month', cast(date as date)) as month,
   count(*) as orders,
   sum(total) as revenue
from SalesData.Sales
group by 1
```

**<BigValue 
  data={orders} 
  value=orders
  title="Num orders"
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

```sql orders_revenue_prd_ctg
with aov as (
  select 
   product_line as Product_Category,
   count(*) as orders,
   sum(total) as revenue
from SalesData.Sales
group by 1
)
select *, (revenue/orders) as AOV from aov
```
<Dropdown data={products} name=product value=product_line title="Select a Product"
/>

<BarChart
  data={monthly_orders_revenue}
  x=month
  y=revenue
  yFmt=usd0k
  labels=true
  labelSize=14
  title="Monthly Revenue"
/>

<BarChart
  data={monthly_orders_revenue}
  x=month
  y=orders
  yFmt=num
  labels=true
  labelSize=14
  title="Monthly Orders"
/>

<DataTable data={orders_revenue_prd_ctg} rowNumbers=true sortable=true>
  <Column id=Product_Category title="Product Category"/>
  <Column id=revenue contentType=colorscale align=center fmt=usd0k/>
  <Column id=orders contentType=colorscale scaleColor=#e3af05 align=center/>
  <Column id=AOV contentType=colorscale scaleColor=#e3af05 align=center fmt=usd/>
</DataTable>

<!-- 
The number of orders is **<Value data={orders} color="green" fmt="num"/>**

Total revenue is **<Value data={sales_data} column="total" fmt=usd agg="sum" color="green"/>**

Total COGS is **<Value data={sales_data} column="cogs" fmt=usd agg="sum" color="blue"/>**

GROSS PROFIT is **<Value data={gross_profit} fmt=usd color="green" redNegatives="true"/>** -->