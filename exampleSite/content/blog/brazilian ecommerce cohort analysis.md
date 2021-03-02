---
title: "Cohort Analysis of Brazilian Ecommerce with pandas"
date: 2021-03-02T22:00:00+06:00
draft: false

# post thumb
image: "..static/images/post/202103-cohort-analysis-brazilian-ecommerce/brazilian-ecommerce-cohort-analysis.png"

# meta description
description: "Cohort Analysis of Brazilian Ecommerce with pandas in Python"

# taxonomies
categories:
  - "Data Science"
  - "Python"
tags:
  - "Python"
  - "Plotly"
  - "pyplot"
  - "pandas"
  - "pivot table"
  - "Cohort Analysis"
# post type
type: "featured"
---

I was only acquainted with cohort analysis but thanks to Renat Alimbekov. Renat is a Deep Learning Scientist with a [telegram channel](https://t.me/renat_alimbekov) and a [standalone blog]('https://alimbekov.com'), as well as a mentor at Yandex.Praktikum. I had a different mentor during my studies but I've been following Renat's telegram channel and noticed that he started giving typical data science assignments. So I accepted this challenge and started learning. You can find his solution [here](https://alimbekov.com/cohort-analysis-python/)

We're given a dataset of Brazilian Ecommerce, the two tables to merge (olist_orders_dataset.csv and olist_order_payments_dataset.csv), and two questions:
```
order_payment = orders.merge(payments, how='inner', on='order_id')
```

1. How many orders on average, and how much do all cohorts make in the first year?
2. Compare any of the two cohorts by revenue and number of orders

It was late night last Thursday, I gave it a try before going to sleep. When I was solving it, the words "in the first year" made me jump over to the next question cause it was time to sleep. I decided to see what I would get in the second question, solved it in a different way (from what one will see below, also I first used a different cohort column) and tagged Renat in a tweet. He gave me some feedback so I felt encouraged to get back to the task. In one of the following nights I wrote a few ideas of how to approach the first question just before falling asleep (see the time in the screenshot).

[image](..static\images\post\202103-cohort-analysis-brazilian-ecommerce\night-ideas.jpg)

The column we use to define cohorts:

```
cohort_column = 'order_purchase_timestam'
```

So I wanted to find the gap between the first and last order, then select the orders made within 365 days after the first order was made. I saved the first and last orders:

```
order_payment[cohort_column] = pd.to_datetime(order_payment[cohort_column])
first_orders = order_payment.pivot_table(index='customer_id', values=cohort_column, aggfunc={cohort_column:'first'}).reset_index()
last_orders = order_payment.pivot_table(index='customer_id', values=cohort_column, aggfunc={cohort_column:'last'}).reset_index()
first_orders.columns = ['customer_id', 'first_order']
last_orders.columns = ['customer_id', 'last_order']
```

Let's now calculate the difference between the first order and the last order.

``day_diff = first_orders.merge(last_orders.set_index('customer_id'), on='customer_id')
day_diff['day_diff'] = ((day_diff['last_order'] - day_diff['first_order']).astype("timedelta64[D]")).astype('int')
day_diff.head(3)``

[image](..static\images\post\202103-cohort-analysis-brazilian-ecommerce\day-difference-first-last-order-table.png)

Okay, the gap has been calculated.

```
print('Number of customers whose latest order was made in other days after the first order is', (day_diff['day_diff'] > 0).sum())
```
```
Number of customers whose latest order was made in other days after the first order is 0
```

However this is weird. Since none of these so-called next orders was made at a different point in time, I believe those were other items in a cart that were registered as different orders. We see that if we just drop the payment details and calculate duplicates (i.e. multiple orders made by one customer), the time the purchase was made in remains the same for the same customers:

```
same_time_customer_orders = order_payment.drop(['order_id','payment_sequential','payment_type','payment_installments', 'payment_value'], axis=1).duplicated(keep=False).sum()
print("Total number of multiple orders made by the same customers at one time in time for each customer is", same_time_customer_orders)
```
```Total number of multiple orders made by the same customers at one time in time for each customer is 7407
```

One would expect that a customer is coming back but considering the previous discovery, I'd assume that these were one-time purchases of multiple items registered as multiple orders.

Since the purchase time is never different that makes it simpler and there's no need to select orders made within 365 days after the first order of every customer. I can now move on to the cohort analysis questions set by Renat. There's a total of 99441 orders.

- How many orders on average, and how much do all cohorts make in the first year?

```
len(orders)
```
```
99441
```

I'm creating a pivot table to gather information by customer and order_status with the columns that I need for analysis and order_status, will explain the reason why I need 'order_status' column in a step.

```orders = order_payment.pivot_table(index=['customer_id', 'order_status'], values=[cohort_column, 'payment_value', 'order_id'], aggfunc={cohort_column:'min', 'payment_value': 'sum', 'order_id': 'count'}).reset_index()
```
```
orders.columns = ['customer_id', 'order_status', 'order_count', 'order_date', 'payment_value']
orders['order_year'] = orders['order_date'].dt.year
orders['order_month'] = orders['order_date'].dt.month
```

```
orders['order_status'].unique().tolist()
```
```
['delivered',
 'unavailable',
 'processing',
 'shipped',
 'canceled',
 'invoiced',
 'created',
 'approved']
 ```

```
orders = orders[(orders['order_status'] != 'unavailable') & (orders['order_status'] != 'canceled')]
```

Let's choose all order but the ones with status unavailable and canceled. The shop has to return the payment for these orders back to customers, so this shouldn't count.

```
paid_orders = len(orders)
paid_orders
```
```
98206
```

There are 98206 paid orders where the payment hasn't been refunded, not yet at least. Now we can make a new pivot

```
df = orders.pivot_table(index = ['order_year', 'order_month'], values=['customer_id','payment_value', 'order_count'], aggfunc={'customer_id': 'count', 'order_count':'sum', 'payment_value':'sum'}).reset_index()
```
```
df.columns = ['order_year', 'order_month', 'customer_count', 'order_count', 'payment_value']
```

Now I'm creating cohorts via year and date.

```
df["cohort"] = df["order_year"].astype(str) + "-" + df["order_month"].astype(str)
```

Calculating the average amount of orders per customer and average payment amount per order

```
df['mean_order_per_customer'] = round(df['order_count'] / df['customer_count'], 3)
df['mean_payment_per_order'] = round(df['payment_value'] / df['order_count'], 3)
```

```
df = df.set_index('cohort')
df
```

[image](..static\images\post\202103-cohort-analysis-brazilian-ecommerce\df-ecommerce.png)

```
paid_orders / all_orders
```

```
0.9875905068382944
```

```
import matplotlib.pyplot as plt
```

```
plt.figure(figsize=(12,7))
plt.plot(df['payment_value'])
plt.xticks(rotation='45')
plt.title('Total Payments by Cohort', fontsize=14)
plt.xlabel('Cohort', fontsize=13)
plt.ylabel('Total Payments', fontsize=13)
plt.show()
```

[image](..static\images\post\202103-cohort-analysis-brazilian-ecommerce\total-payments-by-cohort.png)

It's clear that there has been growth with a peak in a cohort of November 2017, however it's worth a remark that data are missing for the following months 2016-9, 2016-12, and 2018-9.

- Compare any of the two cohorts by revenue and a number of orders

It's clear that there has been growth with a peak in a cohort of November 2017, however it's worth noticing that the data is missing in 2016-9, 2016-12, and 2018-9

```
selected_cohorts = df.loc[['2018-3', '2018-4']]
selected_cohorts
```

[image](..static\images\post\202103-cohort-analysis-brazilian-ecommerce\selected_cohorts.png)

```
selected_cohorts.iloc[1] - selected_cohorts.iloc[0]
```
```
order_year                    0.000
order_month                   1.000
customer_count             -249.000
order_count                -279.000
payment_value              3567.170
mean_order_per_customer      -0.003
mean_payment_per_order        6.486
dtype: float64
```

The two cohorts are of interest to me because of comparable payment values while the mean order per customer is smaller by 0.003 in the second (2018-4), on average these customers paid more by around 6.5 currency units. Perhaps it's worth studying the profile of those particular customers in detail for advertisement. :)

### Conclusion
