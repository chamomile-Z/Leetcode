#### Solution
#### Approach 1: using MySQL, sub-query and sum(), Runtime: 173ms
```MySQL 
select question_id as survey_log
from 
(
  select question_id,
         sum(case when action = 'answer' then 1 else 0 end) as num_answer,
         sum(case when action = 'show' then 1 else 0 end) as num_show
  from survey_log
  group by question_id
) as tb1
order by (num_answer / num_show) desc
limit 1
```

#### Details
- Building tb1, which include the question_id, number of answer and number of show.
- Calculating the answer rate = num_answer / num_show, and then order by desc
- Using limit 1 to get the highest answer rate

#### Approach 2: using MySQL, sub-query and count(if...), Runtime: 250ms
```MySQL
select question_id as 'survey_log'
from survey_log
group by question_id
order by count(answer_id) / count(if(action = 'show', 1, 0)) desc
limit 1
```
