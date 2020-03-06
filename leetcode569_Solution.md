```SQL Server
with e as
(
    select Id, 
           Company,
           Salary,
           row_number() over (partition by Company order by Salary) Rank,
           count(Id) over (partition by Company) Num
    from Employee
)
select e.Id, e.Company, e.Salary
from e
where (e.Num%2 = 0 and (e.Rank = e.Num/2 or e.Rank = e.Num/2 + 1)) or (e.Num%2 = 1 and e.Rank = (e.Num + 1)/2)
```
