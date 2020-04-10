#### Solution
#### Approach1: Using MySQL, join, group by and case when, Runtime: 288ms(written by myself)
> step1: calculate the company average salary in different month
```MySQL
select pay_date, round(sum(amount) / count(*), 2) as comp_salary
from salary
group by pay_date
```
> step2: calculate the department average salary in different month
```MySQL
select department_id, pay_date, round(sum(amount) / count(employee.employee_id), 2) as dept_salary
from salary
join employee
on salary.employee_id = employee.employee_id
group by department_id, pay_date
```

> step3: comparison part and merge step 1 table and step2 table 
```MySQL
select distinct(date_format(t2.pay_date, "%Y-%m")) as pay_month,
       t2.department_id,
       case when t1.comp_salary < t2.dept_salary then "higher"
            when t1.comp_salary > t2.dept_salary then "lower"
            else "same"
       end as "comparison"
from 
(
    select pay_date, round(sum(amount) / count(*), 2) as comp_salary
    from salary
    group by pay_date
) as t1
join
(
    select department_id, pay_date, round(sum(amount) / count(employee.employee_id), 2) as dept_salary
    from salary
    join employee
    on salary.employee_id = employee.employee_id
    group by department_id, pay_date
) as t2
on t1.pay_date = t2.pay_date
```

**Update: when I changed the location of "case..when" from same beginning of department_id to same beginning of select the runtime changes a lot, but I don't know why????**
```MySQL

select t2.pay_month,
       department_id,
case 
    when comp_salary < dept_salary then "higher"
    when comp_salary > dept_salary then "lower"
    else "same"
end as "comparison"
from 
(
    select date_format(pay_date, "%Y-%m") as pay_month, round(sum(amount) / count(*), 2) as comp_salary
    from salary
    group by pay_month
) as t1
join
(
    select department_id, date_format(pay_date, "%Y-%m") as pay_month, round(sum(amount) / count(employee.employee_id), 2) as dept_salary
    from salary
    join employee
    on salary.employee_id = employee.employee_id
    group by department_id, pay_month
) as t2
on t1.pay_month = t2.pay_month
```

#### Approach2: Using MySQL, avg and case..when, Runtime: 190ms
```MySQL
select department_salary.pay_month, 
       department_id,
       case when department_avg > company_avg then 'higher'
            when department_avg < compant_avg then 'lower'
            else 'same'
       end as comparison
from 
(
  select department_id, avg(amount) as department_avg, date_format(pay_date, '%Y-%m') as pay_month
  from salary
  join employee 
  on salary.employee_id = employee.employee_id
  group by department_id, pay_month
) as department_salary
join 
(
  select avg(amount) as company_avg,  date_format(pay_date, '%Y-%m') as pay_month from salary group by date_format(pay_date, '%Y-%m')
) as company_salary
on department_salary.pay_month = company_salary.pay_month
```
