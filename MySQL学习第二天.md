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

### SELECT语句的语法

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

检索单列  `SELECT prod_name FROM Products;`

检索多列  `SELECT prod_id,prod_name,prod_price FROM Products;`

检索所有列  `SELECT * FROM Products;`

### 去重语句:
`SELECT DISTINCT vend_id FROM Products;`

### 限制结果:前N个语句

* 在 SQL Server和 Access中使用SELECT时，可以使用TOP关键字来限制
`SELECT TOP 5 prod_name FROM Products;`

* 如果使用的DB2，习惯使用下面这一 DBMS特定的 SQL语 句:
`SELECT prod_name
FROM Products
FETCH FIRST 5 ROWS ONLY;`

* 使用 Oracle，需要基于ROWNUM(行计数器)来计算行:

`SELECT prod_name FROM Products
WHERE ROWNUM <=5;`

* 使用 MySQL、MariaDB、PostgreSQL或者 SQLite，需要使用LIMIT 子句:
`SELECT prod_name FROM Products LIMIT 5;`

* 为了得到后面的 5行数据，需要指定从哪儿开始以及检索的行数:
`SELECT prod_name FROM Products LIMIT 5 OFFSET 5;`
LIMIT 5 OFFSET 5指示 MySQL等 DBMS返回从第 5行起的 5行数据

### CASE…END判断语句
https://www.yiibai.com/mysql/case-statement.html
**语法**

*简单case语句*

```
CASE  case_expression
   WHEN when_expression_1 THEN commands
   WHEN when_expression_2 THEN commands
   ...
   ELSE commands
END CASE;
---------------------------------------------------
eg:
CASE sex
         WHEN '1' THEN '男'
         WHEN '2' THEN '女'
ELSE '其他' END
```

*case搜索语句*

```
CASE
    WHEN condition_1 THEN commands
    WHEN condition_2 THEN commands
    ...
    ELSE commands
END CASE;

---------------------------------------------------
eg:
CASE WHEN sex = '1' THEN '男'
         WHEN sex = '2' THEN '女'
ELSE '其他' END

```

## 筛选语句 where

### 语法

```
SELECT field1, field2,...fieldN FROM table_name1, table_name2...
[WHERE condition1 [AND [OR]] condition2.....
```

* 查询语句中你可以使用一个或者多个表，表之间使用逗号`,` 分割，并使用WHERE语句来设定查询条件。
* 你可以在 WHERE 子句中指定任何条件。
* 你可以使用 AND 或者 OR 指定一个或多个条件。
* WHERE 子句也可以运用于 SQL 的 DELETE 或者 UPDATE 命令。
*  WHERE 子句类似于程序语言中的 if 条件，根据 MySQL 表中的字段值来读取指定的数据。

### 操作符

	操作符|说明|操作符|说明
    ---|:--:|---:|---
     	=| 等于|>|大于
    <>| 不等于|>=|大于等于
    !=| 不等于|!>|不大于
    <| 小于|BETWEEN|在指定的两个值之间
    <=| 小于等于|IS NULL|为NULL值
    !<| 不小于|LIKE|匹配基于模式匹配的值


```
SELECT lastname, firstname, jobtitle FROM employees WHERE jobtitle = 'Sales Rep';
/******AND操作符-用来检索满足所有给定条件的行********/
SELECT lastname, firstname, jobtitle FROM employees WHERE jobtitle = 'Sales Rep' AND officeCode = 1;

/******OR操作符-用来检索匹配任一条件的行********/
SELECT lastname, firstname, jobtitle FROM employees WHERE jobtitle = 'Sales Rep' OR officeCode = 1;

/******求值顺序-SQL处理OR操作符前，优先处理AND操作符********/
eg:
SELECT prod_name,prod_price FROM Products WHERE vend_id = 'DLL01' OR vend_id = 'BRS01' AND prod_price >=10;

输出：
prod_name               prod_price 
-------------------    ----------
Fish bean bag toy       3.4900
Bird bean bag toy       3.4900
Rabbit bean bag toy     3.4900
18 inch teddy bear      11.9900
Raggedy Ann             4.9900
此问题的解决方法是使用圆括号对操作符进行明确分组

SELECT prod_name, prod_price
FROM Products
WHERE (vend_id = 'DLL01' OR vend_id = 'BRS01')
AND prod_price >= 10;

输出：
prod_name              prod_price 
-------------------    ---------- 
18 inch teddy bear     11.9900

/******IN操作符-指定条件范围，范围中的每个条件都可以进行匹配********/

SELECT prod_name, prod_price FROM Products
WHERE vend_id IN ( 'DLL01', 'BRS01' ) ORDER BY prod_name;

输出：
prod_name                 prod_price
-------------------       ----------
12 inch teddy bear        8.9900
18 inch teddy bear        11.9900
8 inch teddy bear         5.9900
Bird bean bag toy         3.4900
Fish bean bag toy         3.4900
Rabbit bean bag toy       3.4900
Raggedy Ann               4.9900

/******NOT操作符-用来否定其后条件的关键字********/

SELECT prod_name
FROM Products
WHERE NOT vend_id = 'DLL01' ORDER BY prod_name;

输出：
prod_name 
------------------ 
12 inch teddy bear 
18 inch teddy bear 
8 inch teddy bear 
King doll
Queen doll
```
### 通配符 -用来匹配值的一部分的特殊字符

