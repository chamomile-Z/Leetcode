#### Solution  
#### Approach 1: Using MySQL, union all function, Runtime: 692ms (written by myself) 
> * step 1, group by requester_id, get the number of accepter_id

```MySQL
select requester_id as id, count(accepter_id) as num
from request_accepted
group by requester_id
```
> * step 2, group by accepter_id, get the number of requester_id

```MySQL
select accepter_id as id, count(requester_id) as num
from request_accepted
group by accepter_id
```

> * step 3, union the first one and second one based on "union all"

```MySQL
select requester_id as id, count(accepter_id) as num
from request_accepted
group by requester_id
union all
select accepter_id as id, count(requester_id) as num
from request_accepted
group by accepter_id
```

> * step 4, select via order by and limit 1

```MySQL
select t2.id, t2.num
from
(
select t.subid as id, sum(t.subnum) as num
from
(
select requester_id as subid, count(accepter_id) as subnum
from request_accepted
group by requester_id
union all
select accepter_id as subid, count(requester_id) as subnum
from request_accepted
group by accepter_id
) as t
group by t.subid
) as t2
order by t2.num desc
limit 1
```

> * Update

```MySQL
select t.subid as id, sum(t.subnum) as num
from
(
select requester_id as subid, count(accepter_id) as subnum
from request_accepted
group by requester_id
union all
select accepter_id as subid, count(requester_id) as subnum
from request_accepted
group by accepter_id
) as t
group by t.subid
order by num desc
limit 1
```

