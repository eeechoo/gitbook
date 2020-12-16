---
description: 'https://leetcode-cn.com/problems/department-highest-salary/'
---

# 184. 部门工资最高的员工

## 方法1

### 思路

\(1\) 因为 **Employee** 表包含 _Salary_ 和 _DepartmentId_ 字段，我们可以以此在部门内查询最高工资。

```bash
SELECT
    DepartmentId, MAX(Salary)
FROM
    Employee
GROUP BY DepartmentId;
```

{% hint style="info" %}
注意：有可能有多个员工同时拥有最高工资，所以最好在这个查询中不包含雇员名字的信息。
{% endhint %}

```bash
| DepartmentId | MAX(Salary) |
|--------------|-------------|
| 1            | 90000       |
| 2            | 80000       |
```

\(2\) 然后，我们可以把表 **Employee** 和 **Department** 连接，再在这张临时表里用 `IN` 语句查询部门名字和工资的关系。

```text
SELECT
    Department.name AS 'Department',
    Employee.name AS 'Employee',
    Salary
FROM
    Employee
        JOIN
    Department ON Employee.DepartmentId = Department.Id
WHERE
    (Employee.DepartmentId , Salary) IN
    (   SELECT
            DepartmentId, MAX(Salary)
        FROM
            Employee
        GROUP BY DepartmentId
	)
;

作者：LeetCode 
链接：
https://leetcode-cn.com/problems/department-highest-salary/solution/bu-men-gong-zi-zui-gao-de-yuan-gong-by-leetcode/ 
来源：力扣（LeetCode） 
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

{% hint style="info" %}
第一次见 可以两个字段in的 长见识了
{% endhint %}

```text
| Department | Employee | Salary |
|------------|----------|--------|
| Sales      | Henry    | 80000  |
| IT         | Max      | 90000  |
```

### 参考链接





## 方法一（要不要 left join）

 **Employee** 表

```text
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
+----+-------+--------+--------------+
```

 **Department** 表为空

```text
+----+----------+
| Id | Name     |
+----+----------+
```

使用 `left join`

```sql
select Department.name as Department ,Employee.name as Employee ,Salary 
from Employee 
left join Department on Employee.DepartmentId = Department.Id;
```

```text
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| NULL       | Joe      |  70000 |
+------------+----------+--------+
```

where 子句查询结果

```sql
select DepartmentId,max(Salary) 
from Employee 
group by DepartmentId;

+--------------+-------------+
| DepartmentId | max(Salary) |
+--------------+-------------+
|            1 |       70000 |
+--------------+-------------+
```

最终通过 SQL 查询结果

{% hint style="info" %}
可以看到 NULL 的结果并不会被 where 过滤掉，而是保留了下来
{% endhint %}

```sql
select Department.name as Department, Employee.name as Employee, Salary
from Employee
         left join Department on Employee.DepartmentId = Department.Id
where (Employee.DepartmentId, Salary) in
      (select DepartmentId, max(Salary)
       from Employee
       group by DepartmentId);

+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| NULL       | Joe      |  70000 |
+------------+----------+--------+

所以应该使用 
Department left join Employee 
或者直接使用 
join
```

## 方法二

（1）Window Function

```text
用法：
<窗口函数> over (partition by <用于分组的列名>
                order by <用于排序的列名>)
```

（2）SQL 语句

```sql
Select `Department`,
       `Employee`,
       `Salary`
from (Select b.Name                                                       as Department
           , a.Name                                                       as Employee
           , Salary
           , DENSE_RANK() Over (Partition by b.Name Order By Salary DESC) as ranking
      From Employee a
               Join
           Department b
           on a.DepartmentId = b.Id
     ) c
where ranking = 1;

作者：sitara-v
链接：https://leetcode-cn.com/problems/department-highest-salary/solution/bu-men-gong-zi-zui-gao-de-yuan-gong-xiao-rhsj/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


但是因为 ranking 列是现生成的，所以不能这样写

Select b.Name                                                       as Department,
       a.Name                                                       as Employee,
       Salary,
       DENSE_RANK() Over (Partition by b.Name Order By Salary DESC) as ranking
From Employee as a
         Join
     Department as b
     on a.DepartmentId = b.Id
where ranking = 1;




```

