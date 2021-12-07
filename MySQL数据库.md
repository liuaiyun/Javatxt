# MySQL数据库

## 一、什么是数据库

数据库（Database）是按照数据结构来组织、存储和管理数据的仓库。（DB）

每个数据库都有一个或多个不同的API用于创建，访问，管理，搜索和复制所保存的数据。

我们也可以将数据存储在文件中，但是在文件中读写数据速度相对较慢。

所以，现在我们使用关系型数据库管理系统（RDBMS）来存储和管理大数据量，所谓的关系型数据库，是建立在关系模型基础上的数据库，借助于集合代数等数学概念和方法来处理数据库中的数据。

RDBMS即关系数据库管理系统(Relational Database Management System)的特点：

​	**1.数据以表格的形式出现**

​	**2.每行为各种记录名称**

​	**3.每列为记录名称所对应的数据域**

​	**4.许多的行和列组成一张表单**

​	**5.若干的表单组成DataBase**

## 二、RDBMS术语



​	**数据库：数据库是一些关联表的集合。**

​	**数据表：表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。**

​	**列：一列（数据元素）包含了相同类型的数据，例如邮政编码的数据。**

​	**行：一行（=元组，或记录）是一组相关的数据，例如一条用户订阅的数据。**

​	**冗（rong）余：存储两倍数据，冗余降低了性能，但提高了数据的安全性。**

​	**主键：主键是唯一的。一个数据表中只能包含一个主键。可以使用主键来查询数据。**

​	**外键：外键用于关联两个表。**

​	**符合键：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。**

​	**索引：使用索引可快速访问数据库表中的特定信息。索引是数据库表中一列或多列的值进行排序的一种结构，类似于书籍的目录。**

​	**参照完整性：参照的完整性要求关系中不允许引用不存在的实体，与实体完整性是关系模型必须满足的完整性约束条件，目的是保证数据的一致性。**

MySQL为关系型数据库(Relational Database Management System)，这种所谓的“关系型”可以理解为“表格”的概念，一个关系型数据库由一个或数个表格组成。

​	**表头（header）：每一列的名称；**

​	**列（col）：具有相同数据类型的集合；**

​	**行（row）：每一行用来描述某条记录的具体信息；**

​	**值（value）：行的具体信息，每个值都必须与该列的数据类型相同。**

​	**键（key）：键的值在当前列中具有唯一性。**

## 三、MySQL数据库



MySQL是一个关系型数据库管理系统，由瑞典MySQL AB公司开发，目前属于Oracle公司。MySQL是一种关联数据库管理系统，关联数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库中，这样就增加了速度并提高了灵活性。

MySQL是开源的，目前隶属于Oracle旗下产品。

MySQL支持大型的数据库。可以处理拥有上千万条记录的大型数据库。

MySQL使用标准的SQL数据语言形式。

MySQL可以运行于多个系统上，并且支持多种语言，这些编程包括C、C++、Python、Java、Perl、PHP、Eiffel、Ruby 和 Tcl 等。

MySQL对PHP有很好的支持，PHP是目前最流行的Web开发语言。

MySQL支持大型数据库，支持5000万条记录的数据仓库，32位系统表文件最大可支持4GB，64位系统支持最大的表文件为8TB。

MySQL是可以定制的，采用了GPL协议，你可以修改源码来开发自己的MySQL系统。

## 四、MySQL软件

1.安装（s）

2.卸载

​	1.去mysql的安装目录找到my.ini文件

​		复制datadir=“C:/ProgramData/MySQL/MySQL Server 5.5/Data/"

​	2.卸载MySQL

​	3，删除C:/ProgramData目录下的MySQL文件夹。

3.配置

​	MySQL服务启动

​		1.手动

​		2，cmd-->services.msc 打开服务的窗口

​		3.使用管理员打开cmd

​			**net start mysql：启动mysql的服务**

​			**net stop mysql：关闭mysql的服务**

MySQL登录

​	1.mysql -uroot -p密码

​	2.mysql -hip -uroot -mysql：启动mysql的服务

​	3.mysql --host=ip --user=root --password=连接目标的密码

MySQL的退出

​		1.exit

​		2.quit

MySQL目录结构

​	1.MySQL安装目录：basedir="D:/develop/MySQL/"

​		配置文件my.ini

​	2.MySQL数据目录：daytadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"

​		几个概念：

​			**数据库：文件夹**

​			**表：文件**

​			**数据：数据**

### 数据库的备份与还原

1.命令行

​	语法：

​		备份：mysqldump -u用户名 -p密码 数据库名称 > 保存的路径

​		还原：

​				1.登录数据库

​				2.创建数据库

​				3.使用数据库

​				4.执行文件。source文件路径

2.图形化工具：

-------------------------------------

## 五、什么是SQL？

Structure Query Language（结构化查询语言）简称SQL，它被美国国家标准局（ANSI）确定为关系型数据库语言的美国标准，后被国际化标准组织（ISO）采纳为关系数据库语言的国际标准。**数据库管理系统可以通过SQL管理系统库；定义和操作数据，维护数据的完整性和安全性。**

#### 优点：

1.简单易学，具有很强的操作性。

2.绝大多数重要的数据库管理系统均支持SQL。