**搜索模式(search pattern)** 由字面值、通配符或两者组合构成的搜索条件

*1.* 百分号（%）通配符   ->%表示任何字符出现任意次数

```
SELECT prod_id, prod_name FROM Products
WHERE prod_name LIKE 'Fish%';

prod_id            prod_name
-------            ------------------ 
BNBG01             Fish bean bag toy
```

*2.* 下划线(_)通配符   ->只匹配 单个字符，而不是多个字符。

>DB2不支持通配符`_`,如果使用的是 Microsoft Access，需要使用`?`而不是`_`。

```

SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE '__ inch teddy bear';
```

*3.* 方括号([ ])通配符

方括号([])通配符用来指定一个字符集，它必须匹配指定位置(通配 符的位置)的一个字符

```
找出所有名字以J或M起头的联系人，可进行如下查询
SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[JM]%' ORDER BY cust_contact;
```

## 分组语句 GROUP BY

**语法**

```
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;
```

### 聚集函数


聚集函数是以值是一个集合（集或者多重集）为输入、返回单个值得函数。SQL提供了五个固有聚集函数。

平均值：avg		
最小值：min		
最大值：max		
总和：sum   
计数：count

#### 基本聚集
上面五种都属于基本聚集

```
-- 找出教师的平均工资
SELECT AVG (salary)
FROM instructor

--找出名字的个数，这里的distinct作用是是的重复的元素不被计数
SELECT COUNT(DISTINCT name)
FROM instructor;
```

#### 组合聚集函数


SELECT语句可根据需要包含多个聚集函数

```
SELECT COUNT(*) AS num_items, MIN(prod_price) AS price_min, MAX(prod_price) AS price_max, AVG(prod_price) AS price_avg
FROM Products;

num_items      price_min   price_max      price_avg
----------     ----------  ---------      ---------
9              3.4900      11.9900        6.823333
```

### 分组聚集 GROUP BY

**group by 子句**作用：对给出的一个或多个属性来构造分组，将属性上取值相同的元组分到同一组中。

```
# 找出每个系的教师平均工资
SELECT dept_name, avg(salary) AS avg_salary
FROM instructor
GROUP BY dept_name;
//按dept_name排序并分组数据 ，对每个dept_name 而不是整表计算 avg_salary

# 找出教师的平均工资,则是省略了group by，将整个关系当成一个组别
SELECT AVG (salary)
FROM instructor
```

* GROUP BY子句可以包含任意数目的列，因而可以对分组进行嵌套，
更细致地进行数据分组。
* 如果在GROUP BY子句中嵌套了分组，数据将在最后指定的分组上进 行汇总。换句话说，在建立分组时，指定的所有列都一起计算(所以 不能从个别的列取回数据)。
* GROUP BY子句中列出的每一列都必须是检索列或有效的表达式(但 不能是聚集函数)。如果在SELECT中使用表达式，则必须在GROUPBY 子句中指定相同的表达式。不能使用别名。
* 大多数 SQL实现不允许GROUP BY列带有长度可变的数据类型(如文 本或备注型字段)。
* 除聚集计算语句外，SELECT语句中的每一列都必须在GROUPBY子句 中给出。
* 如果分组列中包含具有NULL值的行，则NULL将作为一个分组返回。 如果列中有多行NULL值，它们将分为一组。
* GROUP BY子句必须出现在WHERE子句之后，ORDER BY子句之前。

### 过滤分组 HAVING
在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与聚合函数一起使用。

HAVING 子句可以让我们筛选分组后的各组数据		
**语法**

```
 SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value;

```

```
SELECT cust_id, COUNT(*) AS orders FROM Orders
GROUP BY cust_id
HAVING COUNT(*) >= 2;

cust_id        orders 
----------     ----------- 
1000000001     2
```
>HAVING 和 WHERE 的差别 这里有另一种理解方法，WHERE在数据分组前进行过滤，HAVING在数 据分组后进行过滤。这是一个重要的区别，WHERE排除的行不包括在 分组中。这可能会改变计算值，从而影响 HAVING子句中基于这些值 过滤掉的分组。

