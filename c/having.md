`HAVING` 子句在 SQL 中用于对使用 `GROUP BY` 子句分组后的结果进行过滤。它和 `WHERE` 子句的作用类似，但 `WHERE` 在 `GROUP BY` 之前应用，用于过滤行，而 `HAVING` 在 `GROUP BY` 之后应用，用于过滤分组后的结果。

### `HAVING` 的作用和使用场景：

1. **过滤分组后的数据**：
   - `HAVING` 子句允许你在分组和聚合操作之后过滤结果。你可以使用 `HAVING` 来指定条件，只有符合条件的分组才会出现在最终结果中。

2. **与聚合函数结合使用**：
   - `HAVING` 通常与聚合函数（如 `SUM`、`COUNT`、`AVG`、`MAX`、`MIN` 等）一起使用，以便进一步过滤聚合后的结果。

### 举例说明：

假设你有一个包含员工信息的表 `employees`，包括 `employee_id`、`department_id` 和 `salary` 列。你想要查找每个部门的总工资大于 50000 的部门，可以使用 `HAVING` 子句来实现。

```sql
SELECT department_id, SUM(salary) AS total_salary
FROM employees
GROUP BY department_id
HAVING SUM(salary) > 50000;
```

### 解释：

1. **`GROUP BY department_id`**：
   - 首先，按 `department_id` 分组。

2. **`SUM(salary) AS total_salary`**：
   - 对每个部门的工资求和。

3. **`HAVING SUM(salary) > 50000`**：
   - 过滤出总工资大于 50000 的部门。

### `HAVING` 与 `WHERE` 的区别：

- **`WHERE` 子句**：在分组和聚合之前过滤行。
- **`HAVING` 子句**：在分组和聚合之后过滤结果。

#### 举例对比：

假设你有一张销售记录表 `sales`，包含 `sale_id`、`amount` 和 `sale_date` 列。

**使用 `WHERE` 过滤行**：
```sql
SELECT sale_date, SUM(amount) AS total_sales
FROM sales
WHERE amount > 100
GROUP BY sale_date;
```
- 这会先过滤出 `amount` 大于 100 的销售记录，然后按 `sale_date` 分组并计算每天的总销售额。

**使用 `HAVING` 过滤分组后的结果**：
```sql
SELECT sale_date, SUM(amount) AS total_sales
FROM sales
GROUP BY sale_date
HAVING SUM(amount) > 1000;
```
- 这会先按 `sale_date` 分组并计算每天的总销售额，然后只返回总销售额大于 1000 的日期。

总结来说，`HAVING` 子句用于在分组和聚合操作之后过滤结果，是对分组结果进行进一步筛选的重要工具。它在处理聚合数据时非常有用，为数据分析提供了更强大的能力。