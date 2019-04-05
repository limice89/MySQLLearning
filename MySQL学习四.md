# MySQL学习四

## 数据导入

**1.** mysql命令导入

```
mysql -u用户名 -p密码 <要导入的数据库数据(*.sql)
eg:
$mysql -uroot -p12345678 <student.sql
```

**2.** source 命令导入

source 命令导入数据库需要先登录到数库终端：

```
mysql> CREATE DATABASE Study;   #创建数据库
mysql> use Study;               #使用已创建的数据库
mysql> set names utf8;          #设置编码
mysql> source /user/desktop/db/Study.sql  #导入备份数据库
```

**3.** 使用 LOAD DATA导入数据

从当前目录中读取文件 dump.txt ，将该文件中的数据插入到当前数据库的 mytbl 表中。 

```
mysql> LOAD DATA LOCAL INFILE 'dump.txt' INTO TABLE mytbl;
```
如果指定LOCAL关键词，则表明从客户主机上按路径读取文件。如果没有指定，则文件在服务器上按路径读取文件。

你能明确地在LOAD DATA语句中指出列值的分隔符和行尾标记，但是默认标记是定位符和换行符。

两个命令的 FIELDS 和 LINES 子句的语法是一样的。两个子句都是可选的，但是如果两个同时被指定，FIELDS 子句必须出现在 LINES 子句之前。

如果用户指定一个 FIELDS 子句，它的子句 （TERMINATED BY、[OPTIONALLY] ENCLOSED BY 和 ESCAPED BY) 也是可选的，不过，用户必须至少指定它们中的一个。

```
mysql> LOAD DATA LOCAL INFILE 'dump.txt' INTO TABLE mytbl
  -> FIELDS TERMINATED BY ':'
  -> LINES TERMINATED BY '\r\n';
```

LOAD DATA 默认情况下是按照数据文件中列的顺序插入数据的，如果数据文件中的列与插入表中的列不一致，则需要指定列的顺序:

```
mysql> LOAD DATA LOCAL INFILE 'dump.txt' 
    -> INTO TABLE mytbl (b, c, a);
```

**4.** 使用 mysqlimport 导入数据

 mysqlimport客户端提供了LOAD DATA INFILEQL语句的一个命令行接口。mysqlimport的大多数选项直接对应LOAD DATA INFILE子句。

从文件 dump.txt 中将数据导入到 mytbl 数据表中, 可以使用以下命令:

```
$ mysqlimport -u root -p --local database_name dump.txt
password *****
```
mysqlimport命令可以指定选项来设置指定格式,命令语句格式如下:

```
$ mysqlimport -u root -p --local --fields-terminated-by=":" \
   --lines-terminated-by="\r\n"  database_name dump.txt
password *****
```

mysqlimport 语句中使用 --columns 选项来设置列的顺序：

```
$ mysqlimport -u root -p --local --columns=b,c,a \
    database_name dump.txt
password *****
```

### **mysqlimport的常用选项介绍**

选项|	功能
---|---
-d or --delete|	新数据导入数据表中之前删除数据数据表中的所有信息
-f or --force |	不管是否遇到错误，mysqlimport将强制继续插入数据
-i or --ignore |	mysqlimport跳过或者忽略那些有相同唯一 关键字的行， 导入文件中的数据将被忽略。
-l or -lock-tables 	|数据被插入之前锁住表，这样就防止了， 你在更新数据库时，用户的查询和更新受到影响。
-r or -replace |	这个选项与－i选项的作用相反；此选项将替代 表中有相同唯一关键字的记录。
--fields-enclosed- by= char	|指定文本文件中数据的记录时以什么括起的， 很多情况下 数据以双引号括起。 默认的情况下数据是没有被字符括起的。
--fields-terminated- by=char|	指定各个数据的值之间的分隔符，在句号分隔的文件中， 分隔符是句号。您可以用此选项指定数据之间的分隔符。 默认的分隔符是跳格符（Tab）
--lines-terminated- by=str	|此选项指定文本文件中行与行之间数据的分隔字符串 或者字符。 默认的情况下mysqlimport以newline为行分隔符。 您可以选择用一个字符串来替代一个单个的字符： 一个新行或者一个回车。


## 数据库导出

 **1.** 使用 SELECT ... INTO OUTFILE 语句导出数据
 
 以下实例中我们将数据表 runoob_tbl 数据导出到 /tmp/runoob.txt 文件中:

```
mysql> SELECT * FROM runoob_tbl 
    -> INTO OUTFILE '/tmp/runoob.txt';
```

可以通过命令选项来设置数据输出的指定格式，以下实例为导出 CSV 格式:

