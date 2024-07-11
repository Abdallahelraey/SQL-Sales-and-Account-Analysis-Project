# Sales Analysis SQL Project

## Overview

This project involves analyzing sales data using SQL queries to gain insights into the performance of different sales regions, the top-performing sales representatives, customer channel preferences, and the spending patterns across different regions.

## SQL Queries and Insights

### Query 1: Number of Accounts per Region

```sql
SELECT r.name, COUNT(*) AS "Number of Accounts"
FROM accounts a
JOIN sales_reps s
ON s.id = a.sales_rep_id
JOIN region r
ON r.id = s.region_id 
GROUP by 1
```

**Insight:** The Northeast region has the highest number of accounts, followed by the West, Southeast, and Midwest.

### Query 2: Top 5 Sales Representatives by Number of Orders

```sql
SELECT s.name, COUNT(o.id) AS "Number of orders"
FROM accounts a
JOIN sales_reps s
ON s.id = a.sales_rep_id
JOIN orders o
ON a.id = o.account_id
GROUP by 1
ORDER by 2 DESC
LIMIT 5
```

**Insight:** Earlie Schleusner is the top sales representative in terms of the number of orders, followed by Vernita Plump, Tia Amato, Georgianna Chisholm, and Moon Torian.

### Query 3: Best Customer Channel Preferences

```sql
WITH t1 AS (
   SELECT a.id, a.name, SUM(o.total_amt_usd) tot_spent
   FROM orders o
   JOIN accounts a
   ON a.id = o.account_id
   GROUP BY a.id, a.name
   ORDER BY 3 DESC
   LIMIT 1)
SELECT a.name, w.channel, COUNT(*)
FROM accounts a
JOIN web_events w
ON a.id = w.account_id AND a.id =  (SELECT id FROM t1)
GROUP BY 1, 2
ORDER BY 3 DESC;
```

**Insight:** The best customer prefers the direct channel the most, followed by organic, AdWords, Facebook, Twitter, and banner channels.

### Query 4: Maximum Spending by Region

```sql
SELECT r.name, MAX(o.total)
FROM accounts a
JOIN orders o
ON a.id = o.account_id
JOIN sales_reps s
ON s.id = a.sales_rep_id
JOIN region r
ON s.region_id = r.id
GROUP BY 1
ORDER BY 2 DESC
```

**Insight:** The West region has the highest spending in terms of total, followed by the Northeast, Southeast, and Midwest.

## Visual Insights

Based on the queries, the following insights were visualized:

- **Number of Accounts per Region:** The Northeast leads with 106 accounts, followed by the West with 101, the Southeast with 96, and the Midwest with 48.
- **Top 5 Sales Representatives by Number of Orders:** Earlie Schleusner is the top sales representative with the most orders.
- **Best Customer Channel Preferences:** The best customer prefers the direct channel the most.
- **Maximum Spending by Region:** The West region has the highest spending in terms of total.

## Requirements

To run these queries, you need access to an SQL database client and the relevant sales data.

## Project Structure

```plaintext
.
├── README.md
└── sales_queries.sql
```

- `README.md`: This file.
- `sales_queries.sql`: Contains the SQL queries used for the analysis.

## Acknowledgements

This project is based on a fictional sales database and was created as part of an SQL learning exercise.
