# MySQL学习三

* [表操作](#1)

	1. [MySQL表数据类型](#1.1)
	2. [用SQL语句创建表](#1.2)
	3. [用SQL语句向表中添加数据](#1.3)
	4. [用SQL语句删除表](#1.4)
	5. [用SQL语句修改表](#1.5)
	
* [表联结](#2)

	1. [MySQL 别名](#2.1)
	2. [INNER JOIN](#2.2)
	3. [LEFT JOIN](#2.3)
	4. [CROSS JOIN](#2.4)
	5. [自连接](#2.5)
	6. [UNION](#2.6)

## <span id = "1">表操作</span>

### <span id = "1.1">MySQL表数据类型<span>

>数据类型是定义列中可以存储什么数据以及该数据实 际怎样存储的基本规则。

MySQL支持多种类型，大致可以分为三类：数值、日期/时间和字符串(字符)类型。

#### 数值类型

类型|大小|范围(有符号)|范围(无符号)|用途
---|---|---|---|---
TINYINT|1字节|(-128,127)|(0,255)|小整数值
SMALLINT|2字节|(-32 768，32 767) |(0，65 535) |大整数值
MEDIUMINT|3字节|(-8 388 608，8 388 607) |(0，16 777 215) |大整数值
INT或INTEGER |4字节| (-2 147 483 648，2 147 483 647) | 	(0，4 294 967 295) |大整数值
BIGINT|8字节|(-9,223,372,036,854,775,808，9 223 372 036 854 775 807) |(0，18 446 744 073 709 551 615) |极大整数值
FLOAT|4字节|(-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) |0，(1.175 494 351 E-38，3.402 823 466 E+38) |单精度浮点数值
DOUBLE|8字节|(-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) |0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) |双精度浮点数值
DECIMAL|对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 |依赖于M和D的值 |依赖于M和D的值  |小数值

#### 日期和时间类型

>表示时间值的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。

每个时间类型有一个有效值范围和一个"零"值，当指定不合法的MySQL不能表示的值时使用"零"值。

类型|大小(字节)|范围|格式|用途
---|---|---|---|---
DATE| 3|1000-01-01/9999-12-31 |YYYY-MM-DD |日期值 
TIME|3| 	'-838:59:59'/'838:59:59' | 	HH:MM:SS | 时间值或持续时间
YEAR|1|1901/2155 |YYYY |年份值
DATETIME|8|1000-01-01 00:00:00/9999-12-31 23:59:59 | YYYY-MM-DD HH:MM:SS|混合日期和时间值 
TIMESTAMP| 4|1970-01-01 00:00:00/2038|结束时间是第 2147483647 秒，北京时间 2038-1-19 11:14:07，格林尼治时间 2038年1月19日 凌晨 03:14:07|YYYYMMDD HHMMSS | 	混合日期和时间值，时间戳 

#### 字符串类型

>字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET

类型| 	大小 |	用途
---|---|---
CHAR |	0-255字节 |	定长字符串
VARCHAR |	0-65535 字节 |	变长字符串
TINYBLOB |0-255字节 |	不超过 255 个字符的二进制字符串
TINYTEXT |0-255字节 |	短文本字符串
BLOB |	0-65 535字节 |	二进制形式的长文本数据
TEXT |	0-65 535字节 |	长文本数据
MEDIUMBLOB |	0-16 777 215字节 |	二进制形式的中等长度文本数据
MEDIUMTEXT |	0-16 777 215字节 |	中等长度文本数据
LONGBLOB |	0-4 294 967 295字节 |	二进制形式的极大文本数据
LONGTEXT |	0-4 294 967 295字节 |	极大文本数据

>不管使用何种形式的字符串数据类型，字符串值都必须括在单引号内

### <span id = "1.2">用SQL语句创建表<span>

要在数据库中创建一个新表，可以使用MySQL CREATE TABLE语句:

```
CREATE TABLE [IF NOT EXISTS] table_name(
        column_list
) engine=table_type;

```
>表名在数据库中必须是唯一的。 `IF NOT EXISTS`是语句的可选部分，允许您检查正在创建的表是否已存在于数据库中,烈建议在每个`CREATE TABLE`语句中使用`IF NOT EXISTS`来防止创建已存在的新表而产生错误
>其次，在`column_list`部分指定表的列表。字段的列用逗号(，)分隔
>需要为`engine`子句中的表指定存储引擎。可以使用任何存储引擎，如：InnoDB，MyISAM，HEAP，EXAMPLE，CSV，ARCHIVE，MERGE， FEDERATED或NDBCLUSTER。如果不明确声明存储引擎，MySQL将默认使用InnoDB。

示例:

```
CREATE TABLE IF NOT EXISTS tasks (
  task_id INT(11) NOT NULL AUTO_INCREMENT,
  subject VARCHAR(45) DEFAULT NULL,
  start_date DATE DEFAULT NULL,
  end_date DATE DEFAULT NULL,
  description VARCHAR(200) DEFAULT NULL,
  PRIMARY KEY (task_id)
) ENGINE=InnoDB;

-----------------------------------------------------------

*column_name指定列的名称。每列具有特定数据类型和大小，例如：VARCHAR(255)。
*NOT NULL或NULL表示该列是否接受NULL值。
*DEFAULT值用于指定列的默认值。
*AUTO_INCREMENT指示每当将新行插入到表中时，列的值会自动增加。每个表都有一个且只有一个AUTO_INCREMENT列。
PRIMARY KEY 关键字用于定义列为主键。 您可以使用多列来定义主键，列间以逗号分隔

```

#### 存储引擎

存储引擎就是指表的类型以及表在计算机上的存储方式。

MySql支持的存储引擎包括MyISAM、InnoDB、BDB、MEMORY、MERGE、EXAMPLE、NDB Cluster、ARCHIVE、CSV、BLACKHOLE、FEDERATED等

创建表如果不指定存储引擎，系统默认使用默认存储引擎，MySql5.5之前的默认引擎是MyISAM，5.5之后改为InnoDB

**MyISAM**

MyISAM 不支持事务、也不 不支持外键，其优点是速度快，对事务完整性没有要求。以SELECT和INSERT为主的应用基本上都就可以使用这个表

**InnoDB**

InnoDB存储引擎提供了具有提交、回滚和崩溃恢复能力的事务安全。但是对比MyISAM的存储引擎，InnoDB写的处理效率差一些，并且会占用更多的磁盘空间以保留数据和索引。

```
create table autoincre_demo (i smallint not null auto_increment,name varchar(10),primary key(i))engine=innodb;
insert into autoincre_demo values(1,'1'),(0,'2'),(null,'3');

如果插入空或者0，则实际插入的将是自动增长后的值。
```

**MEMORY **

memory 存储引擎使用存在于内存中的内容来创建表，每个MEMORY表实际对应一个磁盘文件，格式是.fm,该文件中只存储表的结构。而其数据文件，都是存储在内存中，这样有利于数据的快速处理，提高整个表的效率。值得注意的是，服务器需要有足够的内存来维持MEMORY存储引擎的表的使用。如果不需要了，可以释放内存，甚至删除不需要的表。


MEMORY默认使用哈希索引。速度比使用B型树索引快。当然如果你想用B型树索引，可以在创建索引时指定。


注意，MEMORY用到的很少，因为它是把数据存到内存中，如果内存出现异常就会影响数据。如果重启或者关机，所有数据都会消失。因此，基于MEMORY的表的生命周期很短，一般是一次性的。

![存储引擎对比](https://images2015.cnblogs.com/blog/851461/201703/851461-20170306202857781-1607368004.png)

* **InnoDB**：支持事务处理，支持外键，支持崩溃修复能力和并发控制。如果需要对事务的完整性要求比较高（比如银行），要求实现并发控制（比如售票），那选择InnoDB有很大的优势。如果需要频繁的更新、删除操作的数据库，也可以选择InnoDB，因为支持事务的提交（commit）和回滚（rollback）。 

* **MyISAM**：插入数据快，空间和内存使用比较低。如果表主要是用于插入新记录和读出记录，那么选择MyISAM能实现处理高效率。如果应用的完整性、并发性要求比 较低，也可以使用。

* **MEMORY**：所有的数据都在内存中，数据的处理速度快，但是安全性不高。如果需要很快的读写速度，对数据的安全性要求较低，可以选择MEMOEY。它对表的大小有要求，不能建立太大的表。所以，这类数据库只使用在相对较小的数据库表。

注意，同一个数据库也可以使用多种存储引擎的表。如果一个表要求比较高的事务处理，可以选择InnoDB。这个数据库中可以将查询要求比较高的表选择MyISAM存储。如果该数据库需要一个用于查询的临时表，可以选择MEMORY存储引擎。

### <span id = "1.3">用SQL语句向表中添加数据<span>

**语法**

```
INSERT INTO table_name ( field1, field2,...fieldN )
                       VALUES
                       ( value1, value2,...valueN );
```

**多种添加方式**

```
--指定列名
INSERT INTO student(id,name,gender,age) VALUES(01,'xiaoming',1,18); 
--不指定列名
INSERT INTO employee VALUES(02,'xiaobai',1,17);
--指定列名
INSERT INTO employee(id,name) VALUES(03,'xiaohong');
--添加多条
INSERT INTO student(id,name,gender,age) VALUES(01,'xiaoming',1,18)，
(02,'xiaohong',0,16)，
(03,'xiaoxiao',1,17);
```

**表复制**

```
INSERT INTO table_1
SELECT c1, c2, FROM table_2;


```

### <span id = "1.4">用SQL语句删除表<span>

* 删除表 ----DROP

```
DROP TABLE table_name;
```

```
实例：删除学生表。

drop table student;
```

* 删除表内数据 ----DELETE

```
DELETE FROM table_name
WHERE condition;

```

```
实例：删除学生表内姓名为张三的记录。

delete from  student where  T_name = "张三";
```

* 清除表内数据，保存表结构 ----TRUNCATE

```
TRUNCATE TABLE yable_name;
```

```
实例：清除学生表内的所有数据。

truncate  table  student;
```

>1、当你不再需要该表时， 用 drop;		
>2、当你仍要保留该表，但要删除所有记录时， 用 truncate		
>3、当你要删除部分记录时， 用 delete。

### <span id = "1.5">用SQL语句修改表<span>

* 重命名表

```
第一种
RENAME TABLE old_table_name TO new_table_name;
第二种
ALTER TABLE old_table_name
RENAME TO new_table_name;


---------------------------------------------------
旧表(old_table_name)必须存在，新表(new_table_name)必须不存在。 如果新表new_table_name存在，则该语句将失败。
除了表之外，我们还可以使用RENAME TABLE语句来重命名视图。
在执行RENAME TABLE语句之前，必须确保没有活动事务或锁定表。

请注意，不能使用RENAME TABLE语句来重命名临时表，但可以使用ALTER TABLE语句重命名临时表。


```

* 修改列名

```
语法：ALTER TABLE tb_name CHANGE [COLUMN] old_col_name new_col_name column_definition [FIRST | AFTER col_name]
ALTER TABLE 表名字 CHANGE 原列名 新列名 数据类型 约束;

eg:
修改表tb1的name字段，改为username

ALTER TABLE tb1 CHANGE name username VARCHAR(50);
```

* 修改表中的数据

```
UPDATE [LOW_PRIORITY] [IGNORE] table_name 
SET 
    column_name1 = expr1,
    column_name2 = expr2,
    ...
WHERE
    condition;
    
eg:
UPDATE employees 
SET 
    lastname = 'Hill',
    email = 'mary.hill@yiibai.com'
WHERE
    employeeNumber = 1056;


```

* 删除行

```
DELETE TABLE table_name where condition;

eg:
DELETE FROM employees 
WHERE
    officeCode = 4;
----------------------------------------
限制要删除的行数，则使用LIMIT子句
DELETE FROM table
LIMIT row_count;
请注意，表中的行顺序未指定，因此，当您使用LIMIT子句时，应始终使用ORDER BY子句，不然删除的记录可能不是你所预期的那样。

DELETE FROM table_name
ORDER BY c1, c2, ...
LIMIT row_count;

```

* 删除列

```
ALTER TABLE table_name DROP (COLUMN) column_name;

eg:
ALTER TABLE student
DROP name;
```

* 新建列

```
在一个建好的表中增加一列：
ALTER TABLE table_name ADD COLUMN new_column_name VARCHAR(20) NOT NULL;
这条语句会向已有的表中加入新的一列，这一列在表的最后一列位置。如果我们希望添加在指定的一列，可以用：
ALTER TABLE table_name ADD COLUMN new_column_name VARCHAR(20) NOT NULL AFTER column_name;
如果添加到第一列，使用：
ALTER TABLE table_name ADD COLUMN new_column_name VARCHAR(20) NOT NULL FIRST;
```

* 新建行

```
INSERT INTO table_name ( field1, field2,...fieldN )
                       VALUES
                       ( value1, value2,...valueN );
```

###### 作业 #######

```
项目三：超过5名学生的课（难度：简单）
创建如下所示的courses 表 ，有: student (学生) 和 class (课程)。
例如,表:
+---------+------------+
| student | class      |
+---------+------------+‘’
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |
| A      | Math       |
+---------+------------+

编写一个 SQL 查询，列出所有超过或等于5名学生的课。
应该输出:
+---------+
| class   |
+---------+
| Math    |
+---------+
Note:
学生在每个课中不应被重复计算。
```

```
CREATE TABLE IF NOT EXISTS courses(
student VARCHAR(8) NOT NULL, class VARCHAR(40) NULL
);

INSERT INTO courses(student,class) VALUES('A','Math'),
('B','English'),
('C','Math'),
('D','Biology'),
('E','Math'),
('F','Computer'),
('G','Math'),
('H','Math'),
('I','Math'),
('A','Math');

SELECT class FROM courses GROUP BY class HAVING count(DISTINCT student)>=5;

```

```
项目四：交换工资（难度：简单）
创建一个 salary表，如下所示，有m=男性 和 f=女性的值 。
例如:
| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |

交换所有的 f 和 m 值(例如，将所有 f 值更改为 m，反之亦然)。要求使用一个更新查询，并且没有中间临时表。
运行你所编写的查询语句之后，将会得到以下表:
| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f  | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |
```

```
CREATE TABLE IF NOT EXISTS salary(
id INT(10) NOT NULL AUTO_INCREMENT,
name VARCHAR(10) DEFAULT NULL,
sex VARCHAR(1) NOT NULL,
salary VARCHAR(10) NULL,
PRIMARY KEY (id)
);

INSERT INTO salary(id, name,sex,salary) VALUES(1,'A','m','2500'),
(2,'B','f','1500'),
(3,'C','m','5500'),
(4,'D','f','500');

UPDATE salary SET sex = (CASE sex WHEN 'f' THEN 'm' ELSE 'f' END);
UPDATE salary SET sex = if(sex ='f','m','f');

```


## <span id = "2">表联结</span>

### <span id = "2.1"> MySQL别名</span>

**为表取别名:**

```
SELECT * FROM 表名 [AS] 别名；
--------------------------------------------------------------
SELECT * FROM employees as
em WHERE em.employeeNumber = 1501;

+----------------+----------+-----------+-----------+------------------+------------+-----------+-----------+
| employeeNumber | lastName | firstName | extension | email            | officeCode | reportsTo | jobTitle  |
+----------------+----------+-----------+-----------+------------------+------------+-----------+-----------+
|           1501 | Bott     | Larry     | x2311     | lbott@yiibai.com | 7          |      1102 | Sales Rep |
+----------------+----------+-----------+-----------+------------------+------------+-----------+-----------+
```

**为字段取别名**

```
SELECT 字段名 [AS] 别名 [，字段名 [AS] 别名，……] FROM 表名； 
------------------------------------------------------------
SELECT employeeNumber AS id, lastName AS name 
FROM employees;
+------+-----------+
| id   | name      |
+------+-----------+
| 1002 | Murphy    |
| 1056 | Patterson |
| 1076 | Firrelli  |
| 1088 | Patterson |
| 1102 | Bondur    |
| 1143 | Bow       |
| 1165 | Jennings  |
| 1166 | Thompson  |
| 1188 | Firrelli  |
| 1216 | Patterson |
| 1286 | Tseng     |
| 1323 | Vanauf    |
| 1337 | Bondur    |
| 1370 | Hernandez |
| 1401 | Castillo  |
| 1501 | Bott      |
| 1504 | Jones     |
| 1611 | Fixter    |
| 1612 | Marsh     |
| 1619 | King      |
| 1621 | Nishi     |
| 1625 | Kato      |
| 1702 | Gerard    |
+------+-----------+
23 rows in set (0.00 sec)

SELECT
 CONCAT_WS(' ', lastName, firstname) `Full name`
FROM
 employees
ORDER BY
 `Full name`;
+-------------------+
| Full name         |
+-------------------+
| Bondur Gerard     |
| Bondur Loui       |
| Bott Larry        |
| Bow Anthony       |
| Castillo Pamela   |
| Firrelli Jeff     |
| Firrelli Julie    |
| Fixter Andy       |
| Gerard Martin     |
| Hernandez Gerard  |
| Jennings Leslie   |
| Jones Barry       |
| Kato Yoshimi      |
| King Tom          |
| Marsh Peter       |
| Murphy Diane      |
| Nishi Mami        |
| Patterson Mary    |
| Patterson Steve   |
| Patterson William |
| Thompson Leslie   |
| Tseng Foon Yue    |
| Vanauf George     |
+-------------------+
23 rows in set (0.01 sec)

```

### <span id = "2.2"> INNER JOIN内连接</span>

![内连接](https://www.yiibai.com/uploads/images/201707/1707/671100747_41692.png)

>MySQL INNER JOIN子句将一个表中的行与其他表中的行进行匹配，并允许从两个表中查询包含列的行记录。

**语法**

```
SELECT column_list
FROM t1
INNER JOIN t2 ON join_condition1
INNER JOIN t3 ON join_condition2
...
WHERE where_conditions;

```
![https://www.yiibai.com/uploads/images/201707/1707/202110711_46519.png](https://www.yiibai.com/uploads/images/201707/1707/202110711_46519.png)

```

获取products表中的productCode和productName列的值。获取productlines表产品线的描述 - textDescription列的值。

SELECT 
    productCode, 
    productName, 
    textDescription
FROM
    products t1
        INNER JOIN
    productlines t2 ON t1.productline = t2.productline;
或者
SELECT 
    productCode, 
    productName, 
    textDescription
FROM
    products
        INNER JOIN
    productlines USING (productline);


```

***MySQL INNER JOIN GROUP BY子句***
![订单和订单详细表](https://www.yiibai.com/uploads/images/201707/1707/932110748_83699.png)

```
---s使用GROUP BY 子句的INNER JOIN 子句从 orders 和 orderdetails 表中获取订单号,订单状态和总销售额

SELECT
 	T1.orderNumber,
 	status,
 	SUM(quantityOrdered * priceEach) total
FROM
	orders AS T1
		INNER JOIN
	orderdetails AS T2 ON T1.orderNumber = T2.orderNumber
GROUP BY orderNumber
limit 10;
----------------------------------------------------------
+-------------+---------+----------+
| orderNumber | status  | total    |
+-------------+---------+----------+
|       10100 | Shipped | 10223.83 |
|       10101 | Shipped | 10549.01 |
|       10102 | Shipped |  5494.78 |
|       10103 | Shipped | 50218.95 |
|       10104 | Shipped | 40206.20 |
|       10105 | Shipped | 53959.21 |
|       10106 | Shipped | 52151.81 |
|       10107 | Shipped | 22292.62 |
|       10108 | Shipped | 51001.22 |
|       10109 | Shipped | 25833.14 |
+-------------+---------+----------+
10 rows in set (0.00 sec)

SELECT 
    orderNumber, 
    productName, 
    msrp, 
    priceEach
FROM
    products p
        INNER JOIN
    orderdetails o ON p.productcode = o.productcode
        AND p.msrp > o.priceEach
WHERE
    p.productcode = 'S10_1678';
--------------------------------------------------------
+-------------+---------------------------------------+-------+-----------+
| orderNumber | productName                           | msrp  | priceEach |
+-------------+---------------------------------------+-------+-----------+
|       10107 | 1969 Harley Davidson Ultimate Chopper | 95.70 |     81.35 |
|       10121 | 1969 Harley Davidson Ultimate Chopper | 95.70 |     86.13 |
|       10134 | 1969 Harley Davidson Ultimate Chopper | 95.70 |     90.92 |
|       10145 | 1969 Harley Davidson Ultimate Chopper | 95.70 |     76.56 |
|       10159 | 1969 Harley Davidson Ultimate Chopper | 95.70 |     81.35 |
|       10403 | 1969 Harley Davidson Ultimate Chopper | 95.70 |     85.17 |
...
|       10417 | 1969 Harley Davidson Ultimate Chopper | 95.70 |     79.43 |
+-------------+---------------------------------------+-------+-----------+
26 rows in set (0.00 sec)

```

### <span id = "2.3"> LEFT JOIN左连接</span>

![左连接接](https://www.yiibai.com/uploads/images/201707/1807/936120749_19056.png)


>LEFT JOIN子句允许您从匹配的左右表中查询选择行记录，连接左表(t1)中的所有行，即使在右表(t2)中找不到匹配的行也显示出来，但使用NULL值代替

示例:
![订单表和客户表](https://www.yiibai.com/uploads/images/201707/1807/357120755_17620.png)
>
订单(orders)表中的每个订单必须属于客户(customers)表中的客户。客户(customers)表中的每个客户在订单(orders)表中可以有零个或多个订单。

要查询每个客户的所有订单，可以使用LEFT JOIN子句

```
SELECT
 c.customerNumber,
 c.customerName,
 orderNumber,
 o.status
FROM
 customers c
LEFT JOIN orders o ON c.customerNumber = o.customerNumber;
或者:
SELECT
 c.customerNumber,
 customerName,
 orderNumber,
 status
FROM
 customers c
LEFT JOIN orders USING (customerNumber);

```

***使用MySQL LEFT JOIN子句来查找不匹配的行***

例如，要查找没有下过订单的所有客户，请使用以下查询：

```
SELECT 
    c.customerNumber, 
    c.customerName, 
    orderNumber, 
    o.status
FROM
    customers c
        LEFT JOIN
    orders o ON c.customerNumber = o.customerNumber
WHERE
    orderNumber IS NULL;
--------------------------------- 
SELECT 
	c.customerNumber,
	c.customerName,
	o.orderNumber,
	o.status
FROM
	customers c
			LEFT JOIN
	orders o USING(customerNumber)
WHERE 
	orderNumber IS NULL;
-------------------------------------------------------
+----------------+--------------------------------+-------------+--------+
| customerNumber | customerName                   | orderNumber | status |
+----------------+--------------------------------+-------------+--------+
|            125 | Havel & Zbyszek Co             |        NULL | NULL   |
|            168 | American Souvenirs Inc         |        NULL | NULL   |
|            169 | Porto Imports Co.              |        NULL | NULL   |
|            206 | Asian Shopping Network, Co     |        NULL | NULL   |
|            223 | Natrlich Autos                 |        NULL | NULL   |
|            237 | ANG Resellers                  |        NULL | NULL   |
|            247 | Messner Shopping Network       |        NULL | NULL   |
|            273 | Franken Gifts, Co              |        NULL | NULL   |
...
|            481 | Raanan Stores, Inc             |        NULL | NULL   |
+----------------+--------------------------------+-------------+--------+
24 rows in set (0.01 sec)

```

***WHERE子句与ON子句中的条件***

```
SELECT 
    o.orderNumber, 
    customerNumber, 
    productCode
FROM
    orders o
        LEFT JOIN
    orderDetails USING (orderNumber)
WHERE
    orderNumber = 10123;
    
+-------------+----------------+-------------+
| orderNumber | customerNumber | productCode |
+-------------+----------------+-------------+
|       10123 |            103 | S18_1589    |
|       10123 |            103 | S18_2870    |
|       10123 |            103 | S18_3685    |
|       10123 |            103 | S24_1628    |
+-------------+----------------+-------------+
4 rows in set (0.00 sec)

#######################################################
如果将条件从WHERE子句移动到ON子句：
SELECT 
    o.orderNumber, 
    customerNumber, 
    productCode
FROM
    orders o
        LEFT JOIN
    orderDetails d ON o.orderNumber = d.orderNumber
        AND o.orderNumber = 10123;
+-------------+----------------+-------------+
| orderNumber | customerNumber | productCode |
+-------------+----------------+-------------+
|       10123 |            103 | S18_1589    |
|       10123 |            103 | S18_2870    |
|       10123 |            103 | S18_3685    |
|       10123 |            103 | S24_1628    |
|       10298 |            103 | NULL        |
...
|       10399 |            496 | NULL        |
+-------------+----------------+-------------+
329 rows in set (0.01 sec)


```
>请注意，对于INNER JOIN子句，ON子句中的条件等同于WHERE子句中的条件。

### <span id = "2.4"> CROSS JOIN交叉连接</span>

CROSS JOIN连接两个表,结果集将包括两个表中的所有行，其中结果集中的每一行都是第一个表中的行与第二个表中的行的组合。 当连接的表之间没有关系时，会使用这种情况。
要特别注意的是，如果每个表有1000行，那么结果集中就有1000 x 1000 = 1,000,000行，那么数据量是非常巨大的。

**语法**

```
SELECT 
    *
FROM
    T1
        CROSS JOIN
    T2;
------------------------------------------------------------


SELECT customers.customerName, orders.orderNumber
FROM customers
	CROSS JOIN
orders
ORDER BY orderNumber;
```

```
如果添加了WHERE子句，如果T1和T2有关系，则CROSS JOIN的工作方式与INNER JOIN子句类似，如以下查询所示：
SELECT 
    *
FROM
    T1
        CROSS JOIN
    T2
WHERE
    T1.id = T2.id;

```
### <span id = "2.5"> 自连接</span>

```
employees表
+----------------+-----------+-----------+-----------+-----------------------+------------+-----------+----------------------+
| employeeNumber | lastName  | firstName | extension | email                 | officeCode | reportsTo | jobTitle             |
+----------------+-----------+-----------+-----------+-----------------------+------------+-----------+----------------------+
|           1002 | Murphy    | Diane     | x5800     | dmurphy@yiibai.com    | 1          |      NULL | President            |
|           1056 | Patterson | Mary      | x4611     | mpatterso@yiibai.com  | 1          |      1002 | VP Sales             |
|           1076 | Firrelli  | Jeff      | x9273     | jfirrelli@yiibai.com  | 1          |      1002 | VP Marketing         |
|           1088 | Patterson | William   | x4871     | wpatterson@yiibai.com | 6          |      1056 | Sales Manager (APAC) |
|           1102 | Bondur    | Gerard    | x5408     | gbondur@gmail.com     | 4          |      1056 | Sale Manager (EMEA)  |
|           1143 | Bow       | Anthony   | x5428     | abow@gmail.com        | 1          |      1056 | Sales Manager (NA)   |
|           1165 | Jennings  | Leslie    | x3291     | ljennings@yiibai.com  | 1          |      1143 | Sales Rep            |
|           1166 | Thompson  | Leslie    | x4065     | lthompson@yiibai.com  | 1          |      1143 | Sales Rep            |
|           1188 | Firrelli  | Julie     | x2173     | jfirrelli@yiibai.com  | 2          |      1143 | Sales Rep            |
|           1216 | Patterson | Steve     | x4334     | spatterson@yiibai.com | 2          |      1143 | Sales Rep            |
|           1286 | Tseng     | Foon Yue  | x2248     | ftseng@yiibai.com     | 3          |      1143 | Sales Rep            |
|           1323 | Vanauf    | George    | x4102     | gvanauf@yiibai.com    | 3          |      1143 | Sales Rep            |
|           1337 | Bondur    | Loui      | x6493     | lbondur@yiibai.com    | 4          |      1102 | Sales Rep            |
|           1370 | Hernandez | Gerard    | x2028     | ghernande@gmail.com   | 4          |      1102 | Sales Rep            |
|           1401 | Castillo  | Pamela    | x2759     | pcastillo@gmail.com   | 4          |      1102 | Sales Rep            |
|           1501 | Bott      | Larry     | x2311     | lbott@yiibai.com      | 7          |      1102 | Sales Rep            |
|           1504 | Jones     | Barry     | x102      | bjones@gmail.com      | 7          |      1102 | Sales Rep            |
|           1611 | Fixter    | Andy      | x101      | afixter@yiibai.com    | 6          |      1088 | Sales Rep            |
|           1612 | Marsh     | Peter     | x102      | pmarsh@yiibai.com     | 6          |      1088 | Sales Rep            |
|           1619 | King      | Tom       | x103      | tking@gmail.com       | 6          |      1088 | Sales Rep            |
|           1621 | Nishi     | Mami      | x101      | mnishi@gmail.com      | 5          |      1056 | Sales Rep            |
|           1625 | Kato      | Yoshimi   | x102      | ykato@gmail.com       | 5          |      1621 | Sales Rep            |
|           1702 | Gerard    | Martin    | x2312     | mgerard@gmail.com     | 4          |      1102 | Sales Rep            |
+----------------+-----------+-----------+-----------+-----------------------+------------+-----------+----------------------+

```

要获得整个组织结构，可以使用employeeNumber和reportsTo列将employees表连接自身。employees表有两个角色：一个是经理，另一个是直接报告者(即，下属员工)。

```
SELECT 
    CONCAT(m.lastname, ', ', m.firstname) AS 'Manager',
    CONCAT(e.lastname, ', ', e.firstname) AS 'Direct report'
FROM
    employees e
        INNER JOIN
    employees m ON m.employeeNumber = e.reportsto
ORDER BY manager;

```

在上述输出中，只能看到有经理的员工。 但是，由于INNER JOIN子句，所以看不到总经理。总经理是没有任何经理的员工，或者他的经理人是NULL

```
SELECT 
    IFNULL(CONCAT(m.lastname, ', ', m.firstname),
            'Top Manager') AS 'Manager',
    CONCAT(e.lastname, ', ', e.firstname) AS 'Direct report'
FROM
    employees e
        LEFT JOIN
    employees m ON m.employeeNumber = e.reportsto
ORDER BY manager DESC;

```

### <span id = "2.6"> UNION</span>

>MySQL UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中.多个 SELECT 语句会删除重复的数据。

**语法**

```
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions]
UNION [ALL | DISTINCT]
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions];

-----------------------------------------------------


    expression1, expression2, ... expression_n: 要检索的列。

    tables: 要检索的数据表。

    WHERE conditions: 可选， 检索条件。

    DISTINCT: 可选，删除结果集中重复的数据。默认情况下 UNION 操作符已经删除了重复数据，所以 DISTINCT 修饰符对结果没啥影响。

    ALL: 可选，返回所有结果集，包含重复数据。
    
------------------------------------------------------
SELECT country FROM Websites
UNION
SELECT country FROM apps
ORDER BY country; 

SELECT country FROM Websites
UNION ALL
SELECT country FROM apps
ORDER BY country; 

```

* CROSS JOIN

![CROSS JOIN](https://www.yiibai.com/uploads/images/201707/1807/423180701_88270.png)

* INNER JOIN

![INNER JOIN](https://www.yiibai.com/uploads/images/201707/1807/791200745_93575.png)

* LEFT JOIN

![LEFT JOIN](https://www.yiibai.com/uploads/images/201707/1807/397200753_25587.png)

* RIGHT JOIN

![RIGHT JOIN](https://www.yiibai.com/uploads/images/201707/1807/906210707_20913.png)

* 自连接

单个表内部处理数据

* UNION

多个表混合组成一个查询结果集合

###### 作业 #######

```
项目五：组合两张表 （难度：简单）
在数据库中创建表1和表2，并各插入三行数据（自己造）
表1: Person
+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId 是上表主键

表2: Address
+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId 是上表主键

编写一个 SQL 查询，满足条件：无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：FirstName, LastName, City, State

**************************************************************
创建表
CREATE TABLE Person (PersonId INT(10) NOT NULL PRIMARY KEY,FirstName VARCHAR(20) NULL,SecondName VARCHAR(20) NULL);

CREATE TABLE Address (AddressId INT(10) NOT NULL PRIMARY KEY, PersonId INT(10) NOT NULL, City VARCHAR(10) NULL,State VARCHAR(40) NULL);
插入数据
INSERT INTO Person VALUES(1,'wang','gang'),(2,'xiao','ming'),(3,'li','si');

INSERT INTO Address VALUES(1,2,'北京','朝阳'),(2,3,'石家庄','正定'),(3,1,'郑州','金水');

作业答案:
SELECT 
		p.FirstName,
		p.SecondName,
		a.City,
		a.State
FROM Person p
		LEFT JOIN
	  Address a
ON (p.PersonId = a.PersonId);
或
SELECT 
		p.FirstName,
		p.SecondName,
		City,
		State
FROM Person p
		LEFT JOIN
	  Address
USING(PersonId);

项目六：删除重复的邮箱（难度：简单）
编写一个 SQL 查询，来删除 email 表中所有重复的电子邮箱，重复的邮箱里只保留 Id 最小 的那个。
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
Id 是这个表的主键。
例如，在运行你的查询语句之后，上面的 Person表应返回以下几行:
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | a@b.com |
| 2  | c@d.com  |
+----+------------------+

*********************************************************
创建数据库
CREATE TABLE IF NOT EXISTS email(
	id INT PRIMARY KEY AUTO_INCREMENT,
	Email VARCHAR(30) NOT NULL
);
插入数据
INSERT INTO email VALUES(1,'a@b.com'),(2,'c@d.com'),(3,'a@b.com');

作业参考答案:
DELETE e1
FROM email e1, email e2  
WHERE e1.email = e2.email
AND  
e1.id > e2.id;
```






