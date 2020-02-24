Write a SQL query to rank scores. If there is a tie between two scores, both should have the same ranking. Note that after a tie, 
the next ranking number should be the next consecutive integer value. 

The following is the Table: Scores

| Id | Score |
| -- | ----- |
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |

For this table, after ranking, the result should be:

| Score | Rank |
| ----- | ---- |
| 4.00  |  1   |
| 4.00  |  1   |
| 3.85  |  2   |
| 3.65  |  3   |
| 3.65  |  3   |
| 3.50  |  4   |
