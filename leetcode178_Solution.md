#### Solution
#### Approach 1: using SQL Server's window function dense_rank(), Runtime is 1611 ms
```SQL Server script
select Score, dense_rank() over(order by Score desc) as Rank
from Scores
order by Rank 
```
#### Details
SQL Server window functions deal with the sets of rows defined by OVER syntax which mainly help to analyze,  
- Running Totals  
- Moving averages  
- Gaps and islands   
##### Other functions using with OVER
Aggregate functions: SUM, COUNT, MIN, MAX  
Rank functions: RANK, DENSE_RANK, ROW_NUMBER, BTILE  
Distribution functions: PERCENT_RANK, CUME-DIST, PERCENTILE_CONT, PERCENTILE_DISC  
Offset function: LAG, LEAD, FIRST_VALUE, LAST_VALUE

In this question, dense_rank() could help to generate the result. And The following table will show clearly about 
the difference among RANK(), DENSE_RANK() and ROW_NUMBER().  

| Scores | RANK() | DENSE_RANK() | ROW_NUMBER() |
| ------ | ------ | ------------ | ------------ |
|  100   |   1    |      1       |      1       |
|   97   |   2    |      2       |      2       |
|   97   |   2    |      2       |      3       |
|   90   |   4    |      3       |      4       |
|   84   |   5    |      4       |      5       |
|   84   |   5    |      4       |      6       |
|   84   |   5    |      4       |      7       |
|   77   |   8    |      5       |      8       |
|   75   |   9    |      6       |      9       |
|   60   |   10   |      7       |      10      |

Since the question requests to have no "holes" between ranks, so using DENSE_RANK().

#### Approach 2: using MySQL's inner join, Runtime is 524 ms
``` MySQL script
select s1.Score, count(s2.Score) as Rank
from 
(
  select distinct Score
  from Scores
) as s2,
Scores as s1
where s1.Score <= s2.Score
group by s1.Id, s1.Score
order by s1.Score desc
```
#### Details
This method is wonderful! Generating two tables, s1 is the original table, s2 is a new table which actually separate distinct 
scores. A specific score's consecutive rank is same to count the number how many scores are bigger or equal than the score.

#### Approach 3: using MySQL, similar to the above, Runtime is 747 ms
``` MySQL script
select Score, (select count(distinct Score) from Scores where Score > = s.Score) as Rank
from Scores as s
order by Score desc
```

#### Approach 4: using MySQL, similar to the above, Runtime is 308 ms
``` MySQL script
select Score, (select count(*) from (select distinct Score s from Scores) t 
where s > = Score) as Rank
from Scores
order by Score desc
```
#### Approach 5: using MySQL, quite different with the above solutions, Runtime is 427 ms
``` MySQL script
select Score,
#build two variables, using @var_name.
#set value for variables, using := (which equals to set var_name = var_value)
@rank := @rank + (@pre <> (@pre := Score)) Rank
from Scores, (select @rank := 0, @pre := -1) init
order by Score desc;
```
#### Summary
Great questions! Learn a lot from this question, it is useful to do multiple times for each approach!!!

#### Reference (above approaches are collected based on the following website address)
<https://www.cnblogs.com/grandyang/p/5351611.html>    
<https://www.cnblogs.com/biwork/p/3241983.html>




