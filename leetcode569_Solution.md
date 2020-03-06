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

Runtime: 244ms
```MySQL
SELECT 
    Id, Company, Salary
FROM
    (SELECT 
        e.Id,
            e.Salary,
            e.Company,
            IF(@prev = e.Company, @Rank:=@Rank + 1, @Rank:=1) AS rank,
            @prev:=e.Company
    FROM
        Employee e, (SELECT @Rank:=0, @prev:=0) AS temp
    ORDER BY e.Company , e.Salary , e.Id) Ranking
        INNER JOIN
    (SELECT 
        COUNT(*) AS totalcount, Company AS name
    FROM
        Employee e2
    GROUP BY e2.Company) companycount ON companycount.name = Ranking.Company
WHERE
    Rank = FLOOR((totalcount + 1) / 2)
        OR Rank = FLOOR((totalcount + 2) / 2)
```
