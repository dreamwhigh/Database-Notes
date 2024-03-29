---
typora-copy-images-to: pics
---

## 关系数据库

### 主流关系数据库

1. 商用数据库，例如：[Oracle](https://www.oracle.com/)，[SQL Server](https://www.microsoft.com/sql-server/)，[DB2](https://www.ibm.com/db2/)等；
2. 开源数据库，例如：[MySQL](https://www.mysql.com/)，[PostgreSQL](https://www.postgresql.org/)等；
3. 桌面数据库，以微软[Access](https://products.office.com/access)为代表，适合桌面应用程序使用；
4. 嵌入式数据库，以[Sqlite](https://sqlite.org/)为代表，适合手机应用和桌面程序。

### SQL

SQL（Structured Query Language）是用来访问和操作数据库系统的语言。定义了三种操作数据库的能力：

- **DDL：Data Definition Language**

  DDL允许用户定义数据，也就是创建表、删除表、修改表结构这些操作。通常，DDL由数据库管理员执行。

- **DML：Data Manipulation Language**

  DML为用户提供添加、删除、更新数据的能力

- **DQL：Data Query Language**

  DQL允许用户查询数据

**语法特点**

SQL语言关键字不区分大小写，针对不同的数据库，对于表名和列名，有的数据库区分大小写，有的数据库不区分大小写。

这里，SQL关键字总是大写，以示突出，表名和列名均使用小写。

### 关系模型

关系数据库是建立在关系模型上的。而关系模型本质上就是若干个存储数据的二维表，表和表之间需要建立“一对多”，“多对一”和“一对一”的关系，这样才能够按照应用程序的逻辑来组织和存储数据。

表的每一**行**称为**记录**（Record），记录是一个逻辑意义上的数据。

表的每一**列**称为**字段**（Column），同一个表的每一行记录都拥有相同的若干字段。

### 主键、外键

主键：能够唯一区分出不同的记录的字段，一般采用自增整数类型（INT NOT NULL AUTO_INCREMENT）的id字段作为主键

联合主键：两个或更多的字段都设置为主键，尽量不使用，因为它给关系表带来了复杂度的上升。

外键：可以把该表的数据与另一张表关联起来的列，通过定义外键约束实现，可以保证无法插入无效的数据，但会降低数据库的性能，一般不使用，对应的列仍然是一个普通的列，但是起到了外键的作用。

### 索引

索引是关系数据库中对某一列或多个列的值进行**预排序的数据结构**。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，这样就大大加快了查询速度。

```sql
//创建索引，包含name和score两列
ALTER TABLE students
ADD INDEX idx_name_score (name, score);
```

**优点**：提高了查询效率

**缺点**：插入、更新和删除记录时，需要同时修改索引

#### 主键索引 

PRIMARY KEY

关系数据库会自动创建主键索引。使用主键索引的效率是最高的，因为主键会保证绝对唯一。

```

ALTER TABLE tbl_name ADD PRIMARY KEY (column_list)
```



#### 唯一索引

UNIQUE INDEX

具有唯一性约束的列

```sql
//添加唯一索引 UNIQUE INDEX
ALTER TABLE tbl_name ADD UNIQUE index_name (column_list)

//添加唯一约束而不创建索引
ALTER TABLE students
ADD CONSTRAINT uni_name UNIQUE (name);
```

#### 其他索引

```
CREATE INDEX indexName ON mytable(username(length)); 

ALTER TABLE tbl_name ADD INDEX index_name (column_list)//普通索引，索引值可出现多次。

ALTER TABLE tbl_name ADD FULLTEXT index_name (column_list)//指定索引为 FULLTEXT ，用于全文索引。
```

#### 其他命令

```
SHOW INDEX FROM table_name;//显示索引信息
 ALTER TABLE testalter_tbl DROP INDEX c;//删除索引
```



## 软件安装

MySQL8 安装教程参照：[MySQL8.0.15 安装教程 ](<https://www.jianshu.com/p/647a596cb251>)

按照[Navicat Premium 12 安装与激活 ](<https://blog.csdn.net/ly_solo/article/details/80405373>) 进行安装，需要注意：

- 使用博主百度云盘提供的安装包，如果使用官网下载的安装包安装会导致最后请求码无效的错误
- 需要断网激活，否则会出现”激活次数到达上限“的错误

## 基础

本机安装的 MySQL 服务名设置为 MySQL8。

安装的MySQL中包括两部分：

- MySQL Server，可执行程序是mysqld，在后台运行
- MySQL Client，命令行客户端，可执行程序是mysql

在MySQL Client中输入的SQL语句通过**TCP连接**发送到MySQL Server。默认端口号是3306，即如果发送到本机MySQL Server，地址就是`127.0.0.1:3306`。

也可以只安装MySQL Client，然后连接到远程MySQL Server。假设远程MySQL Server的IP地址是`10.0.1.99`，那么就使用`-h`指定IP或域名：

```
mysql -h 10.0.1.99 -u root -p
```



### 语句规范

- 关键字与函数名称全部大写（小写也能够识别）
- 数据库名称、表名称、字段名称全部小写
- SQL 语句必须以分号结尾

### 启动、停止、卸载

在 Windows 命令提示符下运行:

```
启动:net start MySQL8

停止: net stop MySQL8

卸载: sc delete MySQL8
```

### 登陆、退出

登陆：

```
MySQL -h 主机名 -u 用户名 -p

-h : 该命令用于指定客户端所要登录的 MySQL 主机名, 登录当前机器该参数可以省略;
-u : 所要登录的用户名;
-p : 告诉服务器将会使用一个密码来登录, 如果所要登录的用户名密码为空, 可以忽略此选项。
```

退出，3 种命令

```
MySQL>exit;//仅断开了客户端和服务器的连接，MySQL服务器仍然继续运行
MySQL>quit;
MySQL>\q;
```



### MySQL 提示符

提示符

```
\D：完整的日期
\d：当前数据库
\h：服务器名称
\u：当前用户
```

修改方法

1. 连接时修改

```
shell>MySQL -uroot -proot--prompt 提示符
```

2. 连接后通过 `prompt` 修改

```
MySQL>prompt 提示符
```





## 关键字，表达式

| <>              | 不相等                                                       |
| --------------- | ------------------------------------------------------------ |
|                 |                                                              |
| IN              | WHERE column_name IN (value1,value2,...) 表示该列取值为括号内的任一值 |
| TOP             | 规定要返回的记录的数目                                       |
| LIKE            | 在 WHERE 子句中搜索列中的指定模式                            |
| %               | 替代任意字符串，包括空                                       |
| _               | 替代一个字符                                                 |
| [ABC]           | 字符列中的任何单一字符                                       |
| [!ABC]或[\^ABC] | 不在字符列中的任何单一字符                                   |
|                 |                                                              |
|                 |                                                              |
|                 |                                                              |
|                 |                                                              |
|                 |                                                              |
|                 |                                                              |



## 数据库操作

### 创建数据库

`CREATE DATABASE IF NOT EXISTS db_name CHARACTER SET = charset_name;` 创建数据库 db_name，并设定编码方式为 charset_name；

`SHOW DATABASES;` 查看当前存在的所有数据库；

`SHOW CREATE DATABASE db_name;` 查看数据库 db_name 的编码方式；

`ALTER DATABASE db_name CHARACTER SET = utf8;` 修改数据库 db_name 的编码方式为 utf-8；

`SHOW WARNINGS;` 查看警告具体内容；

### 删除数据库

`DROP DATABASE IF EXISTS db_name;` 

### 选择数据库

`USE db_name;`

## 数据库表操作

### 创建表

`create table;`  表名称 (列声明);

 `show tables;`  查看已创建了表的名称; 

`SHOW COLUMNS FROM tbl_name` 查看数据表的详细信息；

`describe/DESC 表名;`   查看已创建的表的详细信息。

- "not null" 说明该列的值不能为空, 必须要填, 如果不指定该属性, 默认可为空;
- "auto_increment" 需在整数列中使用, 其作用是在插入数据时若该列为 NULL, MySQL 将自动产生一个比现存值更大的唯一标识符值。在每张表中仅能有一个这样的值且所在列必须为索引列。
- "primary key" 表示该列是表的主键, 本列的值必须唯一, MySQL 将自动索引该列。

`USE 表名;` 打开数据库表

`SHOW TABLES;` 查看当前数据库中的表 

`SHOW TABLES FROM db_name` 查看指定数据库中的表

`SELECT DATABASE();` 查看当前打开的数据库表

示例：

```
	CREATE TABLE [IF NOT EXISTS] students
	（
		id int unsigned not null auto_increment primary key,
		name char(8) not null,
		sex char(4) not null,
		age tinyint unsigned not null,
		tel char(13) null default "-"
	);
```

### 插入

`insert [into] 表名 values (值 1, 值 2, 值 3, ...);` 插入一条记录

`insert [into] 表名 [(列名 a, 列名 b, 列名 c)] values (值 1, 值 2, 值 3);` 只插入部分数据（列 a, b,c）

INSERT [INTO] 表名 SET 列名=...

INSERT [INTO] 表名 SELECT ...

### 更新

`update 表名称 set 列名称=新值 where 更新条件;` update 语句用来修改表中的数据。

例如：

```
update students set tel=default where id=5;
update students set age=age+1;
update students set name="张伟鹏", age=19 where tel="13288097888";
```

### 删除

`delete from 表名称 where 删除条件;`



### 修改

#### 修改数据库表的结构

`alter table` 语句用于创建后对表的修改, 基础用法如下:

```
#添加列，也可同时添加多列
alter table 表名 add 列名 列数据类型 [after 插入位置 ];
alter table 表名 add 列名 1 列数据类型 1，列名 2 列数据类型 2;

#修改列
alter table 表名 change 列名称 列新名称 新数据类型;

#删除列
alter table 表名 drop 列名称;

#重命名
alter table 表名 rename 新表名;

#删除表
drop table 表名;

#删除库
drop database 数据库名;

```

#### 修改约束

```
#添加主键约束
ALTER TABLE tbl_name ADD [CONSTRAINT [symbol]] PRIMARY KEY [index_type](index_col_name)
#删除主键约束
ALTER TABLE tbl_name DROP PRIMARY KEY
#添加唯一约束
ALTER TABLE tbl_name ADD [CONSTRAINT [symbol]] UNIQUE [INDEX|KEY] [index_name] [index_type](index_col_name,...)
#删除唯一约束
ALTER TABLE tbl_name DROP {INDEX|KEY} index_name
#添加外键约束
ALTER TABLE tbl_name ADD [CONSTRAINT [symbol]] FOREIGN KEY [index_type](index_col_name,...) reference_definition
#删除外键约束
ALTER TABLE tbl_name DROP FOREIGN KEY fk_symbol
#添加或删除默认约束
ALTER TABLE tbl_name ALTER [COLUMN] col_name {SET DEFAULT literal|DROP DEFAULT}


```

#### 修改列定义 

**MODIFY**

```
ALTER TABLE tbl_name MODIFY [COLUMN] col_name col_definition [FIRST|AFTER col_name]


```

#### 修改列名称 

**CHANGE**

```
ALTER TABLE tbl_name CHANGE [COLUMN] old_col_name new_col_name col_definition [FIRST | AFTER col_name]

```

#### 修改数据表名称

**RENAME**，两种方法：

- ALTER TABLE
- RENAME TABLE（可实现多表改名）

```
ALTER TABLE tbl_name RENAME [TO|AS] new_tbl_name

RENAME TABLE tbl_name TO new_tbl_name [,tbl_name2 TO new_tbl_name2]…

```

### 查询

`select 列名称 from 表名称 [查询条件 ];` 根据一定查询规则到数据库中获取数据；

#### 投影查询

只返回指定列的数据，而不是所有列的数据，也可以给每一列取别名

```
SELECT 列1 别名1, 列2 别名2, 列3 别名3 FROM
```

#### 排序

`ORDER BY` 用于排序，位于 `WHERE `子句后面。

- ASC，升序，默认的排序规则
- DESC，降序

```sql
SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;
```

#### 分页查询

结果集数据量很大时，采取分页显示。

当前页面号`pageIndex`，每页显示的结果数量`pageSize`，当前页的索引`pageIndex`（从1开始），确定`LIMIT`和`OFFSET`应该设定的值：

- `LIMIT`总是设定为`pageSize`；
- `OFFSET`计算公式为`pageSize × (pageIndex - 1)`。

#### 聚合查询

| 聚合函数 | 说明                                   |
| -------- | -------------------------------------- |
| COUNT(*) | 查询所有列的行数                       |
| SUM      | 计算某一列的合计值，该列必须为数值类型 |
| AVG      | 计算某一列的平均值，该列必须为数值类型 |
| MAX      | 计算某一列的最大值                     |
| MIN      | 计算某一列的最小值                     |

`GROUP BY`子句可以实现分组聚合

```sql
//统计各班的男生和女生人数
SELECT class_id, gender, COUNT(*) num FROM students GROUP BY class_id, gender;
```

#### 多表查询

SELECT查询从多张表同时查询数据，`SELECT * FROM <表1> <表2>`。

```sql
SELECT
    s.id sid,
    s.name,
    c.id cid,
FROM students s, classes c//取别名
WHERE s.gender = 'M' AND c.id = 1;//表名.列名
```

#### 连接查询



|          | 关键字           | 描述                       |
| -------- | ---------------- | -------------------------- |
| 内连接   | INNER JOIN       | 等值连接，只保留有关联的行 |
| 自连接   | INNER JOIN       | 自己连接自己               |
| 自然连接 | MATURAL JOIN     | 所有同名列的等值连接       |
| 左外连接 | LEFT OUTER JOIN  | 保留左表没有关联的行       |
| 右外连接 | RIGHT OUTER JOIN | 保留右表没有关联的行       |
| 外连接   | OUTER JOIN       | 保留所有没有关联的行       |

语法：`SELECT ... FROM <表1> INNER JOIN <表2> ON <条件...>`

#### WHERE 关键词

`select 列名称 from 表名称 where 条件;` 指定查询条件；

`where` 后面可以接一般的比较运算符（ =、>、<、>=、<、!= ），扩展运算符（[not] null、in、like ），组合查询（or、and ）等条件查询方式。

例如：

`select * from students where sex="女";` 

#### SELECT 关键字

`SELECT`可以用作计算，不带`FROM`子句的`SELECT`语句有一个有用的用途，就是用来判断当前到数据库的连接是否有效。许多检测工具会执行一条`SELECT 1;`来测试数据库连接。

```sql
SELECT 100+200;//用于计算，300
SELECT 1;//用于测试数据库连接
```



<div align="center"> <img src="https://github.com/dreamwhigh/Database-Notes/blob/master/docs/pics/0.png?raw=true" width="600"/> </div><br>

<div align="center"> <img src="https://github.com/dreamwhigh/Database-Notes/blob/master/docs/pics/1.png?raw=true" width="600"/> </div><br>

### 

## 约束

按约束所针对的字段多少来划分：

- 表级约束（约束一个以上的字段）
- 列级约束（只针对一个字段）。

按功能来划分：

- 非空约束：NOT NULL
- 主键约束：PRIMARY KEY
- 唯一约束：UNIQUE KEY
- 默认约束：DEFAULT
- 外键约束：FOREIGN KEY

### 主键约束 PRIMARY KEY

- 每张数据表只能存在一个主键，保证记录的唯一性
- 主键默认为 NOT NULL
- AUTO_INCREMENT 一定与主键一起使用，但主键不一定要使用 AUTO_INCREMENT
- 主键会自动船创建索引 

### 唯一约束 UNIQUE KEY

- 保证记录的唯一性
- 唯一约束的字段可以为 NULL，但只能有一个 NULL，否则与唯一性相悖
- 每张数据表可以存在多个唯一约束

### 默认约束 DEFAULT

- 插入记录时，若没有明确为字段赋值，则自动赋予默认值

### 外键约束

保证数据的一致性，完整性，实现一对一，一对多的数据关系。

**要求：**

- 父表和子表必须使用相同的存储引擎
- 数据表的存储引擎只能为 InnoDB
- 外键列和参照列必须有相似的数据类型（数字的长度要求相同，字符的长度可以不同）
- 外键列和参照列必须创建索引，如果外键列不存在索引，MySQL 会自动创建

```
#创建父表
MySQL>CREATE TABLE provinces(
    -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    -> pname VARCHAR(20) NOT NULL
    -> );
    
#创建子表    
MySQL>CREATE TABLE users(
    -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    -> username VARCHAR(10) NOT NULL,
    -> pid SMALLINT UNSIGNED,
#provinces(id) 为参照列，pid 为外键列
    -> FOREIGN KEY(pid) REFERENCES provinces(id)
    -> );
#provinces(id) 作为主键会自动创建索引，外键列 pid 不存在索引，MySQL 也会自动创建索引

```

**参照操作**

CASCADE：从父表删除或者更新且自动删除或更新子表中匹配的行

```
#添加了参照 CASCARD
FOREIGN KEY(pid) REFERENCES provinces(id) ON DELETE CASCARD


```

SET NULL：从父表删除或更新行，并设置子表中的外键行为 null,如果使用该选项，必须保证子表列没有指定 not null

RESTRICT：拒绝对父表的删除或更新操作。

NO ACTION：标准 SQL 的关键字，在 MySQL 中于 RESTRICT 相同

### 列级约束和表级约束

- 列级约束可以在列定义时或列定义后声明；表级约束只能在列定义后声明
- NOT NULL 和 DEFAULT 只能创建列级约束

## 子查询

- 子查询嵌套在查询内部，必须始终出现在圆括号内
- 子查询可以包含普通 select 可以包括的任何子句，比如：distinct、 group by、order by、limit、join 和 union 等
- 对应的外部查询必须是以下语句之一：select、insert、update、delete、set 或 者 do

**返回值**

可以返回标量、一行、一列或子查询（N 行 N 列）

**可使用的操作符**

- **比较运算符**：=、>、<、>=、<=、<>
- **ANY**：对于子查询返回的列中的**任何一个**数值，如果比较结果为 TRUE，就返回 TRUE
- **SOME**：ANY 的别名，用法相同
- **IN**：指定的一个值是否在这个集合中，在返回 TRUE，否则返回 FALSE。IN 相当于“=ANY”
- **ALL**：对于子查询返回的列中的所有值，如果比较结果都为 TRUE，才返回 TRUE”
- **EXISTS** ：判断子查询的结果集是否为空，若子查询的结果集不为空，则返回 TRUE；否则返回 FALSE。

## 范式

|      | 描述                             |
| ---- | -------------------------------- |
| 1NF  | 属性不可分                       |
| 2NF  | 每个非主属性完全依赖于键码       |
| 3NF  | 每个非主属性不传递函数依赖于键码 |

## 函数

#### 内置函数

[MySQL 函数大全 ](<http://c.biancheng.net/MySQL/function/>)

#### 自定义函数

开启自定义函数功能

```
show variables like '%fun%';#查看 MySQL 是否支持自定义函数功能

set global log_bin_trust_function_creators = 1; #开启自定义函数功能

```

创建函数

```
CREATE FUNCTION <函数名> ( [ <参数 1> <类型 1> [ , <参数 2> <类型 2>] ] … )
  RETURNS <类型>
 # 函数主体
 BEGIN
 其他语句；
 RETURN <值>；
 END



```

## 存储过程

[MySQL 存储过程 ](<https://www.cnblogs.com/mark-chan/p/5384139.HTML>)

### 简介

存储过程（Stored Procedure）是一组为了完成特定功能的 SQL 语句集，经编译后存储在数据库中，用户通过指定存储过程的名字并给定参数（如果该存储过程带有参数）来调用执行它。

### 优点

- 增强 SQL 语言的功能和灵活性
- 标准组件式编程，代码复用
- 由于存储过程是预编译的，所以性能高，执行速度较快
- 减少网络流量
- 代码封装，保证了一定的安全性

### 语法

**创建存储过程**

```
CREATE
    [DEFINER = { user | CURRENT_USER }]
　PROCEDURE sp_name ([proc_parameter[,...]])
    [characteristic ...] routine_body
 
proc_parameter:
    [ IN | OUT | INOUT ] param_name type

```

**其他语法**

```
#声明语句结束符，可以自定义，这里为 '//'
DELIMITER //
#声明存储过程
  CREATE PROCEDURE myproc(OUT s int)
#过程体
#存储过程开始符号
    BEGIN
      SELECT COUNT(*) INTO s FROM students;
#存储过程结束符号
    END//
DELIMITER ;

```

变量赋值：`SET @p_in=1 `

变量定义：`DECLARE l_int int unsigned default 4000000; `

创建 MySQL 存储过程、存储函数：`create procedure 存储过程名 (参数)`

存储过程体：`create function 存储函数名 (参数)`

### 参数

三种参数类型,IN,OUT,INOUT：

`CREATEPROCEDURE 存储过程名 ([[IN |OUT |INOUT ] 参数名 数据类形...])`

- IN 输入参数：表示调用者向过程传入值（传入值可以是字面量或变量），参数值在调用存储过程时指定

- OUT 输出参数：表示过程向调用者传出值 (可以返回多个值)（传出值只能是变量），参数值可以被存储过程改变，并且可以返回

  该变量是用户变量，用@num 的形式命名

- INOUT 输入输出参数：既表示调用者向过程传入值，又表示过程向调用者传出值（值只能是变量），参数值在调用时指定，可以被改变和返回

### 调用

```
CALL 存储过程名 #无参数
CALL 存储过程名（参数值） #有参数

```

### 与自定义函数的区别

- 存储过程实现的功能相对复杂，函数针对性较强
- 存储过程可以返回多个值，函数只能有一个返回值
- 存储过程一般独立执行，函数可以作为 SQL 语句的组成部分来出现
- 存储过程也比通过 API 接口调用程序要快。

## 存储引擎

数据库存储引擎是数据库底层软件组织，数据库管理系统（DBMS）使用数据引擎进行创建、查询、更新和删除数据。不同的存储引擎提供不同的存储机制、索引技巧、锁定水平等功能，使用不同的存储引擎，还可以 获得特定的功能。

[四种 MySQL 存储引擎 ](<https://www.cnblogs.com/wcwen1990/p/6655416.HTML>)

| **功 能**    | MyISAM | **Memory** | **InnoDB** | **ArcHive** |
| ------------ | ------ | ---------- | ---------- | ----------- |
| 存储限制     | 256TB  | RAM        | 64TB       | None        |
| 事务安全     | -      | -          | Yes        | -           |
| 锁颗粒       | 表锁   | 表锁       | 行锁       | 行锁        |
| 支持全文索引 | Yes    | -          | -          | -           |
| 支持数索引   | Yes    | Yes        | Yes        | -           |
| 支持哈希索引 | -      | Yes        | -          | -           |
| 支持数据缓存 | -      | N/A        | Yes        | -           |
| 数据压缩     | Yes    | -          | -          | Yes         |
| 支持外键     | -      | -          | Yes        | -           |

## 三级模式、两级映像

[数据库模式 ](<https://www.cnblogs.com/xiehuan-blog/p/9033481.html>)

<div align="center"> <img src="https://github.com/dreamwhigh/Database-Notes/blob/master/docs/pics/2.png?raw=true" width="600"/> </div><br>

又称**存储模式**，是数据物理结构和存储方式的描述，是数据在数据库内部的表示方式。所有的内部记录类型、索引和文件的组织方式，以及数据控制方面的细节。

内部记录并不涉及物理记录，也不涉及设备的约束。比内模式更接近于物理存储和访问的那些软件机制是操作系统的一部分（即文件系统）。

描述内模式的数据定义语言称为“内模式 DDL”。

### 概念模式

又称**模式**，是数据库中全部数据的逻辑结构和特征的描述，它由若干个概念记录类型组成，只涉及行的描述，不涉及具体的值。

概念模式反映的是数据库的结构及其联系，所以是相对稳定的；而实例反映的是数据库某一时刻的状态，所以是相对变动的。

概念模式不仅要描述概念记录类型，还要描述记录间的联系、操作、数据的完整性和安全性等要求。但是，概念模式不涉及存储结构、访问技术等细节。只有这样，概念模式才算做到了“物理数据独立性”。

描述概念模式的数据定义语言称为“模式 DDL” 。

### 外模式

又称**用户模式或子模式**，是用户与数据库系统的接口，是用户用到的那部分数据的描述。它由若干个外部记录类型组成。用户使用数据操纵语言对数据库进行操作，实际上是对外模式的外部记录进行操作。

描述外模式的数据定义语言称为“外模式 DDL”。有了外模式后，程序员不必关心概念模式，只与外模式发生联系，按外模式的结构存储和操作数据。

**联系**

数据按外模式的描述提供给用户；按内模式的描述存储在磁盘上；而概念模式提供了连接这两级模式的相对稳定的中间层，并使得两级中任意一级的改变都不受另一级的牵制。

### 两级映像

数据库系统在三级模式之间提供了两级映像：**模式/内模式的映像、外模式/模式的映像**。这两级映射保证了数据库中的数据具有较高的**物理独立性和逻辑独立性**。

- 模式/内模式的映像：实现概念模式到内模式之间的相互转换。
- 外模式/模式的映像：实现外模式到概念模式之间的相互转换。

数据的独立性是指数据与程序独立，将数据的定义从程序中分离出去，由 DBMS 负责数据的存储，从而简化应用程序，大大减少应用程序编制的工作量。数据的独立性是由 DBMS 的二级映像功能来保证的。数据的独立性包括数据的物理独立性和数据的逻辑独立性。

数据的物理独立性是指当数据库的内模式发生改变时，数据的的逻辑结构不变。由于应用程序处理的只是数据的逻辑结构，这样物理独立性可以保证，当数据的物理结构改变了，应用程序不用改变。但是，为了保证应用程序能够正确执行，需要修改概念模式/内模式之间的映像。

数据的逻辑独立性是指用户的应用程序与数据库结构是相互独立的。数据的逻辑结构发生变化后，用户程序也可以不修改。但是，为了保证应用程序能够正确执行，需要修改外模式/概念模式之间的映像。

## 其他命令

```
SHOW CREATE TABLE 表名 #查看创建该表的命令情况，包括存储引擎（默认为 InnoDB）和编码方式（默认为 utf-8）

SHOW INDEXES FROM 表名 #查看索引该表索引

SELECT a INTO b;#赋值 b=a

```



## 难点

### DELETE、TRUNCATE 和 DROP

[DELETE、TRUNCATE 和 DROP](<https://xiaozhuanlan.com/topic/6375814029>)

**作用**

- DELETE 删除表中 WHERE 语句指定的数据
- TRUNCATE 清空表，相当于删除表中的所有数据
- DROP 删除表结构

**事务**

- DELETE 会被放到日志中以便进行回滚
- TRUNCATE 和 DROP 立即生效，不会放到日志中，也就不支持回滚

**删除空间**

- DELETE 不会减少表和索引占用的空间；
- TRUNCATE 会将表和索引占用的空间恢复到初始值；
- DROP 会将表和索引占用的空间释放。

**耗时**

通常来说，DELETE < TRUNCATE < DROP。

## 参考资料

[21 分钟 MySQL 入门教程 ](<https://www.cnblogs.com/mr-wid/arcHive/2013/05/09/3068229.HTML#c2>)

[菜鸟驿站 MySQL 教程 ](<https://www.runoob.com/MySQL/MySQL-tutorial.HTML>)

[CS-Notes](<https://cyc2018.github.io/CS-Notes/#/notes/MySQL>)

[慕课网-与 MySQL 的零距离接触 ](<https://www.imooc.com/learn/122>)

[廖雪峰的官方网站-SQL](<https://www.liaoxuefeng.com/wiki/1177760294764384/1179613436834240>)

## 待整理

加锁

select .. from  
不加任何类型的锁

select...from lock in share mode
在扫描到的任何索引记录上加共享的（shared）next-key lock，还有主键聚集索引加排它锁 

select..from for update
在扫描到的任何索引记录上加排它的next-key lock，还有主键聚集索引加排它锁 

update..where   delete from..where
在扫描到的任何索引记录上加next-key lock，还有主键聚集索引加排它锁 

insert into..
简单的insert会在insert的行对应的索引记录上加一个排它锁，这是一个record lock，并没有gap，所以并不会阻塞其他session在gap间隙里插入记录。不过在insert操作之前，还会加一种锁，官方文档称它为insertion intention gap lock，也就是意向的gap锁。这个意向gap锁的作用就是预示着当多事务并发插入相同的gap空隙时，只要插入的记录不是gap间隙中的相同位置，则无需等待其他session就可完成，这样就使得insert操作无须加真正的gap lock。想象一下，如果一个表有一个索引idx_test，表中有记录1和8，那么每个事务都可以在2和7之间插入任何记录，只会对当前插入的记录加record lock，并不会阻塞其他session插入与自己不同的记录，因为他们并没有任何冲突。
————————————————
版权声明：本文为CSDN博主「胡儿胡儿」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/cug_jiang126com/article/details/50596729

# 