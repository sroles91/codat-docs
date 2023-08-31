---
title: "Commerce metrics"
sidebar_label: Overview
description: "Describes the metrics and formulas used to calculate them by the commerce metrics endpoints"
createdAt: "2022-04-04T07:04:34.727Z"
updatedAt: "2022-11-30T14:38:23.838Z"
---

Commerce metrics include a set of pre-calculated ratios and metrics focused on commerce and sales data that help lenders assess merchant health and evaluate the credit risk of a company. _Commerce metrics_ endpoints provide the data that lets you see what works and what doesn't, giving you the opportunity to advise your customers or prospects on what they can do to improve their performance.

## What metrics are available?

Our _Commerce metrics_ endpoints, available through our API reference, return metrics on:

- [Revenue](https://docs.codat.io/lending-api#/operations/get-commerce-revenue-metrics)
- [Orders](https://docs.codat.io/lending-api#/operations/get-commerce-orders-metrics)
- [Refunds](https://docs.codat.io/lending-api#/operations/get-commerce-refunds-metrics)
- [Customer retention](https://docs.codat.io/lending-api#/operations/get-commerce-customer-retention-metrics)
- [Lifetime value](https://docs.codat.io/lending-api#/operations/get-commerce-lifetime-value-metrics)

The following table details which metrics are calculated and their formulas:

| Metric                 | Description                                                                                                               | How that's translated to Codat data                                                                                                                                                                                                                                                                                                                                                                                                                           |
|------------------------|---------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **[Revenue](https://docs.codat.io/lending-api#/operations/get-commerce-revenue-metrics)**            |                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Revenue                | The gross revenue for a selected period.                                                                                  | `Revenue = SUM(orderlines.quantity * orderlines.unitPrice)` for the specified period                                                                                                                                                                                                                                                                                                                                                                          |
| Revenue growth         | The percentage change in revenue between the present period’s value and the previous period's value.                          | `Revenue growth % change = ((b-a)/a) * 100` compared to the previous period. <br/> a. previous month's revenue, <br/> b. current month's revenue.                                                                                                                                                                                                                                                                                                                     |
| **[Orders](https://docs.codat.io/lending-api#/operations/get-commerce-orders-metrics)**             |                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Number of orders       | The number of orders for a specific period.                                                                               | `Number of orders = COUNT(orders)` for that period                                                                                                                                                                                                                                                                                                                                                                                                            |
| Order value            | The sum of the values of all orders over a specific period.                                                               | `Value of orders = SUM(orders.totalAmount)` for that period                                                                                                                                                                                                                                                                                                                                                                                                   |
| Average order value    | The average order value over a specified period.                                                                        | `Average order value = a / b` for that period. <br/> a. order value, <br/> b. COUNT(orders).                                                                                                                                                                                                                                                                                                                                                                              |
| **[Refunds](https://docs.codat.io/lending-api#/operations/get-commerce-refunds-metrics)**            |                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Number of refunds      | The number of orders where the `totalRefund` field is NOT NULL.                                                             | `Number of refunds = COUNT(orders)` for the selected period where `totalRefund > 0` for the selected period                                                                                                                                                                                                                                                                                                                                                  |
| Value of refunds       | The sum of all refunds over a specified period.                                                                           | `Value of refunds = SUM(orders.totalRefund)` for the selected period, always expressed as a negative value                                                                                                                                                                                                                                                                                                                                                    |
| Refund rate            | The number of refunds compared with the number of orders over a specific period.                                          | `Refund rate = a / b` for that period. <br/> a. number of refunds, <br/> b. number of orders.                                                                                                                                                                                                                                                                                                                                                                             |
| **[Customer retention](https://docs.codat.io/lending-api#/operations/get-commerce-customer-retention-metrics)** |                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Existing customers     | COUNT of unique customers where they have placed orders in the specified period AND any previous period                   | `Existing customers = COUNT(customers)` who placed orders in the specified period and any previous period                                                                                                                                                                                                                                                                                                                                                     |
| New customers          | COUNT of unique customers where they have placed orders in the specified period AND NONE in any previous period.          | `New customers = COUNT(customers)` who placed orders in the specified period only                                                                                                                                                                                                                                                                                                                                                                             |
| Total customers        | SUM of Existing and New customers.                                                                                        | `Total customers = a + b`. <br/> a. new customers, <br/> b. existing customers.                                                                                                                                                                                                                                                                                                                                                                                           |
| Retention rate         | The percentage of existing customers within the period compared to the total customers at the end of the previous period. | `Retention rate = (a/(b + c)) * 100` <br/> a. COUNT(customers): current period's existing customers, i.e. customers who have placed their very first order before the current period <br/> b. COUNT(customers): previous period's existing customers, i.e. customers who have placed their very first order before the previous period <br/> c. COUNT(customers): previous period's new customers, i.e. customers who have placed their very first order in the previous period |
| Repeat rate            | The percentage of existing customers to total customers over the specified period.                                        | `Repeat rate = (a / a + b) * 100` <br/> a. COUNT(customers): current period's existing customers, i.e. customers who have placed their very first order before the current period <br/> b. COUNT(customers): current period's new customers, i.e. customers who have placed their very first order in the current period                                                                                                                                                  |
| **[Lifetime value](https://docs.codat.io/lending-api#/operations/get-commerce-lifetime-value-metrics)**     |                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Lifetime value         | The revenue a business can expect from a paying customer during their time as a paying customer.                          | `Lifetime value = a * b * c` <br/> a. Average order value for that period <br/> b. `COUNT(orders) / COUNT(customers)`: average number of orders per customer, for that period <br/> c. `Average customer lifespan`: average difference in days between the last and first orders in a specified period, for all customers.                                                                                                                                                      |