3.高度非过程化；用SQL操作数据库时大部分的工作由DBMS自动完成。

#### 分类：

1.DDL（Data Definition Language）数据定义语言，用来操作数据库、表、列等；常用语句：CREATE、ALTER、DROP。

2.DML(Data Manipulation Language)数据操作语言，用来操作数据库中表里的数据；常用语句：INSERT、UPDATE、DAELETE。

3.DCL（Data Control Language）数据控制语言，用来操作访问权限和安全级别；常用语句：GRANT、DENY。

4.DQL（Data Query Language）数据查询语言，用来查询数据；常用语句：SELECT。

## 六、数据库的三大范式

**1.第一范式（1NF）**是指数据库表的每一列都是不可分割的基本数据线（每列的值具有原子性，不可再分割）。

**2.第二范式（2NF）**（首先要满足第一范式）。如果表示单主键，那么主键以外的列必须完全依赖于主键；如果表是复合主键，那么主键以外的列必须完全依赖于主键，不能仅依赖主键的一部分。

**3.第三范式（3NF）**（首先要满足第二范式）。表中的非主键列必须和主键直接相关而不能间接相关（非主键列之间不能相关依赖）。

## 七、数据库的数据类型

### 1.整数类型

MySQL中的整数类型分为五种，分别是TINYINT、SMALLINT、MEDIUMINT、INt和BIGINT。

数据类型		字节数		无符号数的取值范围								有符号数的取值范围

TINYINT			1						0-255													-128-127

SMALLINT		2						0-65535											-32768-32768

MEDIUMINT	3						0-16777215									-8388608-8388608

INT					4						0-4294967295									-2147483648-2147483648

BIGINT			5						0~18446744073709551615			-9223372036854775808~9223372036854775808

### 2.浮点型和定点数类型

单浮点（FLOAT）双浮点（DOUBLE）定点数类型（DECIMAL）

FLoat	4

DOUBLE	8

MECIMAL（M，D）	M+2【M表示数据的长度，D表示小数点后的长度】

### 3.字符串类型

字符串：（CHAR）（VARCHAR）		VARCHAR存储可变长度的字符串

CHAR（M）字节数M

VARCHAR（M）字节数M+1

### 4.文本类型

文本类型-->表示大文本数据（文章内容、评论、详情）四种:

TINYTEXT				0-255字节	tinytext

TEXT						0-65535字节

MEDIUMTEXT		0-16777215字节	mediumtext

LONGTEXT				0-4294967295字节

### 5.日期与时间类型

MySQL：YEAR、DATE、TIME、DATETIME、TIMESTAMP

YEAR				1			1901~2155																				YYYY										0000（零值）

DATE				4			1000-01-01~9999-12-31													YYYY-MM-DD							0000-00-0

TIME				3			-828:59~838:59:59																	HH:MM:SS							00:00:00

DATETIME		8			1000-01-01 00:00:00~9999-12-31 23:59:59			YYYY-MM-DD HH:MM:SS			0000-00-00	00:00:00

TIMESTAMP	4			1970-01-01 00:00:01~2038-01-19 03:14:07			YYYY-MM-DD HH:MM:SS				0000-00-00 00:00:00

#### 5.1YEAR类型

YEAR--年份		在MySQL中有三种格式

1.使用四位字符串或数字表示（1901-2155或’1901‘-’2155‘），输入或插入该范围数到数据库的值均相同

2.使用两位字符串表示（’00’-‘99’ 其中‘00’-‘69’范围-->2000-2069	‘70’-‘99’-->1970-1999）

3.使用两位数字表示（1-99	1-69-->2001-2069	70-99-->1970-1999）

**注：一定要区分‘0’和0	‘0’--2000	0--0000**

#### 5.2TIME类型

三种格式

1.以‘D HH:DD:SS'字符串格式表示（D【0-34】	小时值 = D×24+HH） ’2 11:30:50'-->59:30:50

2.‘HHMMSS’字符串或者HHMMSS数字表示。

3.使用CURRENT_TIME或NOW()输入当前系统时间。

#### 5.3DATETIME类型

DATETIME四种格式

1.‘YYYY-MM-DD HH:DD:SS'或’YYYYMMDDHHMMSS‘字符串格式表示日期或时间。

2.’YY-MM-DD HH:MM:SS'或‘YYMMDDHHMMSS'字符串格式表示日期和时间（YY 【’00-69‘-->2000-2069	'70'-'99'-->1970-1999】）。

3.YYYYMMDDHHMMSS或者YYMMDDHHMMSS数字格式表示日期或时间。

4.使用NOW()来输入当前系统的日期和时间

#### 5.4TIMESTAMP类型

有几种TIMESTAMP与DATETIME不同的形式：

1.使用CURRENT_TIMESTAMP输入系统当前日期和时间。

2.输入NULL时系统会输入当前日期和时间。

3.无任何输入时系统会输入系统当前日期和时间。

### 6.二进制类型

MySQL常用BLOB存储二进制类型数据（tp、PDF文档等）

TINYBLOB（0-255字节）

BLOB（0-65535字节）

MEDIUMBLOB（0-16777215字节）

LONGBLOB（0-4294967295字节）

## 八、数据库、数据表的基本操作

### 1.数据库的基本操作

创建一个数据库	creat database 数据库名称；

