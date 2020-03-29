#### Solution
#### Approach 1: Using MySQL, with join, union and where, Runtime: 264ms (written by myself)
```MySQL
select s1.id, s1.visit_date, s1.people
from stadium s1, stadium s2, stadium s3
where s2.id - s1.id = 1 and s3.id - s2.id = 1 and s1.people >= 100 and s2.people >= 100 and s3.people >= 100
union
select s2.id, s2.visit_date, s2.people
from stadium s1, stadium s2, stadium s3
where s2.id - s1.id = 1 and s3.id - s2.id = 1 and s1.people >= 100 and s2.people >= 100 and s3.people >= 100
union 
select s3.id, s3.visit_date, s3.people
from stadium s1, stadium s2, stadium s3
where s2.id - s1.id = 1 and s3.id - s2.id = 1 and s1.people >= 100 and s2.people >= 100 and s3.people >= 100
order by id, visit_date
```

#### Details
1. **Join** function to help choose out consecutive rows  
2. **Where** function to help choose out consecutive rows and people number larger or equal than 100  
3. **Union** function to help combine three sub-table information

#### Note
Union: could delete the repeat row in different tables
Union all: will reserve(keep) all the information in different tables, even the rows are repeat.

#### Approach 2: Using MySQL, with join and where, Runtime: 258ms (solution)
```MySQL
select distinct t1.*
from stadium t1, stadium t2, stadium t3
where t1.people >= 100 and t2.people >= 100 and t3.people >= 100
and
(
   (t1.id - t2.id = 1 and t1.id - t3.id = 2 and t2.id - t3.id =1)  -- t1, t2, t3
   or
   (t2.id - t1.id = 1 and t2.id - t3.id = 2 and t1.id - t3.id =1) -- t2, t1, t3
   or
   (t3.id - t2.id = 1 and t2.id - t1.id =1 and t3.id - t1.id = 2) -- t3, t2, t1
)
order by t1.id
```

#### Approach 3: Using SQL Server, with row_number, Runtime: 770ms (discussion)
```SQL Server 
with cte as
(
SELECT id, visit_date, people,
row_number() over(order by id) as rn
FROM stadium
where people >= 100
), cte2 as
(
SELECT id, visit_date, people, id-rn as diff
from cte
)
select id, visit_date, people
from cte2
WHERE diff in (select diff from cte2
group by diff
having count(diff) >= 3)
```

#### Approach 4: Using SQL Server, with lead and lag, Runtime: 1150ms (discussion)
```SQL Server
select id
, visit_date
, currentPeople as people
from
(select id
, visit_date
, people as currentPeople
, lead(people,1) over(order by visit_date) as next_1
, lead(people,2) over(order by visit_date) as next_2
, lag(people,1) over(order by visit_date) as prev_1
, lag(people,2) over(order by visit_date) as prev_2
from stadium)a
where (prev_1>=100 and prev_2>=100 and currentPeople>=100)
or (prev_1>=100 and next_1>=100 and currentPeople>=100)
or (next_2>=100 and next_1>=100 and currentPeople>=100)
```

#### Reference
<https://leetcode.com/problems/human-traffic-of-stadium/solution/>  
<https://leetcode.com/problems/human-traffic-of-stadium/discuss/488757/MS-SQL-Solution-95-faster-than-other-MS-SQL-Submissions>  
<https://leetcode.com/problems/human-traffic-of-stadium/discuss/394649/Faster-than-95-submissions!-Easy-to-understand>

