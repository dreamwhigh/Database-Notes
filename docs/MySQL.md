---
typora-copy-images-to: pics

---

# MySQL

## 软件安装

MySQL8 安装教程参照：[MySQL8.0.15 安装教程 ](<https://www.jianshu.com/p/647a596cb251>)

按照[Navicat Premium 12 安装与激活 ](<https://blog.csdn.net/ly_solo/article/details/80405373>) 进行安装，需要注意：

- 使用博主百度云盘提供的安装包，如果使用官网下载的安装包安装会导致最后请求码无效的错误
- 需要断网激活，否则会出现”激活次数到达上限“的错误

## 基础

本机安装的 MySQL 服务名设置为 MySQL8

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
MySQL>exit;
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

## 数据类型

[MySQL 数据类型 ](<https://www.runoob.com/MySQL/MySQL-data-types.HTML>)

### 数值类型

| 类型           | 大小                                          | 范围（有符号）                                               | 范围（无符号）                                               |
| :------------- | :-------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| TINYINT        | 1 字节                                        | (-2^7^，2^7^ - 1                                             | (0，2^8^ - 1)                                                |
| SMALLINT       | 2 字节                                        | (-2^15^，2^15^ - 1)                                          | (0，2^16^ - 1)                                               |
| MEDIUMINT      | 3 字节                                        | (-2^23^，2^23^ - 1)                                          | (0，2^24^ - 1)                                               |
| INT 或 INTEGER | 4 字节                                        | (-2^31^，2^31^ - 1)                                          | (0，2^32^ - 1)                                               |
| BIGINT         | 8 字节                                        | (-2^63^，2^63^ - 1)                                          | (0，2^64^ - 1)                                               |
| FLOAT[(M, D)]  | 4 字节                                        | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  |
| DOUBLE[(M, D)] | 8 字节                                        | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) |
| DECIMAL        | 对 DECIMAL(M,D) ，如果 M>D，为 M+2 否则为 D+2 | 依赖于 M 和 D 的值                                           | 依赖于 M 和 D 的值                                           |

其中 M 指数字总位数，D 指小数点后的位数

### 字符串类型

| 类型       | 大小               | 用途                            |
| :--------- | :----------------- | :------------------------------ |
| CHAR       | 0 - 2^8^ -1 字节   | 定长字符串                      |
| VARCHAR    | 0 - 2^16^ -1 字节  | 变长字符串                      |
| TINYBLOB   | 0 - 2^8^ - 1 字节  | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0 - 2^8^ - 1 字节  | 短文本字符串                    |
| BLOB       | 0 - 2^16^ - 1 字节 | 二进制形式的长文本数据          |
| TEXT       | 0 - 2^16^ - 1 字节 | 长文本数据                      |
| MEDIUMBLOB | 0 - 2^24^ - 1 字节 | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0 - 2^24^ - 1 字节 | 中等长度文本数据                |
| LONGBLOB   | 0 - 2^32^ - 1 字节 | 二进制形式的极大文本数据        |
| LONGTEXT   | 0 - 2^32^ - 1 字节 | 极大文本数据                    |

1GB=2^10^ MB=2^10^ * 2^10^ KB = 2^10^  * 2^10^  * 2^10^ byte =  2^10^  * 2^10^  * 2^10^  * 8 bit

日期和时间类型

| 类型      | 大小 (字节) | 范围                                                  | 格式                | 用途                     |
| :-------- | :---------- | :---------------------------------------------------- | :------------------ | :----------------------- |
| DATE      | 3           | 1000-01-01 / 9999-12-31                               | YYYY-MM-DD          | 日期值                   |
| TIME      | 3           | '-838:59:59' / '838:59:59'                            | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1           | 1901/2155                                             | YYYY                | 年份值                   |
| DATETIME  | 8           | 1000-01-01 00:00:00 / 9999-12-31 23:59:59             | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4           | '1970-01-01 00:00:01' UTC / '2038-01-19 03:14:07' UTC | YYYYMMDD HHMMSS     | 混合日期和时间值，时间戳 |

### 其他数据类型

BINARY、VARBINARY、ENUM、SET、Geometry、Point、MultiPoint、LineString、MultiLineString、Polygon、GeometryCollection 等

## 数据库表操作

### 创建数据库表

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

### 插入数据

`insert [into] 表名 values (值 1, 值 2, 值 3, ...);` 插入一条记录

`insert [into] 表名 [(列名 a, 列名 b, 列名 c)] values (值 1, 值 2, 值 3);` 只插入部分数据（列 a, b,c）

INSERT [INTO] 表名 SET 列名=...

INSERT [INTO] 表名 SELECT ...

### 查询数据

`select 列名称 from 表名称 [查询条件 ];` 根据一定查询规则到数据库中获取数据；

使用通配符 * 可查询表中所有的内容, 例如：

`select * from students;`

#### WHERE 关键词

`select 列名称 from 表名称 where 条件;` 指定查询条件；

`where` 后面可以接一般的比较运算符（ =、>、<、>=、<、!= ），扩展运算符（[not] null、in、like ），组合查询（or、and ）等条件查询方式。

例如：

`select * from students where sex="女";` 

#### SELECT 关键字

<div align="center"> <img src="https://github.com/dreamwhigh/Database-Notes/blob/master/docs/_media/0.png?raw=true " width="200"/> </div><br>

<div align="center"> <img src="https://github.com/dreamwhigh/Database-Notes/blob/master/docs/_media/1.png?raw=true " width="200"/> </div><br>

### 更新数据

`update 表名称 set 列名称=新值 where 更新条件;` update 语句用来修改表中的数据。

例如：

```
update students set tel=default where id=5;
update students set age=age+1;
update students set name="张伟鹏", age=19 where tel="13288097888";
```

### 删除数据

`delete from 表名称 where 删除条件;`



### 修改操作

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

## 连接

### 内连接

INNER JOIN

#### 自连接



### 外连接

#### 左外连接

LEFT OUTER JOIN

#### 右外连接

RIGHT OUTER JOIN

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

## 其他命令

```
SHOW CREATE TABLE 表名 #查看创建该表的命令情况，包括存储引擎（默认为 InnoDB）和编码方式（默认为 utf-8）

SHOW INDEXES FROM 表名 #查看索引该表索引

SELECT a INTO b;#赋值 b=a

```



### 难点

#### DELETE、TRUNCATE 和 DROP

[DELETE、TRUNCATE 和 DROP](<https://xiaozhuanlan.com/topic/6375814029>)

##### 作用

- DELETE 删除表中 WHERE 语句指定的数据
- TRUNCATE 清空表，相当于删除表中的所有数据
- DROP 删除表结构

##### 事务

- DELETE 会被放到日志中以便进行回滚
- TRUNCATE 和 DROP 立即生效，不会放到日志中，也就不支持回滚

##### 删除空间

- DELETE 不会减少表和索引占用的空间；
- TRUNCATE 会将表和索引占用的空间恢复到初始值；
- DROP 会将表和索引占用的空间释放。

##### 耗时

通常来说，DELETE < TRUNCATE < DROP。

## 参考资料

[21 分钟 MySQL 入门教程 ](<https://www.cnblogs.com/mr-wid/arcHive/2013/05/09/3068229.HTML#c2>)

[菜鸟驿站 MySQL 教程 ](<https://www.runoob.com/MySQL/MySQL-tutorial.HTML>)

[CS-Notes](<https://cyc2018.github.io/CS-Notes/#/notes/MySQL>)

[慕课网-与MySQL的零距离接触](<https://www.imooc.com/learn/122>)

