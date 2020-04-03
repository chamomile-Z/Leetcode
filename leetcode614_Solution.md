#### Solution
#### Approach: Using MySQL, count and group by, Runtime: 826ms (written by myself)
```MySQL
select f.ffee as follower, count(distinct f.ffer) as num
from 
( 
    select f1.followee as ffee, f1.follower as ffer
    from follow f1, follow f2
    where f1.followee = f2.follower
) as f
group by f.ffee
```

#### Note
One thing needs to be noticed, when calculating the number, we need to add "distinct", however
I am a little confused why?? Is it because there is duplicated records???