创建一个叫db1的数据库MySQL命令	show creat database db1；

删除MySQL命令	drop database db1；

查询出MySQL中所有的数据库MySQL命令	show database；

将数据库的字符集修改为gbk MySQL命令	alter database db1 character set gbk；

切换数据库MySQL命令	use db1；

查看当前使用的数据库MySQL命令	select database();

### 2.数据表的基本操作

在操作数据表之前应该使用“USE数据库名”，指定操作在哪个数据库中进行相关操作，否则会出现“No database selected”错误。

creat table 表名(

​			字段1 字段类型，

​			字段2 字段类型，

​			……

​			字段n 字段类型

)；

#### 2.1创建数据表

创建学生表MySQL命令

creat table student(

id int,

name varchar(20),

gender varchar(10),

birthday date

);

#### 2.2查看数据表

1.查看当前数据库所有表MySQL命令

show tables；

2.查表得基本信息MySQL命令

show creat table student；

3.查看表的字段信息MySQL命令

desc student；

#### 2.3修改数据表

1.修改表名MySQL命令

alter table student rename to stu；

2.修改字段名MySQL命令

alter table stu change name sname varchar(10);

3.修改字段数据类型MySQL命令

alter table stu modify sname in；

4.增加字段MySQL命令

alter table stu add address varchar(50);

5.删除字段MySQL命令

alter table stu drop address；

#### 2.4删除数据表

语法	drop table 表名；

## 九.数据表的约束

防止错误的数据被插入数据表，MySQL中定义了一些维护数据库完整的规则；被称为表的约束。

约束条件+说明：

PRIMARY KEY			主键约束用于唯一标识对应的记录

FOREIGN KEY							外键约束

NOT NULL								非空约束

UNIQUE									唯一性约束

DEFAULT					默认值约束，用于设置字段的默认值

保证了表中数据的正确性和唯一性。

### 1.主键约束

primary key用于唯一的标识表中的每一行，被标识为主键的数据在表中是唯一的且值不能为空（类似于身份证号）

主键约束基本语法	字段名 数据类型 primary key；

设置主键约束的第一种方式

例：MySQL命令

creat table student(

id int primary key,

name varchar(20)

);

设置主键约束的第二种方式

例：MySQL命令

creat table student(

id int,

name varchar(20),

primary key(id)

);

### 2.非空约束

非空约束即NOT NULL指的字段值不能为空，基本语法：

字段名 数据类型 NOT NULL；

例：MySQL命令

creat table student(

id int,

name varchar(20) NOT NULL

);

### 3.默认值约束

默认值约束即DEFAULT用于给数据表中的字段指定默认值，即当在表中插入一条新纪录未给该字段赋值，那么，数据库系统会自动为这个字段赋值，基本语法：

字段名 数据类型 DEFAULT 默认值；

例：MySQL命令

creat table student(

id int,

name varchar(20),

gender varchar(10) default 'male'

);

### 4.外键约束

外键约束即FOREIGN KEY常用于多张表之间的约束，基本语法：

--在创建数据表时语法如下：

CONSTRAINT 外键名 FOREIGN KEY（从表外键字段）REFRENCES 主表（主键字段）

--将创建数据表创好后语法如下：

ALTER TABLE 从表名 ADD CONSTRAINT 外键名  FOREIGN KEY（从表外键字段）REFERENCES  主表（主键字段）；

例：创建一个学生表MySQL命令

creat table MySQL student(

id int primary key,

name varchar(20)

);

创建一个班级表MySQL命令

creat table class(

classid int primary key,

studentid int

);

学生表作为zhu'biao班级表作为副表设置外键，MySQL命令

alter table class add constraint fk_class_studentid foreign key(studentid) references student(id);

#### 4.1数据一致性概念

建立外键是为了保证数据的完整性和统一性，若主表中的数据被删除或修改，从表中对应的数据也应该被删除，否则数据库中存在太多垃圾数据。

#### 4.2删除外键

语法如下：

alter table 从表名 drop foreign key 外键名；

例：删除外键MySQL命令

alter table class drop doreign key fk_class_studentid;

#### 4.3关于外键约束需要注意的细节

1.从表里的外键通常为主表的主键

2.从表里外键的数据类型必须与主表中主键的数据类型一致

3.主表发生变化时应注意主表与从表的数据一致性问题

### 5.唯一性约束

唯一性约束即UNIQUE用于保证数据表中字段的唯一性，即表中字段的值不能重复出现，基本语法：

字段名 数据类型 UNIQUE；

例：MySQL命令

creat table student(

id int,

name varchar(20) unique

);

## 十、数据表插入数据

在MySQL通过INSERT语句向数据表中插入数据，先准备一张学生表如下

creat table student(

id int,

name varchar(30),

age int,

gender varchar(30)

);

### 1.为表中所有字段插入数据

每个字段与其值都是严格一一对应的（每个值、值的顺序、值的类型必须与对应的字段相匹配）

各字段无须与其在表中定义的顺序一致，它们只要与VALUES中值一致即可

语法：

INSERT INTO 表名（字段名1，字段名2，……） VALUES（值1，值2……）；

例：

insert into student(id,name,age,gender) values(1,'bob',16,'male');

### 2.为表中指定字段插入数据

语法：

