
# SQL Task 2 Monthly & Yearly Sales (Finance Analytics)


## Objective

Analyze Croma India's financial performance by calculating and aggregating gross sales prices on a monthly and yearly basis for enhanced decision-making.

---

### Monthly Gross Sales Report


#### Steps and Queries


1. Calculated Gross Price per Transaction: Multiply sold_quantity by gross_price.
2. Aggregated Monthly Sales: Sum gross sales grouped by the month.


##### Step 1: Calculating Gross Price Total for Transactions
The gross price for each transaction is calculated by multiplying the sold quantity by the gross price. 

```sql
SELECT date, 
       ROUND(sold_quantity * gross_price, 2) AS Gross_price_total
FROM fact_sales_monthly s
JOIN fact_gross_price g
ON s.product_code = g.product_code 
   AND g.fiscal_year = get_fiscal_year(s.date)
WHERE customer_code = 90002002
ORDER BY date ASC;
```

##### Step 2: Aggregating Monthly Gross Sales
To provide a single row per month, gross sales data is aggregated using the SUM function and grouped by the transaction date.

```sql
SELECT date, 
       ROUND(SUM(sold_quantity * gross_price), 2) AS Gross_price_total
FROM fact_sales_monthly s
JOIN fact_gross_price g
ON s.product_code = g.product_code 
   AND g.fiscal_year = get_fiscal_year(s.date)
WHERE customer_code = 90002002
GROUP BY s.date
ORDER BY date ASC;
```
The result of this query provides insights into monthly sales trends and customer spending patterns.


### Yearly Gross Sales Report

#### Task Overview

- The objective is to generate a yearly gross sales report for Croma India, summarizing total sales by fiscal year.

#### Steps and Queries

1. Grouped Fiscal Year: Aggregated sales data using the get_fiscal_year() UDF.
2. Summarized Total Gross Sales: Calculated the yearly gross sales to identify financial performance.

##### Step 1: Generating Yearly Sales Data

The fiscal year and total gross sales for that year are calculated by grouping data by the fiscal year and summing the gross price totals.
- Here, get_fiscal_year() is the UDF I created to simplify the code and minimize the complexity in code readability.

```sql
SELECT get_fiscal_year(date) AS fiscal_year,
       SUM(ROUND(sold_quantity * g.gross_price, 2)) AS yearly_sales
FROM fact_sales_monthly s
JOIN fact_gross_price g
ON g.fiscal_year = get_fiscal_year(s.date) 
   AND g.product_code = s.product_code
WHERE customer_code = 90002002
GROUP BY get_fiscal_year(date)
ORDER BY fiscal_year;
```

This query provides a concise yearly summary of gross sales, allowing stakeholders to assess the company's financial growth over time.
