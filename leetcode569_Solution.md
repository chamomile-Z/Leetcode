Runtime: 2693ms
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

Runtime: 1196ms
```MySQL
select e.Id, e.Company, e.Salary
from Employee e, Employee f
where e.Company = f.Company
group by e.Company, e.Salary
having sum(case
          when e.Salary = f.Salary then 1 else 0
          end) >= abs(sum(sign(e.Salary - f.Salary)))
order by e.Id
```
