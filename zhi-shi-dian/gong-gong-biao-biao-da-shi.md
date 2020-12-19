# 公共表表达式

## subquery

 A subquery is a `SELECT` statement within another statement.

子查询 不能复用，所以这是子查询的劣势所在。  
  
[derived table](https://dev.mysql.com/doc/refman/8.0/en/derived-tables.html) 这个概念隶属于 [subquery ](https://dev.mysql.com/doc/refman/8.0/en/subqueries.html)这个概念下



## 什么是 CTE（Common-Table-Expression）

这篇文章必须读完，并理解透彻 [https://dev.mysql.com/doc/refman/8.0/en/with.html\#common-table](https://dev.mysql.com/doc/refman/8.0/en/with.html#common-table-expressions-recursive-examples)[expressions-recursive-examples](https://dev.mysql.com/doc/refman/8.0/en/with.html#common-table-expressions-recursive-examples)

## 个人理解：

recursive CTE 中的 recursive 并不是指 编程中的递归概念，而是应该对应于 循环 这个概念，应该对应于 LOOP，FOR， WHILE 这些循环概念。  
下面我们来回顾下 For 循环的概念  


```c
for(int i = 0; i < 10; i++){、}
1. for 初始化 i = 0
2. 判断 i < 10
3. 执行
```



## 帮助理解的文章：

[https://www.essentialsql.com/introduction-common-table-expressions-ctes/](https://www.essentialsql.com/introduction-common-table-expressions-ctes/) [https://www.essentialsql.com/recursive-ctes-explained/](https://www.essentialsql.com/recursive-ctes-explained/)







