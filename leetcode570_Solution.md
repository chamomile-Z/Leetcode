#### Solution
#### Approach 1: Using MySQL, join function, Runtime: 454ms (written by myself)
```MySQL
select e.Name
from Employee e
join 
(
    select f.ManagerId, count(f.Id) as Num
    from Employee f
    group by f.ManagerId
    having Num > 4
) as ee
on ee.ManagerId = e.Id
```
#### Details
Build two tables. The first one is original table, the second one is finding the ManagerId which has at least 5 direct report(using group by
could get the number for each different managerId). Then, select the name from the first table based on the second table's ManagerId is same 
to the first table's Id.

#### Approach 2: Using MySQL, where function, Runtime: 212ms (found on leetcode discussion part)
```MySQL
select Name
from Employee
where Id in 
(
  select ManagerId
  from Employee
  group by ManagerId
  having count(*) >= 5
)
```
#### Details
Using where to select the effective rows from Employee table. I found when using where the runtime will be faster than building two tables.
But not sure if it is a rule or it is just a coincidence. Still need to figure out.