INSERT INTO 表名（字段名1，字段名2，……） VALUES（值1，值2，……）；

3.同时插入多条记录

语法：

INSERT INTO 表名 [（字段名1，字段名2，……）] VALUES（值1，值2，……），（值1，值2，……），……；

该方式中：（字段名1，字段名2，……）是可选的，用于指定插入的字段名；（值1，值2，……），（值1，值2，……）

表示要插入的记录，该记录可有多条并且每条记录之间用逗号隔开。

例：向学生表中插入多条学生信息MySQL命令

insert into student (id,name,age,gender) values(2,'lucy',17,'female'),(3,'jack',19,'male'),(4,'tom',18,'male');

## 十一、更新数据

**在MySQL中通过UPDATE语句更新数据表中的数据。**

### 1.UPDATE基本语法

UPDATE 表名 SET 字段名1 = 值1，字段名2 = 值2，…… [WHERE 条件表达式]；

值1、值2代表字段的新数据；WHERE条件可选，用于指定更新数据需要满足的条件

### 2.UPDATE更新部分数据

例：

update student set age = 20,gender = 'famale' where name = 'tom';

### 3.UPDATE更新全部数据

例：

update student set age = 18;

## 十二、删除数据

在MySQL中通过DELETE语句删除数据表中的数据。

准备一张学生表:

create table student(

id int,

name varchar(30),

age int,

gender varchar(30)

);

-------插入数据

