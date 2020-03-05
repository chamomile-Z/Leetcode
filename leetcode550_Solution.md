#### Solution
#### Approach 1: Using MySQL, join function, Runtime: 471ms
```MySQL
select round(ifnull(count(distinct b.player_id), 0) / count(distinct c.player_id), 2) as fraction
from 
(
    select a.player_id as id, min(a.event_date) as str_date
    from Activity a
    group by a.player_id
)as aa, Activity b, Activity c
where b.player_id = aa.id and b.event_date = date_add(str_date, interval 1 day)
```

#### Details
> + Seperating the whole question in three parts
> 1. total number of players
```
select count(distinct player_id) as total
from Activity
```
> 2. the number of players who logged in for at least two consecutive days starting from their first login date.  
     - **Notice: here are two conditions!!! (1) at least two consecutive days (2) starting from their first login date**
```
select count(distinct b.player_id)
from 
(
  select a.player_id as id, min(a.event_date) as str_date
  from Activity a
  group by a.player_id
) as aa, Activity b
where b.player_id = aa.id and b.event_date = date_add(str_date, interval 1 day)
```
> 3. combine the above two parts and calculate the fraction
```
select round(ifnull(count(distinct b.player_id),0) / count(distinct c.player_id),2) as fraction
from 
(
  select a.player_id as id, min(a.event_date) as str_date
  from Activity a
  group by a.player_id
) as aa, Activity b, Activity c
where b.player_id = aa.id and b.event_date = date_add(str_date, interval 1 day)
```
