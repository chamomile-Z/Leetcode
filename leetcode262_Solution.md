#### Solution
#### Approach 1. I solved the question divided into three parts. It is a little complicated.
##### Step1 count the cancellation number
```MySQL script
select t1.Request_at, ifnull(Cnum,0) as cancellationNum
from Trips t1
left join 
(
  select t.Request_at, count(t.Status) as Cnum
  from Trips t,
  (
    select u.Users_Id
    from Users u
    where Banned = 'No'
  ) as u1,
  (
    select u.Users_Id
    from Users u
    where Banned = 'No'
  ) as u2
  where (t.Client_Id, t.Driver_Id) = (u1.Users_Id, u2.Users_Id) and t.Status <> 'Completed'
  group by t1.Request_at
) t2
on t1.Request_at = t2.Request_at
group by t1.Request_at
```

##### Step2 count the total request number 
```MySQL script
select t.Request_at as Day, count(t.Status) as totalNum
from Trips t,
(
  select u.Users_Id
  from Users u
  where Banned = 'No'
) as u1,
(
  select u.User_Id
  from Users u
  where Banned = 'No'
) as u2
where (t.Client_Id, t.Driver_Id) = (u1.Users_Id, u2.Users_Id)
group by t.Request_at
```

##### Step3 combine step1's table(num1) and step2's table(num2) to get the rate
```MySQL script
select num2.Day, round(ifnull(num1.cancellationNum / num2.totalNum, null), 2) as 'Cancellation Rate'
from 
(
  select t1.Request_at, ifnull(Cnum,0) as cancellationNum
  from Trips t1
  left join 
  (
    select t.Request_at, count(t.Status) as Cnum
    from Trips t,
    (
      select u.Users_Id
      from Users u
      where Banned = 'No'
    ) as u1,
    (
      select u.Users_Id
      from Users u
      where Banned = 'No'
    ) as u2
    where (t.Client_Id, t.Driver_Id) = (u1.Users_Id, u2.Users_Id) and t.Status <>   'Completed'
    group by t1.Request_at
  ) t2
  on t1.Request_at = t2.Request_at
  group by t1.Request_at
) num1,
(
  select t.Request_at as Day, count(t.Status) as totalNum
  from Trips t,
  (
    select u.Users_Id
    from Users u
    where Banned = 'No'
  ) as u1,
  (
    select u.User_Id
    from Users u
    where Banned = 'No'
  ) as u2
  where (t.Client_Id, t.Driver_Id) = (u1.Users_Id, u2.Users_Id)
  group by t.Request_at
) num2
where num1.Request_at = num2.Day amd num2.Day >= '2013-10-01' and num2.Day <= '2013-10-03'
```

#### Other great Approaches (from leetcode discuss part)
##### Approach2. Provided by one coder in leetcode discuss (much better than mine)
1. build a newT removing all unwanted records from Trips table
2. from newT, group by date and compute cancellation rate using sum(case()) function
```MySQL script
select Request_at as Day,
round(sum(case when Status like 'cancelled%' then 1 else 0 end) / count(*), 2) as 'Cancellation Rate'
from 
(
  select * from Trips t
  where 
    t.Client_Id not in (select Users_Id from Users where Banned = 'Yes') and
    t.Driver_Id not in (select Users_Id from Users where Banned = 'Yes') and
    t.Request_at between '2013-10-01' and '2013-10-03'
) as newT
group by Request_at
```

#### Reference 
<https://leetcode.com/problems/trips-and-users/discuss/69151/Sharing-my-solution>

