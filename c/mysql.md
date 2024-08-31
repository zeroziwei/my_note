---
tags: 
created_date: 2024-01-13 15:57
modified_date: " 2024-01-13 15:57:44"
review_date: 
pomodoro_time: 
used_time: 
aliases: 
---

## 1	资源

[高频 SQL 50 题（进阶版） - 学习计划 - 力扣（LeetCode）全球极客挚爱的技术成长平台](http://101.201.28.47:8050/studyplan/sql-premium-50/)


## 2	mysql 

[MySQL之C语言API学习小结 – 又见杜梨树](https://dulishu.top/mysql-c-api/)

[[mysql 安装]]


## 3	创建

```mysql 
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    birthdate DATE,
    is_active BOOLEAN DEFAULT TRUE
);
```

## 4	插入 

```mysql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

## 5	查询

``` mysql 
SELECT DISTINCT
<select_list>
FROM
<left_table> <join_type>
JOIN <right_table> ON <join_condition>
WHERE
<where_condition>
GROUP BY
<groupby_list>
HAVING
<having_condition>
ORDER BY
<orderby_condition>
LIMIT <limit number>
```

![[mysql-20240820062703965.webp]]



```mysql
SELECT DISTINCT
    e.employee_id,
    e.first_name,
    e.last_name,
    e.department,
    SUM(s.salary) AS total_salary
FROM
    employees e
INNER JOIN
    salaries s ON e.employee_id = s.employee_id
WHERE
    s.pay_date >= '2023-01-01'
GROUP BY
    e.employee_id, e.first_name, e.last_name, e.department
HAVING
    SUM(s.salary) > 0
ORDER BY
    total_salary DESC
LIMIT 5;

```

```mysql 
SELECT e.employee_id, e.first_name, e.last_name, SUM(s.salary) as total_salary 
FROM employees e 
INNER JOIN salaries s ON e.employee_id = s.employee_id 
WHERE s.pay_date >= '2023-01-01'
group by e.employee_id 
order by total_salary;

```

[[having]]



## 6	索引 

