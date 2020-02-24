#### Solution
```MySQL script
create function getNthHighestSalary(N int) returns int  
begin  
declare M int;  
set M = N - 1;  
  return(    
    select distinct Salary as getNthHighestSalary  
    from Employee  
    order by Salary desc  
    limit 1 offset M  
);  
end 
```
#### Details
##### Create Function Statements
```MySQL
CREATE
  [DEFINER = user]   
  FUNCTION sp_name ([func_parameter[,....]])
  RETURNS type
  [characteristic ...] routine_body
```
*func_parameter* :  
  param_name type
  
*type* :  
  Any valid MySQL data type

*routine_body* :  
  Valid SQL routine statement
  
##### Example (using the solution as an example to understand "create function in MySQL")   
```MySQL script
-- 开始创建函数(可以类比C语言中的声明部分）
create function getNthHighestSalary(N int) returns int  
begin  
--定义变量（declare var_name type[default value], declare 定义的变量范围是从 begin ... end, 一次性可定义多个变量)
declare M int;  
--变量赋值（set var_name = expr)
set M = N - 1;  
  return( 
    -- 函数体（同时也是想返回的值）
    select distinct Salary as getNthHighestSalary  
    from Employee  
    order by Salary desc  
    limit 1 offset M  
);  
end 
```