```
SELECT vend_id, COUNT(*) AS num_prods FROM Products
WHERE prod_price >= 4
GROUP BY vend_id
HAVING COUNT(*) >= 2;
----------------------------------------------------
vend_id        num_prods 
-------        ----------- 
BRS01          3
FNG01          2

//这条语句中，第一行是使用了聚集函数的基本SELECT语句，很像前面的 例子。WHERE子句过滤所有prod_price至少为4的行，然后按vend_id 分组数据，HAVING子句过滤计数为 2或 2以上的分组。如果没有WHERE 子句，就会多检索出一行(供应商 DLL01，销售 4个产品，价格都在 4 以下):

SELECT vend_id, COUNT(*) AS num_prods FROM Products
GROUP BY vend_id
HAVING COUNT(*) >= 2;
----------------------------------------------------
vend_id            num_prods 
-------            ----------- 
BRS01              3
DLL01              4
FNG01              2
```

## 排序语句 ORDER BY

**语法**

```
SELECT field1, field2,...fieldN table_name1, table_name2...
ORDER BY field1, [field2...] [ASC [DESC]]
```


* 你可以使用任何字段来作为排序的条件，从而返回排序后的查询结果。
* 你可以设定多个字段来排序。
* 你可以使用 ASC 或 DESC 关键字来设置查询结果是按升序或降序排列。 默认情况下，它是按升序排列。
* 你可以添加 WHERE...LIKE 子句来设置条件。

```
SELECT prod_id, prod_price, prod_name FROM Products
ORDER BY prod_price, prod_name;

```

***ORDER BY与GROUP BY***


ORDER BY   |   GROUP BY
:--:|:--:
对产生的输出排序|对行分组，但输出可能不是分组的顺序
任意列都可以使用(甚至非选择的列也可以使用)|只可能使用选择列或表达式列，而且必须使用每个选择列表达式
不一定需要|如果与聚集函数一起使用列(或表达式)，则必须使用

### 倒序 正序
倒序 DESC  正序 ASC

```
SELECT prod_id, prod_price, prod_name FROM Products
ORDER BY prod_price DESC, prod_name;

//DESC关键字只应用到直接位于其前面的列名。在上例中，只对prod_price 列指定DESC，对prod_name列不指定。因此，prod_price列以降序排 序，而prod_name列(在每个价格内)仍然按标准的升序排序
```

## 函数

### MySQL 数字函数
### MySQL 字符串函数
### MySQL 日期函数
参考：
http://www.runoob.com/mysql/mysql-functions.html

## SQL注释

*1.* 单行注释

SQL语句中的单行注释使用 `--`

```
create database database_x    --创建数据库database_x

```

*2.* 多行注释

SQL语句中的多行注释采用 `/*…*/`

```
create database database_x
/*
创建一个数据库
名字叫做database_x
*/
```

## SQL代码规范
参考：
[SQL编程格式的优化建议] https://zhuanlan.zhihu.com/p/27466166
 [SQL Style Guide] https://www.sqlstyle.guide


#作业#
```
项目一：查找重复的电子邮箱（难度：简单）
创建 email表，并插入如下三行数据
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+

编写一个 SQL 查询，查找 Email 表中所有重复的电子邮箱。
根据以上输入，你的查询应返回以下结果：
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
说明：所有电子邮箱都是小写字母。
```

![作业一](https://raw.githubusercontent.com/limice89/MySQLLearning/master/images/day2_1.png)
![作业一](https://raw.githubusercontent.com/limice89/MySQLLearning/master/images/day2_2.png)

```
项目二：查找大国（难度：简单）
创建如下 World 表
+-----------------+------------+------------+--------------+---------------+
| name            | continent  | area       | population   | gdp           |
+-----------------+------------+------------+--------------+---------------+
| Afghanistan     | Asia       | 652230     | 25500100     | 20343000      |
| Albania         | Europe     | 28748      | 2831741      | 12960000      |
| Algeria         | Africa     | 2381741    | 37100000     | 188681000     |
| Andorra         | Europe     | 468        | 78115        | 3712000       |
| Angola          | Africa     | 1246700    | 20609294     | 100990000     |
+-----------------+------------+------------+--------------+---------------+
如果一个国家的面积超过300万平方公里，或者(人口超过2500万并且gdp超过2000万)，那么这个国家就是大国家。
编写一个SQL查询，输出表中所有大国家的名称、人口和面积。
例如，根据上表，我们应该输出:
+--------------+-------------+--------------+
| name         | population  | area         |
+--------------+-------------+--------------+
| Afghanistan  | 25500100    | 652230       |
| Algeria      | 37100000    | 2381741      |
+--------------+-------------+--------------+

```

![作业二](https://raw.githubusercontent.com/limice89/MySQLLearning/master/images/day2_3.png)
![作业二](https://raw.githubusercontent.com/limice89/MySQLLearning/master/images/day2_4.png)






