#### Solution
#### Approach1: Using MySQL, group by, Runtime: 1173ms
```MySQL
select product_id, sum(quantity) as total_quantity
from Sales
group by product_id
```
