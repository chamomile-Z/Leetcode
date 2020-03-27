#### Solution
#### Approach 1:  Using MySQL, outer join, count() and ifnull(), Runtime: 306ms
```MySQL 
select dept_name, ifnull(count(student_id), 0) as student_number
from department d
left join student s
on s.dept_id = d.dept_id
group by d.dept_name
order by student_number desc, d.dept_name
```

#### Approach 2: Using MySQL, another way to write, Runtime: 388ms
```MySQL 
select dept_name, count(student_id) as student_number
from student s
right join department d
on s.dept_id = d.dept_id
group by dept_name
order by student_number desc, dept_name 
```
