# KMS-Project
This project contains SQL queries designed to analyze customer behavior and sales patterns from the KMS dataset using Microsoft SQL Server. All queries are based on a single table and provide insights into customer value, purchasing behavior, and segment-based performance over time.

# 🗃️ Table Structure
| Column Name        | Description                                     |
| ------------------ | ----------------------------------------------- |
| `customer_name`    | Name of the customer                            |
| `customer_segment` | Customer type (e.g., Corporate, Small Business,Consumer) |
| `product_name`     | Name of the product or service purchased        |
| `quantity`         | Quantity purchased per transaction              |
| `price`            | Price per unit                                  |
| `order_quantity`   | Number of orders placed (not units)             |
| `order_date`       | Date the order was placed                       |
| `Ship_mode`        | Mode of shipping                    |
| `Region`           | Region where the transaction took place         |

## 📌 Key Business Questions & SQL Queries
- ##### Who are the most valuable customers, and what products do they typically purchase?
``` SELECT 
    k.customer_name,
    k.product_name,
    SUM(k.quantity) AS total_quantity,
    SUM(k.quantity * k.price) AS total_value
FROM 
    KMS k
WHERE 
    k.customer_name IN (
        SELECT TOP 5 customer_name
        FROM KMS
        GROUP BY customer_name
        ORDER BY SUM(Sales) DESC
    )
GROUP BY 
    k.customer_name, k.product_name
ORDER BY 
    k.customer_name, total_value DESC;
```


- ##### Who are the most valuable customers, and what products do they typically purchase?
``` SELECT TOP 1
    customer_name,
    SUM(Sales) AS total_sales
FROM 
    KMS
WHERE 
    customer_segment = 'Small Business'
GROUP BY 
    customer_name
ORDER BY 
    total_sales DESC;
```
##### Which Corporate customer placed the most number of orders between 2009 and 2012?
``` SELECT TOP 1
    customer_name,
    SUM(order_quantity) AS total_orders
FROM 
    KMS
WHERE 
    customer_segment = 'Corporate'
    AND order_date BETWEEN '2009-01-01' AND '2012-12-31'
GROUP BY 
    customer_name
ORDER BY 
    total_orders DESC;
```
# 🧠 Notes
- All queries are optimized for SQL Server.
- No joins are used — analysis is performed on a single flat table (KMS).
- Use of aggregate functions like SUM, GROUP BY, and ORDER BY ensures efficient and meaningful grouping.


