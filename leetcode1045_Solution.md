#### Solution
#### Approach1: Using MySQL, count(), subquery and where, Runtime: 854ms(written by myself)
```MySQL
select customer_id
from
(
    select customer_id, count(distinct(product_key)) as counts
    from Customer
    group by customer_id
) as t1,
(
    select count(product_key) as products
    from Product
)as t2
where t1.counts = t2.products
```

#### Approach2: Using MySQL, similar method, but more clear and simple pattern.
```MySQL
select customer_id
from Customer
group by customer_id
having count(distinct(product_key)) = (select count(distinct product_key) from Product)
```

#### Reference
<https://leetcode.com/problems/customers-who-bought-all-products/discuss/294748/MySQL-subquery>
