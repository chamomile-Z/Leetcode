#### Solution
#### Approach: Using MySQL, group by and count(), Runtime：293ms
```MySQL
select round(sum(TIV_2016), 2) as TIV_2016
from insurance
where insurance.TIV_2015 in 
(
  select TIV_2015
  from insurance 
  group by TIV_2015
  having count(*) > 1
)
and concat(LAT, LON) in 
(
  select concat(LAT, LON)
  from insurance
  group by LAT, LON
  having count(*) = 1
)
```

#### Details
1. Concat the LAT and LON as a whole to represent the location information  
2. These two criteria should be met without an order, so if you attempt to filter data using criteria #1 
and then criteria #2, you will get a wrong result.  

#### Reference
<https://leetcode.com/problems/investments-in-2016/solution/>
