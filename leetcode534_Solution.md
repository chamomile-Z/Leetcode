#### Solution
#### Approach 1: SQL Server window functions, Runtime: 2427ms
``` MS SQL Server
select player_id, 
       event_date, 
       sum(games_played) over(
                                partition by player_id
                                order by event_date
           rows between unbounded preceding and current row) as games_played_so_far        
from Activity
```

> using "partition by" to seperate the table by different player_id, similar to group by   
> using "order by" to order event_date asc
> using framing function: rows between \<Start expr\> and \<End expr\> to select the rows we need

##### Details
> + Window Aggregation Functions  
Window Aggregation Functions support the three factors - Partitioning, Ordering, Framing. The syntax of these three factors as following:
``` SQL Server
function_name(<arguments>) over([<window partition clause>] [<window order clause> [<window frame clause>])
```
> + Framing Part  
> 1. In \<window frame clause\>, there are three parts: \<window frame units\> \<window frame extent\> [\<window frame exclusion\>]
> 2. \<window frame units\> includes ROWS or RANGE
> 3. The usage of ROWS
``` MySQL Script
rows between unbounded preceding |
             <n> preceding |
             <n> following |
             current row
     and unbounded following |
         <n> preceding |
         <n> following |
         current row
```
where,   
> 1. unbounded preceding - the window starts in the first row of the partition 
> 2. unbounded following - the window ends in the last row of the partition  
> 3. current row - the window starts in the current row  
> 4. \<n\> preceding or following - the specific row which is n preceding / following of the current row  

#### Approach 2: Using MySQL, join function, Runtime: 1364ms
```MySQL 
select a.player_id, a.event_date, sum(b.games_played) as games_played_so_far
from Activity a
left join Activity b
on a.player_id = b.player_id and a.event_date >= b.event_date
group by player_id, event_date
order by player_id, event_date
```
##### Details
> + Building copy table to get the result

#### Approach 3: Using MySQL, join function, another way, Runtime: 1517ms
```MySQL
select a.player_id, a.event_date, sum(b.games_played) as games_played_so_far
from Activity a, Activity b
where a.event_date >= b.event_date and a.player_id = b.player_id
group by a.player_id, a.event_date
```

#### Approach 4: Using MySQL, variable, Runtime: 654ms
```MySQL
select player_id, 
       event_date,
       @i := @i * (@p = @p:= player_id)) + games_played as games_played_so_far
from Activity, (select @i := 0, @p := -1) init
order by player_id, event_date
```

##### Details
Very great method!!!

#### Reference
<https://www.cnblogs.com/biwork/p/3244527.html>  
<https://leetcode.com/problems/game-play-analysis-iii/discuss/499476/MySQL-two-solutions-variable-and-join>





