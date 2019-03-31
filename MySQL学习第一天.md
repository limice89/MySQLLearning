# MySQL学习第一天
* MySQl安装
* 图形界面软件 Navicat for SQL
* 数据库基础知识
* MySQL数据库管理系统

## MySQL安装
由于使用mac电脑，使用Homebrew 安装MySQL,简单省事，当然你也可以选择去MySQL官网下载软件进行安装  
执行命令
```
brew install mysql
```

进行安装即可，安装比较顺利不再敖述，有出现问题的自行谷歌百度即可		
启动MySQL  `mysql -uroot`

##  Navicat的安装使用
Navicat需要破解使用，当然也可以选择正版，破解思路从网上找的教程
<b>替换RSA加密算法公钥，不过Mac中的公钥放在程序包目录的rpk文件中，文本编辑替换即可</b>  
>
需要的资源、工具    
④Mac版 Navicat Premium 12 v12.0.22.0 官网下载地址：
英文64位 http://download.navicat.com/download/navicat120_premium_en.dmg
中文简体64位 http://download.navicat.com/download/navicat120_premium_cs.dmg
以上资源百度网盘地址：https://pan.baidu.com/s/1dtKfvw  

下载安装好软件以后进行破解操作  
1.生成自己的公钥和私钥
从网上找的公钥和私钥对
公钥：
```
-----BEGIN PUBLIC KEY-----
MIIBITANBgkqhkiG9w0BAQEFAAOCAQ4AMIIBCQKCAQB8vXG0ImYhLHvHhpi5FS3g
d2QhxSQiU6dQ04F1OHB0yRRQ3NXF5py2NNDw962i4WP1zpUOHh94/mg/KA8KHNJX
HtQVLXMRms+chomsQCwkDi2jbgUa4jRFN/6N3QejJ42jHasY3MJfALcnHCY3KDEF
h0N89FV4yGLyDLr+TLqpRecg9pkPnOp++UTSsxz/e0ONlPYrra/DiaBjsleAESZS
I69sPD9xZRt+EciXVQfybI/2SYeAdXMm1B7tHCcFlOxeUgqYV03VEqiC0jVMwRCd
+03NU3wvEmLBvGOmNGudocWIF/y3VOqyW1byXFLeZxl7s+Y/SthxOYXzu3mF+2/p
AgMBAAE=
-----END PUBLIC KEY-----
```
私钥：
```
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQB8vXG0ImYhLHvHhpi5FS3gd2QhxSQiU6dQ04F1OHB0yRRQ3NXF
5py2NNDw962i4WP1zpUOHh94/mg/KA8KHNJXHtQVLXMRms+chomsQCwkDi2jbgUa
4jRFN/6N3QejJ42jHasY3MJfALcnHCY3KDEFh0N89FV4yGLyDLr+TLqpRecg9pkP
nOp++UTSsxz/e0ONlPYrra/DiaBjsleAESZSI69sPD9xZRt+EciXVQfybI/2SYeA
dXMm1B7tHCcFlOxeUgqYV03VEqiC0jVMwRCd+03NU3wvEmLBvGOmNGudocWIF/y3
VOqyW1byXFLeZxl7s+Y/SthxOYXzu3mF+2/pAgMBAAECggEAK5qZbYt8wenn1uZg
6onRwJ5bfUaJjApL+YAFx/ETtm83z9ByVbx4WWT7CNC7fK1nINy20/mJrOTZkgIx
x6otiNC4+DIsACJqol+RLoo8I9pk77Ucybn65ZteOz7hVZIU+8j6LzW0KDt6yowX
e75r7G/NEpfibNc3Zz81+oDd2x+bHyGbzc9QcePIVuEzkof6jgpbWrQZU14itx9l
VxEgj/fbMccvBx8brR/l9ClmDZd9Y6TWsF1rfJpF3+DPeqFkKCiD7PGz3bs4O/Zd
ZrfV21ZNVusBW49G6bU63gQVKsOf1qGo3efbAW1HVxgTQ/lExVdcMvdenZm+ADKp
L4/wUQKBgQDOfBjn3OC2IerUFu18EgCS7pSjTSibXw+TeX3D5zwszLC091G2rGlT
5DihBUhMfesNdpoZynrs4YB6Sz9C3wSGAB8AM/tNvPhtSVtbMHmrdT2DEEKCvLkO
RNBnt+8aTu2hGRanw9aL1189gzwrmXK5ZuuURfgLrB9ihrvjo4VznQKBgQCapx13
dEA1MwapBiIa3k8hVBCoGPsEPWqM33RBdUqUsP33f9/PCx00j/akwmjgQNnBlAJo
Y7LOqPCyiwOkEf40T4IlHdzYntWQQvHhfBwqSgdkTE9tKj43Ddr7JVFRL6yMSbW3
9qAp5UX/+VzOLGAlfzJ8CBnkXwGrnKPCVbnZvQKBgQCd+iof80jlcCu3GteVrjxM
LkcAbb8cqG1FWpVTNe4/JFgqDHKzPVPUgG6nG2CGTWxxv4UFKHpGE/11E28SHYjb
cOpHAH5LqsGy84X2za649JkcVmtclUFMXm/Ietxvl2WNdKF1t4rFMQFIEckOXnd8
y/Z/Wcz+OTFF82l7L5ehrQKBgFXl9m7v6e3ijpN5LZ5A1jDL0Yicf2fmePUP9DGb
ZTZbbGR46SXFpY4ZXEQ9GyVbv9dOT1wN7DXvDeoNXpNVzxzdAIt/H7hN2I8NL+4v
EjHG9n4WCJO4v9+yWWvfWWA/m5Y8JqusV1+N0iiQJ6T4btrE4JSVp1P6FSJtmWOK
W/T9AoGAcMhPMCL+N+AvWcYt4Y4mhelvDG8e/Jj4U+lwS3g7YmuQuYx7h5tjrS33
w4o20g/3XudPMJHhA3z+d8b3GaVM3ZtcRM3+Rvk+zSOcGSwn3yDy4NYlv9bdUj/4
H+aU1Qu1ZYojFM1Gmbe4HeYDOzRsJ5BhNrrV12h27JWkiRJ4F/Q=
-----END RSA PRIVATE KEY-----
```
2.安装程序，替换应用程序目录中rpk文件的公钥
安装完毕后打开finder,从应用程序中找到应用，右击显示包内容，打开目录、COntent/Resources,编辑rpk文件，将公钥替换保存
![包内容目录](https://github.com/limice89/MySQLLearning/blob/master/images/navicatrpk.png?raw=true)
3.算出有限mac版序列号秘钥（直接从网上找的序列号）
中文版64位密钥序列号： NAVH-T4PX-WT8W-QBL5
英文版64位密钥序列号： NAVG-UJ8Z-EVAP-JAUW

4.解密请求码生成激活码  
①打开应用，断网！！！，点击注册，输入密钥 NAVH-T4PX-WT8W-QBL5（英文版用另一个），然后手动激活

②复制请求码，使用私钥解密请求码，得到激活码明文
注意必须自己解密，因为解密后得到的“DI”是不同的
激活码明文示例

```
{
  "K" : "NAVHT4PXWT8WQBL5",
  "P" : "Mac 10.13",
  "DI" : "ODQ2Yjg2ZDBjMTEzMjhh"
}
```



在线RSA私钥解密：http://tool.chacuo.net/cryptrsaprikey

③将得到的激活码明文进行修改，修改后格式如下

```
{"K":"NAVHT4PXWT8WQBL5", "N":"52limice", "O":"52limice", "DI":"NjhmZjRlZDk1OGUyMGM5", "T":1553998641}

```

激活码明文格式最好复制我的，改变 " " 内的字符即可，在同一行哦，不要换行，否则激活失败！！！
"K" "DI" 都替换成自己机器解密的信息，"N" "O" 自己定义"T"为时间戳

④加密激活码明文，使用私钥加密激活码明文
在线RSA私钥加密：http://tool.chacuo.net/cryptrsaprikey

⑤复制加密后激活码信息到程序激活窗口，点击激活，即可成功激活

然后就可以使用了Navicat,是不是很方便，嘻嘻

##数据库基础知识

一、数据库定义    
数据库（Database）是按照数据结构来组织、存储和管理数据的仓库。
每个数据库都有一个或多个不同的 API 用于创建，访问，管理，搜索和复制所保存的数据。    
数据库是一些关联表的集合。

二、关系型数据库

关系型数据库：指采用了关系模型来组织数据的数据库。
关系模型指的就是二维表格模型，而一个关系型数据库就是由二维表及其之间的联系所组成的一个数据组织。

关系数据库的特点    
1.数据以表格的形式出现。
2.每行为各种记录名称。
3.每列为记录名称所对应的数据域。
4.许多的行和列组成一张表单。
5.若干的表单组成database。

三、二维表

在关系模型中，数据结构表示为一个二维表，一个关系就是一个二维表（但不是任意一个二维表都能表示一个关系，例如两个元组的候选键相同即表中的任意两行相同，就不能表示一个关系。），二维表名就是关系名。表中的第一行通常称为属性名，表中的每一个元组和属性都是不可再分的，且元组的次序是无关紧要的。


四、行

表中的一个记录。 表中数据按行存储，每个记录存储在自己的行内

五、列

表中的一列包含相同的数据，比如个人信息的姓名

六、主键

表中每一行都应该有一列或者几列可以唯一标识自己，比如编号ID,其值能够唯一标识表中的每一行

七、外键

外键（FK）是用于建立或加强两个表数据之间的链接的一列或多列。

```
学生表(学号，姓名，性别，班级)   　　学号是一个主键

　课程表(课程号，课程名，学分)　　　　课程号是一个主键

　成绩表(学号，课程号，成绩)　　　　　学号和课程号的属性组构成一个主键

　成绩表中的学号不是成绩表的主键，不过是学生表的主键，成绩表的外键，同理课程号也是成绩表的外键
　
```
##MySQL数据库管理系统
一、数据库
二、数据表
三、视图
四、存储过程



