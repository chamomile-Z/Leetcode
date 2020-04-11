#### Solution
#### Approach1: Using MySQL, join two tables, Runtime: 2558ms(my solution)
```MySQL
select year, price, product_name
from Sales
join Product
on Sales.product_id = Product.product_id
```

#### Approach2: Using MySQL, similar but much faster, Runtime: 1505ms
```MySQL
select distinct p.product_name, s.year, s.price
from 
  (select distinct product_id, year, price from Sales) s
inner join Product p
using (product_id)
```
**Note**  
The second method is much faster than the first one, because it deleted the repeated product_id
which has same price, year but different quantity. The method is very smart!

#### Reference
<https://leetcode.com/problems/product-sales-analysis-i/discuss/413456/MySQL-99.42>