```
mysql> SELECT * FROM passwd INTO OUTFILE '/tmp/runoob.txt'
    -> FIELDS TERMINATED BY ',' ENCLOSED BY '"'
    -> LINES TERMINATED BY '\r\n';
```

**2.** 导出表作为原始数据

 ***`mysqldump`*** 是 mysql 用于转存储数据库的实用程序。它主要产生一个 SQL 脚本，其中包含从头重新创建数据库所必需的命令 CREATE TABLE INSERT 等。

使用 **`mysqldump`** 导出数据需要使用 `--tab` 选项来指定导出文件指定的目录，该目标必须是可写的

```
$ mysqldump -u root -p --no-create-info --tab=/Users/admin/Desktop/temp yiibaidb customers
Enter password: *****
mysqldump: Got error: 1290: The MySQL server is running with the --secure-file-priv option so it cannot execute this statement when executing 'SELECT INTO OUTFILE'
报错 从网上找解决办法 是mysql没有配置
连接mysql
mysql> show variable like '%secure%';
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'variable like '%secure%'' at line 1
mysql> show variables like '%secure%';
+--------------------------+-------+
| Variable_name            | Value |
+--------------------------+-------+
| require_secure_transport | OFF   |
| secure_file_priv         | NULL  |
+--------------------------+-------+
2 rows in set (0.01 sec)
secure_file_priv 为 NULL 时，表示限制mysqld不允许导入或导出
secure_file_priv 为 /tmp时，表示限制mysqld只能在/tmp目录中执行导入导出，其他目录不能执行
secure_file_priv 没有值时，表示不限制mysqld在任意目录的导入导出

没有配置 secure_file_priv
找到 my.conf
首先需要知道系统是按如下顺序去找my.cnf：

i.    /etc/my.cnf
ii.   /etc/mysql/my.cnf
iii.  /usr/local/etc/my.cnf
iv.  ~/.my.cnf

修改my.cnf 增加
secure_file_priv = "/Users/admin/Documents/mysql/"

重启数据库 配置成功 


执行:
$ mysqldump -u root -p --no-create-info --tab=/Users/admin/Documents/mysql yiibaidb customers

```

**3.** 导出SQL格式的数据

```
$ mysqldump -u root -p RUNOOB runoob_tbl > dump.txt
password ******
```


## 作业

```
#作业#
项目七: 各部门工资最高的员工（难度：中等）
创建Employee 表，包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id。
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
创建Department 表，包含公司所有部门的信息。
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
编写一个 SQL 查询，找出每个部门工资最高的员工。例如，根据上述给定的表格，Max 在 IT 部门有最高工资，Henry 在 Sales 部门有最高工资。
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+

******************************************************
创建数据库表
CREATE TABLE Employee(
	Id INT(10) NOT NULL PRIMARY KEY,
	Name VARCHAR(20),
	Salary VARCHAR(20),
	DepartmentId INT(10)
);
CREATE TABLE Department(
	Id INT(10) NOT NULL PRIMARY KEY,
	Name VARCHAR(20)
);
插入数据
INSERT INTO Employee(
	Id,Name,Salary,DepartmentId
)VALUES(1,'Joe','70000',1),
		(2,'Henry','80000',2),
		(3,'Sam','60000',2),
		(4,'Max','90000',1);
INSERT INTO Department(
	Id,Name
)VALUES(1,'IT'),
		(2,'Sales');
参考查询语句:
SELECT d.Name AS Department,
		e.Name AS Employee,
		e.Salary
FROM Employee AS e
LEFT JOIN 
	  Department AS d ON e.DepartmentId = d.Id
WHERE (e.DepartmentId, e.Salary) IN (SELECT e1.DepartmentId,MAX(e1.Salary) AS Salary FROM Employee e1 GROUP BY e1.DepartmentId);

```

```
项目八: 换座位（难度：中等）
小美是一所中学的信息科技老师，她有一张 seat 座位表，平时用来储存学生名字和与他们相对应的座位 id。
其中纵列的 id 是连续递增的
小美想改变相邻俩学生的座位。
你能不能帮她写一个 SQL query 来输出小美想要的结果呢？
 请创建如下所示seat表：
示例：
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
假如数据输入的是上表，则输出结果如下：
+---------+---------+
|    id   | student |
+---------+---------+8
|    1    | Doris   |
|    2    | Abbot  |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+
注意：
如果学生人数是奇数，则不需要改变最后一个同学的座位。

****************************************************
创建数据库表
CREATE TABLE seat(
 	id INT(10) NOT NULL PRIMARY KEY, 
 	student VARCHAR(10) 
 ); 
 插入数据
INSERT INTO seat(id,student) 
VALUES
	(1,"Abbot"),
	(2,"Doris"),
	(3,"Emerson"),
	(4,"Green"),
	(5,"Jeames");
参考查询语句:

```

