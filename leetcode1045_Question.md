#### 1045. Customers who bought all products
Table: Customer

| Column Name | Type    |
|-------------|---------|
| customer_id | int     |
| product_key | int     |

Table: Product

| Column Name | Type    |
|-------------|---------|
| product_key | int     |

Write an SQL query for a report that provides the customer ids from the Customer table that bought all the products in the Product table.  

For example:

Customer table:
| customer_id | product_key |
|-------------|-------------|
| 1           | 5           |
| 2           | 6           |
| 3           | 5           |
| 3           | 6           |
| 1           | 6           |

Product table:
| product_key |
|-------------|
| 5           |
| 6           |

Result table:
| customer_id |
|-------------|
| 1           |
| 3           |

The customers who bought all the products (5 and 6) are customers with id 1 and 3.


