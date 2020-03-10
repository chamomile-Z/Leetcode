#### Solution
#### Approach 1: Using SQL Server, combine sum function, Runtime: 646ms
```SQL Server
select sum(Number) / (count(Number) + 0.0) as median
from 
(
  select Number,
         Frequency,
         sum(Frequency) over() as total,
         sum(Frequency) over(order by Number 
                        rows between unbounded preceding and current row) as runtotal
  from Numbers
) n1
where total / 2.0 between runtotal - Frequency and runtotal
```

#### Approach 2: Using MySQL, variable, Runtime: 144ms
```MySQL
select avg(Number) as median
from 
  (select * from Numbers order by Number) a,
  (select @i := 0) init
where
  Frequency >= abs(@i+(@i:=@i+Frequency) - select sum(Frequency) from Numbers))
```

#### Details
Still stucked on a hard question, could only figure out half of this question. The two approaches are found in Leetcode discussion part.
Compared the two methods, using window functions is easier to think and to write, but the runtime is still long. Using variables 
is hard but very fast. (I still don't handle the method of writing variables in SQL.... Sad... Need more study and more practice)