```
项目九:  分数排名（难度：中等）
编写一个 SQL 查询来实现分数排名。如果两个分数相同，则两个分数排名（Rank）相同。请注意，平分后的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有“间隔”。
创建以下score表：
+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
例如，根据上述给定的 Scores 表，你的查询应该返回（按分数从高到低排列）：
+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |
+-------+------+

*******************************************************
```
创建数据库表
CREATE TABLE score(
 	Id INT(10) NOT NULL PRIMARY KEY, 
 	Score FLOAT(4) 
 ); 
 插入数据
INSERT INTO score(Id,Score) 
VALUES
	(1,3.50),
	(2,3.65),
	(3,4.00),
	(4,3.85),
	(5,4.00),
	(6,3.65);
参考查询语句:

```
项目十：行程和用户（难度：困难）
Trips 表中存所有出租车的行程信息。每段行程有唯一键 Id，Client_Id 和 Driver_Id 是 Users 表中 Users_Id 的外键。Status 是枚举类型，枚举成员为 (‘completed’, ‘cancelled_by_driver’, ‘cancelled_by_client’)。
+----+-----------+-----------+---------+--------------------+----------+
| Id | Client_Id | Driver_Id | City_Id |        Status      |Request_at|
+----+-----------+-----------+---------+--------------------+----------+
| 1  |     1     |    10     |    1    |     completed      |2013-10-01|
| 2  |     2     |    11     |    1    | cancelled_by_driver|2013-10-01|
| 3  |     3     |    12     |    6    |     completed      |2013-10-01|
| 4  |     4     |    13     |    6    | cancelled_by_client|2013-10-01|
| 5  |     1     |    10     |    1    |     completed      |2013-10-02|
| 6  |     2     |    11     |    6    |     completed      |2013-10-02|
| 7  |     3     |    12     |    6    |     completed      |2013-10-02|
| 8  |     2     |    12     |    12   |     completed      |2013-10-03|
| 9  |     3     |    10     |    12   |     completed      |2013-10-03| 
| 10 |     4     |    13     |    12   | cancelled_by_driver|2013-10-03|
+----+-----------+-----------+---------+--------------------+----------+
Users 表存所有用户。每个用户有唯一键 Users_Id。Banned 表示这个用户是否被禁止，Role 则是一个表示（‘client’, ‘driver’, ‘partner’）的枚举类型。
+----------+--------+--------+
| Users_Id | Banned |  Role  |
+----------+--------+--------+
|    1     |   No   | client |
|    2     |   Yes  | client |
|    3     |   No   | client |
|    4     |   No   | client |
|    10    |   No   | driver |
|    11    |   No   | driver |
|    12    |   No   | driver |
|    13    |   No   | driver |
+----------+--------+--------+
写一段 SQL 语句查出 2013年10月1日 至 2013年10月3日 期间非禁止用户的取消率。基于上表，你的 SQL 语句应返回如下结果，取消率（Cancellation Rate）保留两位小数。
+------------+-------------------+
|     Day    | Cancellation Rate |
+------------+-------------------+
| 2013-10-01 |       0.33        |
| 2013-10-02 |       0.00        |
| 2013-10-03 |       0.50        |
+------------+-------------------+

*******************************************************
创建数据库表:
CREATE TABLE trips(
    Id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    Client_Id INT NOT NULL,
    Driver_Id INT NOT NULL,
    City_Id INT NOT NULL,
    Status ENUM('completed', 'cancelled_by_driver', 'cancelled_by_client') NOT NULL,
    Request_at DATE DEFAULT NULL);

CREATE TABLE Users(
    Users_Id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    Banned VARCHAR(10) NOT NULL,
    Role ENUM('client', 'driver', 'partnet') NOT NULL);

插入数据:

INSERT INTO trips
     VALUES (1,1,10,1,'completed','2013-10-01'),
             (2,2,11,1, 'cancelled_by_driver','2013-10-01'),
             (3,3,12,6,'completed','2013-10-01'),
             (4,4,13,6,'cancelled_by_client','2013-10-01'),
             (5,1,10,1,'completed','2013-10-02'),
             (6,2,11,6,'completed','2013-10-02'),
             (7,3,12,6,'completed','2013-10-02'),
             (8,2,12,12,'completed','2013-10-03'),
             (9,3,10,12,'completed','2013-10-03'),
             (10,4,13,12, 'cancelled_by_driver','2013-10-03');

INSERT INTO Users
    VALUES (1, 'No', 'client'),
            (2, 'Yes', 'client'),
            (3, 'No', 'client'),
            (4, 'No', 'client'),
            (10, 'No', 'driver'),
            (11, 'No', 'driver'),
            (12, 'No', 'driver'),
            (13, 'No', 'driver');
```