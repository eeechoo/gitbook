---
description: 'https://leetcode-cn.com/problems/department-top-three-salaries/'
---

# 185. 部门工资前三高的所有员工

## 思路1：window function

**Employee** 表先排序，然后 `join` 上 **Department 表**，最后使用 `where` 过滤

```sql
SELECT Name,
       Salary,
       DepartmentId,
       DENSE_RANK() over ( partition by DepartmentId order by Salary DESC) as Ranking
FROM Employee


完整代码
select a.Name as Employee,
       b.Name as Department,
       Salary
from (SELECT Name,
             Salary,
             DepartmentId,
             DENSE_RANK() over ( partition by DepartmentId order by Salary DESC) as Ranking
      FROM Employee) a
         join department b on a.DepartmentId = b.Id
where Ranking <= 3;
```

思路2：

