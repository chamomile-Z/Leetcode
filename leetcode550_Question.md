Table: Activity 

|  Column Name  |  Type  |
| ------------  |  ----  |
|  player_id    |  int   |
|  device_id    |  int   |
|  event_date   |  date  |
|  games_played |  int   |

(player_id, event_date) is the primary key of this table.  

Write an SQL query that reports the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places. 
In other words, you need to count the number of players that logged in for at least two consecutive days starting from their first login date, 
then divide that number by the total number of players.

The query result format is in the following example:  
Activity table:  

| player_id | device_id | event_date | games_played |
| --------- | --------- | ---------- | ------------ |
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-03-02 | 6            |
| 2         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |

Result table:  

| fraction |
| -------- |
|   0.33   |


