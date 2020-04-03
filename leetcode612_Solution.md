#### Solution
#### Approach1: Using MySQL, join, where, sqrt(), pow(), Runtime: 169ms (written by myself)
```MySQL
select min(t.dist) as shortest
from
(
    select round(sqrt(power((t1.x - t2.x), 2) + power((t1.y - t2.y), 2)),2) as dist
    from point_2d t1, point_2d t2
) t
where t.dist > 0.0

#### Solution1: Using MySQL, sqrt() and pow(), Runtime: 301ms
```MySQL
SELECT
    ROUND(SQRT(MIN((POW(p1.x - p2.x, 2) + POW(p1.y - p2.y, 2)))), 2) AS shortest
FROM
    point_2d p1
join
    point_2d p2
on p1.x != p2.x or p1.y != p2.y
```

#### Details
This one includes reduplicate calculations

#### Solution2: Using MySQL, optimize to avoid reduplicate calculations
```MySQL
select round(sqrt(min(pow(p1.x - p2.x, 2) + pow(p1.y - p2.y, 2))), 2) as shortest
from point_2d p1
join point_2d p2
on (p1.x <= p2.x and p1.y < p2.y) or (p1.x <= p2.x and p1.y > p2.y) or (p1.x < p2.x and p1.y = p2.y)
```
