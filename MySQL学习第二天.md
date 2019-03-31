# MySQL学习第二天
# MySQL 基础 （一）- 查询语句

* 导入示例数据库
* SQL是什么？MySQL是什么？
* 查询语句
* 筛选语句
* 分组语句
* 排序语句
* 函数
* SQL注释
* SQL代码规范

## 导入示例数据库

导入示例数据库，教程https://www.yiibai.com/mysql/how-to-load-sample-database-into-mysql-database-server.html

首先启动MySQL服务    `mysql.server start`
登录数据库    `mysql -hlocalhost -uroot -p`  并输入密码

```
mysql> CREATE DATABASE IF NOT EXISTS yiibaidb DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
Query OK, 1 row affected, 2 warnings (0.01 sec)

mysql> use yiibaidb
Database changed
mysql> use yiibaidb
Database changed
mysql> source /Users/limice/Downloads/yiibaidb.sql;
```
![导入数据库](https://raw.githubusercontent.com/limice89/MySQLLearning/master/images/createsql.png)

检查是否导入数据库成功
>mysql> select city,phone,country from \`offices\`;

## SQL是什么？ MySQL是什么？

**SQL** 是Structured Query Language(结构化查询语言) 的缩写。SQL是一种专门用来与数据库沟通的语言

SQL包含三个部分：

- 数据定义语言包含定义数据库及其对象的语句，例如表，视图，触发器，存储过程等。
- 数据操作语言包含允许您更新和查询数据的语句。
- 数据控制语言允许授予用户权限访问数据库中特定数据的权限。

**MySQL**

MySQL是一个数据库管理系统，也是一个关系数据库。

## 查询语句

使用SELECT语句从表或视图获取数据。表由行和列组成，如电子表格。 通常，我们只希望看到子集行，列的子集或两者的组合。SELECT语句的结果称为结果集，它是行列表，每行由相同数量的列组成。

**SELECT语句的语法：**

```
SELECT 
    column_1, column_2, ...
FROM
    table_1
[INNER | LEFT |RIGHT] JOIN table_2 ON conditions
WHERE
    conditions
GROUP BY column_1
HAVING group_conditions
ORDER BY column_1
LIMIT offset, length;

```
*SELECT*语句由以下列表中所述的几个子句组成：

* <b style="color:red">`SELECT`</b>之后是逗号分隔列或星号(*)的列表，表示要返回所有列。

* <b style="color:red">`FROM`</b>指定要查询数据的表或视图。

* <b style="color:red">`JOIN`</b>根据某些连接条件从其他表中获取数据。

* <b style="color:red">`WHERE`</b>过滤结果集中的行。

* <b style="color:red">`GROUP BY`</b>将一组行组合成小分组，并对每个小分组应用聚合函数。

* <b style="color:red">`HAVING`</b>过滤器基于`GROUP BY`子句定义的小分组。

* <b style="color:red">`ORDER BY`</b>指定用于排序的列的列表。

* <b style="color:red">`LIMIT`</b>限制返回行的数量。






