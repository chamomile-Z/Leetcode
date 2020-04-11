#### SOlution
#### Approach1: Using MySQL, group by and having, Runtime:328ms(written by myself)
```MySQL
select actor_id, director_id
from ActorDirector
group by actor_id, director_id
having count(distinct(timestamp)) >= 3
```

#### Approach2: Using MySQL, group by and having, Runtime: 352ms(written by myself)
```MySQL
select actor_id, director_id
from ActorDirector
group by actor_id, director_id
having count(*) >= 3
```