insert into student (id,name,age,gender) values(2,'lucy,17,'female'),(3,'jack',19,'male'),(4,'tom',18,'male'),(5,'sal',19,'female'),(6,'sun',20,'male');

### 1.DELETE基本语法

语法：

DELETE FROM 表名 [WHERE 条件表达式];

表名用于指定要执行删除操作的表，[WHERE 条件表达式]为可选参数用于指定删除的条件。

### 2.DELETE删除部分数据

例：

delete from student where age = 14;

### 3.DELETE删除全部数据

例：

delete from student;

### 4.TRUNCATE和DELETE的区别

都能实现删除表中所有数据的功能，区别：

1.DELETE语句后面可跟WHERE子句，可以通过WHERE子句中的条件表达式只删除满足条件的部分记录，但TRUNCATE用于删除全部记录。

2.使用TRUNCATE语句删除表中的数据后，再次向表中添加记录时自动增加的字段默认初始值重新由1开始；使用DELETE语句删除表中所有记录后，再次向表中添加记录时自动增加字段的值为删除时该字段的最大值加1.

3.DELETE语句是DML语句，TRUNCATE语句常被认为是DDL语句。

## 十三、MySQL数据表简单查询



### 1.简单查询概述

简单查询即为不含where的select语句

两种查询：查询所有字段和查询指定字段。

准备测试数据：

---创建数据库

DROP DATABASE IF EXISTS mydb;

CREAT DATABASE mtdb;

USE mydb;

----创建student表

CREAT TABLE student(

​	sid CHAR(6),

​	sname VARCHAR(50),

​	age INT,

​	gender VARCHAR(50) DEFAULT 'male'

);

-----向student表插入数据（【例：：：】）（直接复制）

INSERT INTO student (sid,sname,age,gender) VALUES ('S_1001', 'lili', 14, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1002', 'wang', 15, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1003', 'tywd', 16, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1004', 'hfgs', 17, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1005', 'qwer', 18, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1006', 'zxsd', 19, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1007', 'hjop', 16, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1008', 'tyop', 15, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1009', 'nhmk', 13, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1010', 'xdfv', 17, 'female');

### 2.查询所有字段（方法不唯一）

selsct * from student;

### 3.查询指定字段

查（sid、sname）：：

select sid,sname form student;

### 4.常数的查询

在SELECT中除了书写别名，还可以书写参数，可以用于标记

常数的查询日期标记MySQL命令：

select sid,sname,'2021-03-02' from student;

### 5.从查询结果中过滤重复数据

使用DISTINCT注意：在SELECT查询语句中DISTINCT关键字只能用在第一个所查列名之前。

select distinct gender form student;

### 6.算术运算符（例）

在SELECT查询语句中还可以使用加减乘除运算符

查询学生10年后的年龄MySQL命令：

select sname,age+10 from student;

## 十四、函数

准备测试数据如下：（可直接复制）

--创建数据库

DROP DATABASE IF EXISTS mydb;

CREATE DATABASE mydb;

USE mydb;

--创建student表

CREATE TABLE student(

​	sid CHAR(6),

​	sname VARCHAR(50),

​	age INT,

​	gender VARCHAR(50) DEFAULT 'male'

);

--向student表插入数据（直接复制）

INSERT INTO student (sid,sname,age,gender) VALUES ('S_1001', 'lili', 14, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1002', 'wang', 15, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1003', 'tywd', 16, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1004', 'hfgs', 17, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1005', 'qwer', 18, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1006', 'zxsd', 19, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1007', 'hjop', 16, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1008', 'tyop', 15, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1009', 'nhmk', 13, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1010', 'xdfv', 17, 'female');

### 1.聚合函数

冲击某个字段的最大值，最小值，平均值等。

聚合-->就是将多行汇总成一行。

所有的聚合函数均是输入多行，输出一行。

聚合函数具有自动缕空的功能，若某一个值为NULL，那么会自动过滤使其不参与运算。

**使用规则:**

只有SELECT子句和HAVING子句、ORDER BY子句中能够使用聚合函数（在WHERE 子句中使用是错误的）

#### 1.1、count()

统计表中数据的行数或者统计指定列其值不为NULL的数据个数

select count(*) from student;

#### 1.2、max()

计算指定列的最大值，如果指定列是字符串类型则使用字符串排序运算

select max(age) from student;

#### 1.3、min()

计算指定列的最小值，如果指定列是字符串类型则使用字符串排序运算

select sname,min(age) from student;

#### 1.4、sum()

计算指定列的数值和，如果指定列类型不是数值类型则计算结果为0

select sum(age) from student;

#### 1.5、avg()

计算指定列的平均值，如果指定列类型不是数值类型则计算结果为0

select avg(age) from student;

### 2.其他常用函数

#### 2.1、时间函数

1.SELECT NOW();

2.SELECT DAY(NOW());

3.SELECT DATE(NOW());

4.SELECT TIME(NOW());

5.SELECT YEAR(NOW());

6.SELECT MONTH(NOW());

7.SELECT CURRENT_DATE();

8.SELECT CURRENT_TIME();

9.SELECT CURRENT_TIMESTAMP();

10.SELECT ADDTIME('14:23:12','01:02:01');

11.SELECT DATE_ADD(NOW(),INTERVAL 1 DAY);

12.SELECT DATE_ADD(NOW(),INTERVAL 1 MONTH);

13.SELSECT DATE_SUB(NOW(),INTERVAL 1 DAY);

14.SELECT DATE_SUB(NOW(),INTERVAL 1 MONTH);

15.SELECT DATEDIFF('2019-07-22','2019-05-05');

### 2.2字符串函数

#### 1.--连接函数

SELECT CONCAT()

#### 2.--字符查找函数

SELECT INSTR();

#### 3.--统计长度

SELECT LENGTH();

### 2.3、数学函数

#### 1.--绝对值

SELECT ABS（-1396）;

#### 2.--向下取整

SELECT FLOOR(3.14);

#### 3.--向上取整

SELECT CEILING(3.14);

## 十五、条件查询

可以根据需求获取指定数据---可以在查询语句中通过WHERE子句指定查询条件对查询结果进行过滤。

准备测试数据：（不需要练的前提下直接复制）

--创建数据库

DROP DATABASE EXISTS mydb;

CREATE DATABASE mydb;

USE mydb;

--创建student表

VREATE ABLE student(

​	sid CHAR(6),

​	sname VARCHAR(50),

​	age INT,

​	gender VARCHAR(50) DEFAULT'male'

);

--向student表插入数据（直接复制）

INSERT INTO student (sid,sname,age,gender) VALUES ('S_1001', 'lili', 14, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1002', 'wang', 15, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1003', 'tywd', 16, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1004', 'hfgs', 17, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1005', 'qwer', 18, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1006', 'zxsd', 19, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1007', 'hjop', 16, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1008', 'tyop', 15, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1009', 'nhmk', 13, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1010', 'xdfv', 17, 'female');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1012', 'lili', 14, 'male');
INSERT INTO student (sid,sname,age,gender) VALUES ('S_1013', 'wang', 15, 'female');

### 1.使用关系运算符查询

在WHERE中可使用关系运算符进行条件查询，需要注意的是：：

<> 不等于			剩余都一样

查询年龄>=17的MySQL命令：

select * from studnet where age >= 17;

### 2.使用IN关键字查询

IN关键字用于判断某个字段的值是否在指定集合中。

若字段的值恰好在指定集合中，则将字段所在的记录查询出来。

例：（查S_1002和S_1003的sid）

select * from student shere sid in('S_1002','S_1003');

例：（查除S_1001以外的sid）

select * from student where sid not in ('S_1001');

### 3.使用BETWEEN AND关键字查询

BETWEEN AND用于判断某个字段的值是否在指定的范围之内。

若字段的值在指定范围内，则将所在记录查询出来。

例：（age 15-18）

select * from student where age between 15 and 18;

例：（age != (15,18)）

select * from student where age not between 15 and 18;

### 4.使用空值查询

在MySQL中，使用IS NULL关键字判断字段的值是否为空值。

**空值NULL不同于0，也不同于空字符串**

例：（查sname不为空值）

select * from student where sname is not null;

### 5.使用AND关键字查询

在MySQL中可使用AND关键字可以链接两个或多个查询条件

例：（age>18&&gender == male）

select * from student where age>15 and gender = 'male';

### 6.使用OR关键字查询

使用SELECT语句查询数据时可使用OR关键字链接多个查询条件。

使用Or关键字时，只要记录满足一个条件就会被查出来。

select * from student where age > 15 or gender = 'male';

### 7.使用LIKE关键字查询

MySQL中可使用LIKE关键字可以判断两个字符串是否相匹配

#### 7.1普通字符串

例：（sname = wang）

select * from student where sname like 'wang';

#### 7.2含有%统配的字符串

%用于匹配任意长度的字符串。

例：字符串“a%”匹配以字符a开始任意长度的字符串

例：（查 姓名以li开始）

select * from student where sname like 'li%';

例：（以g结尾）

select * from student where sname like '%g';

例：（查含s）

select * from student where sname like '%s%';

#### 7.3含有——通配的字符串

下划线只匹配单个字符，如果要匹配多个字符，需要连续使用多个下划线通配符。

例：（查zx开头长度为4）

select * from student where sname like 'zx__';

例：（查g结尾长度为4）

select * from student where sname like'___g';

### 8.使用LIMIT限制查询结果的数量

当执行查询数据时可能会返回很多条记录，而用户需要的数据可能只是其中一条或者几条。

例：（年纪最小3位）

select * from studnet order by age asc limit 3;

### 9.使用GROUP BY进行分组查询

GROUP BY子句可以像切蛋糕一样将表中的数据进行分组，在进行查询等操作。换言之，可通俗理解为：通过GROUP BY将原来的表拆分承了几张小表

准备测试代码：(经过前一段时间练习可以直接复制)

-- 创建数据库
DROP DATABASE IF EXISTS mydb;
CREATE DATABASE mydb;
USE mydb;

-- 创建员工表
CREATE TABLE employee (
    id int,
    name varchar(50),
    salary int,
    departmentnumber int
);

-- 向员工表中插入数据
INSERT INTO employee values(1,'tome',2000,1001); 
INSERT INTO employee values(2,'lucy',9000,1002); 
INSERT INTO employee values(3,'joke',5000,1003); 
INSERT INTO employee values(4,'wang',3000,1004); 
INSERT INTO employee values(5,'chen',3000,1001); 
INSERT INTO employee values(6,'yukt',7000,1002); 
INSERT INTO employee values(7,'rett',6000,1003); 
INSERT INTO employee values(8,'mujk',4000,1004); 
INSERT INTO employee values(9,'poik',3000,1001);

#### 9.1GROUP BY和聚合函数一起使用

例：（统计各部门员工个数）

select count(*),departmentnumber from employee group by departmentnumber;

例：（统计编号>1001）

select count(*),departmentnumber from employee where departmentnumber>1001 group by departmentnumber;

#### 9.2GROUP BY和聚合函数以及HAVING一起使用

例：（统计工资总和>8000的部门）

select sum(salary),departmentnumber from employee group by departmentnumber having sum(salary)>8000;

### 10.使用ORDER BY对查询结果进行排序

语法：

SELECT 字段名1，字段名2，……

FROM 表名

ORDER BY 字段名1[ASC | DESC],字段名2[ASC | DESC];

该语法中，字段名1、字段名2是查询结果排序的依据；参数ASC表示按照升序排序，DESC表示按照降序排序；默认情况下，按照ASC方式排序。通常情况下，ORDER BY子句位于整个SELECT语句的末尾。

例：（年龄小到大）

select * from student order by age asc;

例：（年龄大到小）asc-->desc

## 十六、别名设置

在查询数据时可为表和字段取别名，该别名代替表和字段得原名参与查询操作。

### 1.为表取别名

查询操作，表名太长不方便，起别名（不替换）

语法：

SELECT * FROM 表名 AS 表的别名 WHERE ……；

例:

select * from student as stu;

### 2.为字段取别名

查询操作，字段名太长不方便，起别名（不替换）

语法:

SELECT 字段名1 AS 别名1，字段名2 AS 别名2，……FROM 表名 WHERE……；

例：

select name as '姓名',id from student;

## 十七、表的关联关系

在实际开发中数据表之间存在着各种关联关系，介绍三种：

### 多对一：

最常见的一种关系。例：多个员工与一个部门。多对一的表关系中，应将外键建在多的一方否则会造成数据的冗余。

### 多对多：

常见的一种关系。例：多个学生与多个老师。通常：为了实现这种关系需要定义一张中间表（连接表），该表会存在两个外键分别参照老师表和学生表。

### 一对一：

不常见。这种方式存储的信息会放在同一张表中。

准备测试数据如下：（可直接复制）

DROP TABLE IF EXISTS student；

DROP TABLE IF EXISTS class；

--创建班级表

CREAT TABLE class(

​	cid int(4) NOT NULL PRIMARY KEY,

​	cname varchar(30),

);



--创建学生表

CREAT TABLE student(

​	sid int(8) NOT NULL PRIMARY KEY,

​	sname varchar(30),

​	classid int(8) NOT NULL

);



--为学生表添加外键约束

ALTER TABLE student ADD CONSTRAINT fk_student_classid FOREIGN KEY(classid) REFERENCE;



--向班级表插入数据

INSERT INTO class(cid,cname) VALUES(1,'Java');

INSERT INTO class(cid,cname) VALUES(2,'Python');



--向学生表插入数据

INSERT INTO student(sid,sname,classid)VALUES(1,'tome',1);
INSERT INTO student(sid,sname,classid)VALUES(2,'lucy',1);
INSERT INTO student(sid,sname,classid)VALUES(3,'lili',2);
INSERT INTO student(sid,sname,classid)VALUES(4,'domi',2);

### 1.关联查询

例：（查Java班所有学生）

select * from student where classid=(select cid from class where cname='Java');

### 2.关于关联关系的删除数据

例：（从班级表中删除Java班）**【注意：班级表学生表之间存在关联关系，要删除Java班，应该先删除学生表中与该班相关联的学生。】如若不然，学生表中的cid就失去了关联。****

delete from student where classid=(select cid from class where cname = 'Java');

delete from class where canme = 'Java';

## 十八、多表连接查询

### 1.交叉连接查询

交叉连接返回结果是被连接的两个表中所有数据行的**笛卡尔积**，例：集合A={a，b}，集合B={0，1，2}，则集合A和B笛卡尔积为{(a,0),(a,1),(a,2),(b,0),(b,1),(b,2)}。

所以交叉连接也被称为笛卡尔连接，语法：

SELECT * FROM 表1 CROSS JOIN 表2；

CROSS JOIN用于连接两个要查询的表，通过该语句可以查询两个表中所有的数据组合。

**注意：交叉连接查询在实际运用中没有任何意义，了解即可！**

### 2.内连接查询

内连接又称简单连接或自然连接（非常常见的连接查询）。

内连接使用比较运算符对两个表中的数据进行比较并列出与连接条件匹配的数据行，组合成新的记录。

内连接查询只有满足条件的记录才能出现在查询结果中，语法：

SELECT 查询字段1，查询字段2，…… FROM 表1[INNER] JOIN 表2 ON 表1.关系字段=表2.关系字段

INNER JOIN用于连接两个表，ON来指定连接条件，INNER可省略。

准备测试数据如下（可复制）

--若存在数据库mydb则删除

DROP DATABASE IF EXISTS mydb；

--创建数据库mydb

CREATE DATABASE mydb；

--选择数据库mydb

USE mydb；



--创建部门表

CREATE TABLE department(

​	did int(4) NOT NULL PRIMARY KEY,

​	dname varchar(20)

);



-创建员工表

CREATE TABLE employee{

​	eid int(4) NOT NULL PRIMARY KEY,

​	ename varchar(20),

​	eage int(2),

​	departmentid int(4) NOT NULL

};



-- 向部门表插入数据
INSERT INTO department VALUES(1001,'财务部');
INSERT INTO department VALUES(1002,'技术部');
INSERT INTO department VALUES(1003,'行政部');
INSERT INTO department VALUES(1004,'生活部');
-- 向员工表插入数据
INSERT INTO employee VALUES(1,'张三',19,1003);
INSERT INTO employee VALUES(2,'李四',18,1002);
INSERT INTO employee VALUES(3,'王五',20,1001);
INSERT INTO employee VALUES(4,'赵六',20,1004);

例：（查询员工姓名及其所属部门名称）

select employee.ename,department,dname from department inner join employee on department.did=employee.departmentid;

### 3.外连接查询

使用内连接查询时返回的结果只包含符合查询条件和连接条件的数据。

但有时还需要再返回查询结果中不仅包含符合条件的数据，而且还包括左、右表或两个表中的所有数据。--外连接

外连接又分为左连接和右连接。语法：

SELECT 查询字段1，查询字段2，…… FROM 表1 LEFT | RIGHT [OUTER] JOIN 表2 ON 表1.关系字段=表2.关系字段 WHERE 条件；

注：

1LEFT[]OUTER] JOIN左（外）连接：返回包括左表中的所有记录和右表中符合连接条件的记录。

2.RIGHT[OUTER] JOIN右（外）连接：返回包括右表中所有记录和左表中符合连接条件的记录。

准备测试数据如下：（可复制）

-- 若存在数据库mydb则删除
DROP DATABASE IF EXISTS mydb;
-- 创建数据库mydb
CREATE DATABASE mydb;
-- 选择数据库mydb
USE mydb;

-- 创建班级表
CREATE TABLE class(
  cid int (4) NOT NULL PRIMARY KEY, 
  cname varchar(20)
);

-- 创建学生表
CREATE TABLE student (
  sid int (4) NOT NULL PRIMARY KEY, 
  sname varchar (20), 
  sage int (2), 
  classid int (4) NOT NULL
);
-- 向班级表插入数据
INSERT INTO class VALUES(1001,'Java');
INSERT INTO class VALUES(1002,'C++');
INSERT INTO class VALUES(1003,'Python');
INSERT INTO class VALUES(1004,'PHP');

-- 向学生表插入数据
INSERT INTO student VALUES(1,'张三',20,1001);
INSERT INTO student VALUES(2,'李四',21,1002);
INSERT INTO student VALUES(3,'王五',24,1002);
INSERT INTO student VALUES(4,'赵六',23,1003);
INSERT INTO student VALUES(5,'Jack',22,1009);

#### 3.1左外连接

左（外）连接结果包括LEFT JOIN子句中指定的左表的所有记录，以及所有满足连接条件的记录。

注：若左表的某条记录在右表中不存在则在右表中显示为空。

例：（查每个班班级ID、班级名称以及该班所有学生的名字）

select class.cid,class.cname,student.sname from class left outer join student on class.cid=student.classid;

#### 3.2右外连接

右（外）连接结果包括RIGHT JOIN子句中指定的右表的所有记录，以及所有满足连接条件的记录。

注：同上

例：（同上）

select class.cid,class.cname,student.sname from class right outer join student on class.cid=student.classid;

## 十九、子查询

子查询是指一个查询语句嵌套在另一个查询语句内部的查询。

该查询语句可以嵌套在一个SELECT、SELECT……INTO、INSERT……INTO等语句中。

在执行查询时，首先会执行子查询中的语句，再将返回的结果作为外层查询的过滤条件。

再子查询中通常可以使用比较运算符和IN、EXISTS、ANY、ALL等关键字。

准备测试数据如下：（可复制）

DROP TABLE IF EXISTS student;
DROP TABLE IF EXISTS class;

-- 创建班级表
CREATE TABLE class(
  cid int (4) NOT NULL PRIMARY KEY, 
  cname varchar(20)
);

-- 创建学生表
CREATE TABLE student (
  sid int (4) NOT NULL PRIMARY KEY, 
  sname varchar (20), 
  sage int (2), 
  classid int (4) NOT NULL
);

-- 向班级表插入数据
INSERT INTO class VALUES(1001,'Java');
INSERT INTO class VALUES(1002,'C++');
INSERT INTO class VALUES(1003,'Python');
INSERT INTO class VALUES(1004,'PHP');
INSERT INTO class VALUES(1005,'Android');

-- 向学生表插入数据
INSERT INTO student VALUES(1,'张三',20,1001);
INSERT INTO student VALUES(2,'李四',21,1002);
INSERT INTO student VALUES(3,'王五',24,1003);
INSERT INTO student VALUES(4,'赵六',23,1004);
INSERT INTO student VALUES(5,'小明',21,1001);
INSERT INTO student VALUES(6,'小红',26,1001);
INSERT INTO student VALUES(7,'小亮',27,1002);

### 1.带比较运算符的子查询

例：（查张三所在班级信息）

select * from class where cid=(select classid from student where sname='张三')；

例：（查比张三编号大的班级的信息）

select *  from class where cid>(select classid from student where sname='张三');

### 2.带EXISTS关键字的子查询

EXISTS关键字后面的参数可以是任意一个子查询，不产生任何数据只返回TRUE或FALSE。

当返回值为TRUE时外层查询才会执行。

例：（若王五再student表中则从班级表中查询所有班级信息）

select * from class where exists (select * from student where sname ='王五');

### 3.带ANY关键字的子查询

ANY关键字表示满足其中任意一个条件就返回一个结果作为外层查询条件。

例：（查询比任一学生所属班级号还大的班级编号）

select * from class where cid > any (select classid from student);

### 4.带ALL关键字的子查询

ALL关键字与ANY有点类似，只不过带ALL关键字的子查询返回的结果需要同时满足所有内层查询条件

例：（查询比所有学生所属班级号还大的班级编号）

select * from class where cid > all (select classid from student);

## **总结：**

重要（从关键字分析）：

查询语句的书写顺序和执行顺序

select--->from--->where--->group by--->having--->order by--->limit

查询语句的执行顺序

from--->where--->group by--->having--->select--->order by--->limit

# 事务：

1.事物的基本介绍

​	1.概念

​		如果一个包含多个步骤的业务操作，被业务管理，那么这些操作要么同时成功，要么同时失败。

​	2.操作：

​		1.开启事务：start transaction;

​		2.回滚：rollback;

​		3.提交：commit;

​	3.例子：

```
create table account(
    id int primary key auto_increment,
    name varchar(10),
    balance double
);
insert into account (name ,balance) values ('zs',1000),('ls',1000);
update account set balance = 1000;
select *from account;
start transaction ;
update account set balance = balance - 500 where name = 'zs';
/*chucuo;*/
update account set balance = balance + 500 where name = 'ls';
rollback ;
commit;
select *from account;
```

​	4.MySQL数据库中事务默认自动提交

​	一条DML（增删改）语句会自动提交一次事务。

​	事务提交两种方式：

​		自动提交：

​			mysql就是自动提交的

​			一条DML语句会自动提交一次事务

​		手动提交：

​			Oracle数据库默认是手动提交事务

​			需要先开启事务，在提交

​		修改事务的默认提交方式：

​			查看事务的默认提交方式：select @@autocommit;--1代表自动提交，0代表手动提交

​			修改默认提交方式：set @@autocommit = 0;

2.事物的四大特征

​	1.原子性：是不可分割的最小操作单位，要么同时成功，要么同时失败

​	2.持久性：当事务提交或回滚后，数据库会持久化的保存数据。

​	3.隔离性：多个事务之间相互独立。

​	4.一致性：事务操作前后数据总量不变。

3.事务的隔离级别（了解）

​	概念：多个事务之间隔离的，相互独立的，但是如果多个事务错做同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。

​	存在问题：

​		1.脏读：一个事务读取到另一个事务中没有提交的数据

​		2.不可重复读：在同一个事务中，两次读取到的数据不一样。

​		3.幻读：一个事务操作（DML）数据表中所以记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改。

​	隔离级别：

​		1.read uncommitted：读未提交

​			产生的问题：藏独、不可重复读、幻读

​		2.read committed：已经提交（oracle）

​			产生的问题：不可重复读、幻读

​		3.repeatable read：可重复读(MySQL)

​			产生的问题：幻读

​		4.seriallizable：串行化

​			可以解决所有的问题

​	注意：隔离级别从小到大安全性越来越高，但是效率越来越低。

​	数据库设置隔离级别：

​	select @@tx_isolation;

​	数据库设置隔离级别：

​	set global transaction ioslation lecel 级别字符串;

