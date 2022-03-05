# 1.数据库

## 基本操作

```sql
SHOW TABLES;-- 显示数据库中所有的表
USE school
DESCRIBE student; -- 显示数据库中表中所有的信息
-- create database+数据库 单行注释
/*多行注释
hhknk
knk
hno*/ 
```
##  1.1 SQL 语句

sql 语句 可以分为以下三类：

- DDL(Data Definition Language,数据定义语言) 用来创建或者删除存储数据用的数据库以及数据库中的表的对象。DDL包含以下几种指令。

  - CREATE :创建数据库和表对象
  - DROP ：删除数据库和表对象的结构
  - ALTER: 修改数据库和表对象的结构
  
 - DML(Data Manipulation Language,数据操纵语言) 用来查询变更表中的记录。DML包含以下几种指令
      - SELECT : 查询表中的数据
      - INSERT : 向表中插入 新数据
      - UPDATE : 更新表中的数据
      - DELETE : 删除表中的数据

  - DCL(Data Control Language,数据控制语言) 用来确认或者取消对数据库中的数据库进行的变更。除此之外，还可以对RDBMS的用户是否有权限操作数据库中的的数据（数据库表等）进行设定。DCL包含以下几种指令。

      - COMMIT : 确认对数据库中的数据进行变更

      - ROLLBACK ： 取消对数据库中的数据进行变更

      - GRANT :赋予用户操作权限

      - REVOKE : 取消用户的操作权限

        

# 2.操作数据库

操作数据库>操作数据库中的表>操作数据库中表的数据

== mysql关键字不区分大小写 ==

 ## 2.1 操作数据库（了解）

1、创建数据库

```mysql
1 CREATE DATABASE [IF NOT EXISTS] school;
```


2、删除数据库

```mysql
2 DROP DATABASES [IF EXISTS] hello;
```

3、使用数据库

```mysql
3 use `school`; -- ''tab键的上面 如果表名或者字段名是一个特殊字符就需要用 ` `
```

4、 查看数据库

```mysql
show databases -- 查看数据库
```

## 2.2 数据库的列类型

>数值 

- `tinyint  十分小的数据   1个字节`
- `smallint  较小的数据     2个字节`
- `mediumint 中等大小的数据  3个字节`
- `int   标准的整数 4个字节 常用的 int`
- `bigint   较大的数据  8个字节`
- `float  浮点数 4个字节`
- `double 浮点数  8个字节 （精度问题)`
- `decimal 字符串形式的浮点数 金融（银行）计算的时候`

>字符串

- char  字符串固定大小的 0~255
- **varchar  可变字符串 0~65535 常用的变量 String**
- tinytext  微型文本 2^8-1
- **text  文本串 2^16-1  保存大文本**

>时间日期

 `java.util.Date`

- date 	YYYY-MM-DD,日期格式
- time        HH: mm: ss 时间格式
- **datetime YYYY-MM-DD HH: mm: ss最常用的时间格式**
- timestamp  时间戳 ，
- year 年份表示

>null

- 没有值 ，未知
- 注意，不要使用null进行运算



## 2.3、 数据库的字段属性

> <font color=#FF0000>  Unsigened </font>

- 无符号的整数
- 声明该列不能为负数

> `Auto Incr`

- 自动在上一条记录基础上+1（默认）
- 通常用来设计唯一的主键 ~index，必须是整数类型
- 可以自定义设计主键自增的开始值和步长

> 非空 NULL not

## 2.4 创建数据库表

```mysql
-- 目标：创建数据库表 student1
CREATE TABLE `student1` (
  `id` int(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
  `name` varchar(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
  `pwd` varchar(20) NOT NULL DEFAULT '123456' COMMENT '密码',
  `sex` varchar(2) NOT NULL DEFAULT '女' COMMENT '性别',
  `birthday` datetime DEFAULT NULL COMMENT '出生日期',
  `address` varchar(100) DEFAULT NULL COMMENT '家庭住址',
  `email` varchar(50) DEFAULT NULL COMMENT '邮箱',
  PRIMARY KEY (`id`) -- 主键只有一个
) ENGINE=InnoDB DEFAULT CHARSET=utf8

show create database school --查看定义的数据库school的语句
show create table student --查看student数据库表的定义语句
desc student --显示表的结构

-- 
```

> `设置数据库表的字符集编码`
>
> `1 CHARSET=utf8  不设置的话 MySQL的默认编码是Latin1，不支持中文`
>
> `2 在my.ini 中配置默认的编码 character-set-server=utf8 一般选用第一种`

## 2.5 修改删除表

- 修改表

  ``` sql
  -- 修改表名 ALTER TABLE 旧表名  RENAME AS 新表名
  ALTER TABLE student RENAME AS student2
  -- 增加表的字段 ALTER TABLE 表名 ADD 字段名 列属性
  ALTER TABLE student2 ADD age INT(11)
  
  -- modify 是修改约束 比如字符类型 int-->varchar
  -- change 是将某个字段名字重命名 age-->age1
  ALTER TABLE student2 MODIFY age VARCHAR(11) -- 修改约束
  ALTER TABLE student2 CHANGE age age1 INT(11) -- 字段重命名
  
  ALTER TABLE student2 DROP age1 -- 删除表的字段
  ```

- 删除

  ``` sql
  DROP TABLE IF EXISTS student2 -- 删除表如果存在
  ```

  **<font color=#FF0000 >所有的创建和删除操作尽量加上判断，以免报错~</font>**
# 3. MySQL 数据管理

  ## 3.1 外键 （了解）

```sql
-- 学生表的gradeid 字段 要去引用年纪表的graddeid
-- 定义外键key 
-- 给这个外键添加约束（执行引用) references 引用

CREATE TABLE `grade`(
`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
`gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
PRIMARY KEY (`gradeid`)

)ENGINE = INNODB DEFAULT CHARSET=utf8

CREATE TABLE IF NOT EXISTS `student1`(
`id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
`namme` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
`pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
`sex` VARCHAR(2) NOT NULL DEFAULT '男' COMMENT '性别',
`birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
`gradeid` INT(10) NOT NULL COMMENT '学生的年级',
`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
PRIMARY KEY(`id`)
)ENGINE = INNODB DEFAULT CHARSET=utf8

-- 创建表的时候没有外键关系 
ALTER TABLE `student1` 
ADD CONSTRAINT `FK_gradeid`  FOREIGN KEY(`gradeid`) REFERENCES `grade`(`gradeid`);

-- ALTER TABLE 表 ADD CONSTRAINT 约束名 FOREIGN KEY(作为外键的列) REFERENCES 哪个表（哪个字段）
```

以上操作属于物理外键，数据库级别的外键，不建议使用。而且删除表的时候，只能先删除student1 再删除gradeid表。数据库过多，删除就造成了困扰。

最佳实践：

- 数据库就是单纯的表，只用来存储数据，只有行（数据）和列（字段）

- 我们想要使用多张表的数据，想使用外键（程序实现）

  

  ## 3.2 DML语言（全部记住）

数据库的意义： 数据存储，数据管理

DML：

- insert
- update
- delete





## 3.3 添加(insert)

```sql
-- 插入语句（添加）
INSERT INTO `grade`(`gradename`) 
VALUES('大一'),('大二')
-- 插入语句一定要一一对应

INSERT INTO `student1`(`namme`,`pwd`,`sex`)
VALUES('李四','aaaab','男'),('张三','ajfk','女')

INSERT INTO `student1`(`namme`)
VALUES('张三'),('王五')

INSERT INTO `student1`
VALUES('6','李','abab','女','2011-2-2','fja3','正阳','eainl')
-- 必须写默认id'6' 而且要对应表中的每一个字段

```

语法： insert into 表名([字段名1，字段名2，字段名3]) values('值1'),('值2'),('值3',.....)

注意事项：

1，字段和字段之间使用英文逗号 隔开

2，字段是可以省略的，但是后面的值必须要一一对应，不能少

3，同时插入多条数据，VALUES后面的值，需要使用 , 隔开 

``` sql
VALUES(),(),()...
```

## 3.4 修改(update)

``` sql
-- 修改学员的名字，带了条件 
UPDATE `student1` SET `namme` = 'li' WHERE `pwd` = 'ajfk';

-- 不指定条件的情况下，会将所有的进行改动！不建议使用 使用直接跑路吧！！！
UPDATE `student1` SET `name` = '长江7号';

-- 修改多个属性，逗号隔开
UPDATE `student1` SET `namme` = 'lit',`email`='11522344@qq.com' WHERE id = 1;

--语法：
-- update 表名 set column_name1= vslue1,column_name2 = value2,..... where [条件]
```

条件：where 字句 运算符 

操作值 会返回 布尔值

| 操作符           | 含义         | 范围       | 结果  |
| :--------------- | ------------ | ---------- | ----- |
| =                | 等于         | 5=6        | false |
| <>或！=          | 不等         | 5<>6       | true  |
| >                |              |            |       |
| <                |              |            |       |
| <=               |              |            |       |
| >=               |              |            |       |
| BETWEEN...AND... | 在某个范围内 | [2,5]      |       |
| AND              | 我和你 &&    | 5>1AND1>2  | false |
| OR               | 我或你  \|\| | 5>1 OR 1>2 | true  |

```sql
-- 通过多个条件定位数据
UPDATE `stuent1` SET `namme` = '007' WHERE `name` = 'lit' AND `sex` = '男'
```

- 语法： update 表名 set column_name1= vslue1,column_name2 = value2,..... where [条件]

- 条件，筛选的条件，如果没有指定则会修改所有的列

- value ,是一个具体的值，也可以是一个变量 例如：`birthday` = current_date 等于当前时间

  ```sql
  
  UPDATE `student1` SET `birthday` = CURRENT_DATE WHERE `namme`= 'lit' AND `sex`='男';
  ```

  

## 3.5删除(delete)

> delete命令

语法： delete from 表名 [where 条件]

```sql
-- 删除数据(避免这样写，会全部删除)
DELETE  FROM `student`

-- 删除指定数据
DELETE FROM `student` WHERE id = 1;
```

> TRUNCATE 命令
>
> 完全清空数据库表，表的结构和索引约束不会变

```sql
-- 测试delete和truncate区别
CREATE TABLE `test`(
 `id` INT(4) NOT NULL AUTO_INCREMENT,
 `coll` VARCHAR(20) NOT NULL,
 PRIMARY KEY(`id`)
 )ENGINE = INNODB DEFAULT CHARSET = utf8
 
 INSERT INTO `test`(`coll`) VALUES('1'),('2'),('a')

 DELETE FROM `test` -- 不会影响自增 删除原来的数据 在插入自增会在原有基础上增加
 TRUNCATE TABLE `test` -- 自增会归零
```

DELETE 删除的问题，***重启数据库***，现象

- InnoDB引擎 重启数据库 自增列会重1开始（存在内存当中的，断电即失去）
- MyISAM引擎 继续从上一个 自增量开始（存在文件中的，不会失去）

# 4. DQL查询数据（最重点）

## 4.1 DQL

(Data Query Language: 数据查询语言)

- 所有的查询操作都用它 Select

- 简单的查询，复杂的查询它都能做

- 数据库中最核心的语言，最重要的语句

- 使用频率最高的语句

## 4.2 指定查询字段

```sql
-- 查询全部的学生 select 字段 from 表
SELECT * FROM student

-- 查询指定字段
SELECT `studentno`,`studentname` FROM student

-- 别名，给结果的字段起一个名字 AS 可以给字段起别名 也可以给表起别名
SELECT `studentno` AS 学号,`studentname` AS 姓名 FROM student

-- 函数 Concat(a,b)
SELECT CONCAT('姓名： ',studentname) AS 新名字 FROM student
```

> 语法： SELECT 字段 ，....FROM 表
>
> 有的时候，列名字不是那么的见名知意。我们起别名 字段名 AS 别名  表名 AS 别表名

> 去重 ： distinct

```sql
-- 查询一下 有哪些同学参加了考试，成绩表result
SELECT * FROM result
SELECT `studentno` FROM result -- 查询有哪些同学参加了考试

SELECT DISTINCT `studentno` FROM result -- 重复数据 去重
```

作用 ：去除SELECT 查询出来的结果中的重复数据，重复的数据只显示一条，并不改变 表中的重复的数据

> 数据库的列（表达式）

```SQL
SELECT VERSION()-- 查询版本 （函数）
SELECT 100*2-1 AS 计算结果 -- 用来计算 （计算表达式）
SELECT @@auto_increment_increment -- 查询自增的步长（变量）

-- 学员考试成绩+1分查看
SELECT `studentno`,`studentresult`+1 AS '加一分后' FROM result
```

数据库的表达式：文本值，列，null，函数，计算表达式，系统变量。。。

select  表达式  from  表

## 4.3 where 条件字句

作用：检索数据中符合条件的值

搜索的条件有一个或者多个表达式组成！结果 布尔值

> 逻辑运算符

| 运算符    | 语法          | 描述                             |
| --------- | ------------- | -------------------------------- |
| and    && | a and b  a&&b | 逻辑与，俩个都为真，结果为真     |
| or \|\|   | a or b a\|\|b | 逻辑或 其中一个为真，则结果为 真 |
| Not !     | not a  !a     | 逻辑非                           |

尽量使用英文

```SQL
------WHERE-----
SELECT studentno,studentresult FROM result

-- 查询考试成绩在95分 到 100分之间的
SELECT studentno,studentresult FROM result
WHERE studentresult>=95 AND studentresult<=100
-- and &&
SELECT studentno,studentresult FROM result
WHERE studentresult>=95 && studentresult<=100
-- 模糊查询（区间）between and
SELECT studentno,studentresult FROM result
WHERE studentresult BETWEEN 95 AND 100


-- 除了1000 号同学之外的同学的成绩

SELECT studentno,studentresult FROM result
WHERE subjectno != 1

-- != not
SELECT studentno,studentresult FROM result
WHERE  NOT subjectno=1
```

> 模糊查询 ：比较运算符

| 运算符      | 语法                | 描述                            |
| ----------- | ------------------- | ------------------------------- |
| IS NULL     | a is null           | 如果操作符为null,结果为真       |
| IS NOT NULL | a is not null       | 如果操作符不为null，结果为真    |
| BETWEEN     | a between b and c   | 若a 在b和c之间，结果为真        |
| **Like**    | a like b            | sql 匹配，如果a匹配b,则结果为真 |
| **In**      | a in (a1,a2,a3....) | 假设a在                         |

``` sql
-- 模糊查询
-- 查询姓张的同学
-- like结合 %（代表0到任意个字符）  —（代表一个字符）
SELECT `studentno`,`studentname` FROM `student`
WHERE studentname LIKE '张%'

-- 查询 姓赵的同学，后面有一个字的，有俩字 加俩个"- -"
SELECT `studentno`,`studentname` FROM `student`
WHERE studentname LIKE '赵_'


SELECT `studentno`,`studentname` FROM `student`
WHERE studentname LIKE '%强%'

-- ----- in (具体的一个或者多个值确定的 不能用模糊）-----
-- 查询1001,1002,1003学员
SELECT `studentno`,`studentname` FROM `student`
WHERE studentno IN(1001,1002,1003);

-- 查询在北京的同学
SELECT `studentno`,`studentname` FROM `student`
WHERE address IN ('北京朝阳');

--  ------ null not null------
-- 查询地址为空的学生
SELECT `studentno`,`studentname` FROM `student`
WHERE address IS NULL

-- 查询有出生日期的学生 不为空
SELECT `studentno`,`studentname` FROM `student`
WHERE borndate IS NOT NULL
```

## 4.4  多表索引

```sql

/* 查询学生的学号、姓名、科目编号、分数
思路：
1，分析需求： 分析查询的字段来自哪些表（连接查询）
2，确定使用哪种连接查询？ 7种
确定交叉点（这俩个表中哪个数据是相同的）
判断的条件：学生表中的 studentno = 成绩表 studentno
*/
-- join on(on后面跟判断的条件） 连接查询
-- where 等值查询

-- inner join
SELECT s.studentno,studentname,subjectno,studentresult -- 因为studentno在俩个表中都有 要明确哪个表中取
FROM student AS s
INNER JOIN result AS r
WHERE s.studentno = r.studentno

-- left join  学生成绩为空的也查出来了
SELECT s.studentno,studentname,subjectno,studentresult
FROM student s -- as 可以省略
LEFT JOIN result r
ON s.studentno = r.studentno

-- 查询缺考的同学
SELECT s.studentno,studentname,subjectno,studentresult
FROM student s -- as 可以省略
LEFT JOIN result r
ON s.studentno = r.studentno
WHERE studentresult IS NULL

-- right join  学生成绩为空的没有被查出来
SELECT s.studentno,studentname,subjectno,studentresult
FROM student s -- as 可以省略
RIGHT JOIN result r
ON s.studentno = r.studentno

-- 区分左右 返回结果是根据 左右连接里的内容
SELECT s.studentno,studentname,subjectno,studentresult
FROM result r  -- as 可以省略
RIGHT JOIN student s
ON s.studentno = r.studentno

```

| 操作       | 解释                                           |
| ---------- | ---------------------------------------------- |
| inner join | 如果表中至少有一个匹配，就返回                 |
| left join  | 会从左表的返回所有的值，即使右表中没有相应的值 |
| right join | 会从右表返回所有的值，即使左表没有相应的值     |

``` sql
-- 思考题 （查询参加考试同学的：学号、姓名、科目名、分数）
/*思路：
1，分析需求： 分析查询的字段来自哪些表:student/result/subject（连接查询）
2，确定使用哪种连接查询？ 7种
确定交叉点（这ji个表中哪个数据是相同的）
判断的条件：学生表中的 studentno = 成绩表 studentno 
成绩表 subjectno = 科目表 subjectno
本题涉及三表连接
*/
SELECT s.studentno,studentname,subjectname,studentresult
FROM student s
RIGHT JOIN result r
ON r.studentno=s.studentno

-- 连接第三张表
INNER JOIN `subject` AS sub -- subject 为关键字 但是在本题需要加``表示数据表
ON r.subjectno=sub.subjectno

```

> 解题思路：
>
> 1,我要查询哪些数据 select...
>
> 2,从哪几张表中查 from表 XXX Join 连接的表 on 交叉条件（找到俩张表的共同属性）
>
> 3，假设存在一种多表查询，慢慢来，先查询俩张表然后在慢慢增加

> 自连接：
>
> 自己的表和自己的表连接 核心：一张表拆为俩张一样的表

``` sql
CREATE TABLE `category`(
 `categoryid` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主题id',
 `pid` INT(10) NOT NULL COMMENT '父id',
 `categoryname` VARCHAR(50) NOT NULL COMMENT '主题名字',
PRIMARY KEY (`categoryid`) 
 ) ENGINE=INNODB  AUTO_INCREMENT=9 DEFAULT CHARSET=utf8; 

INSERT INTO `category` (`categoryid`, `pid`, `categoryname`) 
VALUES ('2','1','信息技术'),
('3','1','软件开发'),
('5','1','美术设计'),
('4','3','数据库'),
('8','2','办公信息'),
('6','3','web开发'),
('7','5','ps技术');
```



父类：

| categoryid | categoryName |
| ---------- | ------------ |
| 2          | 信息技术     |
| 3          | 软件开发     |
| 5          | 美术设计     |

子类：

| pid(父类的id) | categoryid | categoryName |
| ------------- | ---------- | ------------ |
| 3             | 4          | 数据库       |
| 2             | 8          | 办公信息     |
| 3             | 6          | web开发      |
| 5             | 7          | PS技术       |

| 父类     | 子类            |
| -------- | --------------- |
| 信息技术 | 办公信息        |
| 软件开发 | 数据库、web开发 |
| 美术设计 | PS技术          |

``` sql
-- 查询父子信息  
SELECT  a.categoryname AS '父栏目',b.categoryname AS '子栏目'
FROM category AS a ,category AS b
WHERE a.categoryid = b.`pid`
```

> 查询练习

``` sql
-- 查询学员所属的年级（学号，学生的姓名，年级名称）
SELECT studentno,studentname,gradename
FROM student s
INNER JOIN grade g
ON s.gradeid = g.gradeid

-- 查询科目所属年级（科目名称，年级名称）
SELECT gradename,subjectname
FROM grade g
INNER JOIN `subject` s
ON s.gradeid = g.gradeid

-- 查询参加了 高等数学-2 考试的同学的信息：学号、姓名、科目名称、科目成绩
SELECT s.studentno,studentname,subjectname,studentresult
FROM student s
INNER JOIN `subject` sub
ON s.gradeid = sub.gradeid
-- 连接第三个表
INNER JOIN result r
ON sub.subjectno = r.subjectno
在查询完成的条件下 在进行特定条件的查询
WHERE subjectname = '高等数学-2'
```

## 4.5 分页和排序

> 排序 和 分页
>
> -- --------分页 limit 和 排序 order by
> -- 排序 ：升序 ASC 降序：DESC
> -- ORDER BY 通过那个字段排序，怎么排
> -- 查询的结果根据 成绩降序 排序

``` sql
-- 100万 数据
-- 为什么要分页
-- 缓解数据库压力，给人的体验感更好。瀑布流：属于无限加载

-- 查询参加了 高等数学-2 考试的同学的信息：学号、姓名、科目名称、科目成绩
SELECT s.studentno,studentname,subjectname,studentresult
FROM student s
INNER JOIN `subject` sub
ON s.gradeid = sub.gradeid

INNER JOIN result r
ON sub.subjectno = r.subjectno

WHERE subjectname = '高等数学-2'
ORDER BY studentresult ASC -- 排序

LIMIT 0,5 -- 参数：第一个代表其实数据 第二个一个页面有多少条数据

-- 第一页 limit 0,5   (1-1) *5
-- 第二页 limit 5,5    (2-1)*5
-- 第N页  limit （n-1）*5  ，5   (n-1)*pageSize,pageSize
-- [pageSize:页面大小]
-- [(n-1) * pageSize : 起始值]
-- [n: 当前页]
-- [数据总数/页面大小 = 总页数]
```

# 5. MySQL 函数

## 5.1 常用函数

> 数据函数

``` sql
select ABS(-8); -- 绝对值
select ceiling(9.4); -- 向上取整，10
select floor(9.4); -- 向下取整，9
select rand(); -- 返回0-1 之间的随机数
select sign(0); -- 符号函数：负数返回-1，正数返回1,0返回0
```

> 字符串函数

``` sql
 SELECT CHAR_LENGTH('狂神说坚持就能成功'); /*返回字符串包含的字符数*/
 SELECT CONCAT('我','爱','程序');  /*合并字符串,参数可以有多个*/
 SELECT INSERT('我爱编程helloworld',1,2,'超级热爱');  /*替换字符串,从某个位置开始替换某个长度*/
 SELECT LOWER('KuangShen'); /*小写*/
 SELECT UPPER('KuangShen'); /*大写*/
 SELECT LEFT('hello,world',5);   /*从左边截取*/
 SELECT RIGHT('hello,world',5);  /*从右边截取*/
 SELECT REPLACE('狂神说坚持就能成功','坚持','努力');  /*替换字符串*/
 SELECT SUBSTR('狂神说坚持就能成功',4,6); /*截取字符串,开始和长度*/
 SELECT REVERSE('狂神说坚持就能成功'); /*反转
 
 -- 查询姓周的同学,改成邹
 SELECT REPLACE(studentname,'周','邹') AS 新名字
 FROM student WHERE studentname LIKE '周%';
```

> 日期和时间函数

``` sql
 SELECT CURRENT_DATE();   /*获取当前日期*/
 SELECT CURDATE();   /*获取当前日期*/
 SELECT NOW();   /*获取当前日期和时间*/
 SELECT LOCALTIME();   /*获取当前日期和时间*/
 SELECT SYSDATE();   /*获取当前日期和时间*/
 
 -- 获取年月日,时分秒
 SELECT YEAR(NOW());
 SELECT MONTH(NOW());
 SELECT DAY(NOW());
 SELECT HOUR(NOW());
 SELECT MINUTE(NOW());
 SELECT SECOND(NOW());
```

> 系统信息函数

``` sql
 SELECT VERSION();  /*版本*/
 SELECT USER();     /*用户*/
```

## 5.2 聚合函数（常用）

| 函数名称 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| COUNT()  | 返回满足Select条件的记录总和数，如 select count(*) 【不建议使用 *，效率低】 |
| SUM()    | 返回数字字段或表达式列作统计，返回一列的总和。               |
| AVG()    | 通常为数值字段或表达列作统计，返回一列的平均值               |
| MAX()    | 可以为数值字段，字符字段或表达式列作统计，返回最大的值。     |
| MIN()    | 可以为数值字段，字符字段或表达式列作统计，返回最小的值。     |

``` sql
 -- 聚合函数
 /*COUNT:非空的*/
 SELECT COUNT(studentname) FROM student;
 SELECT COUNT(*) FROM student;
 SELECT COUNT(1) FROM student;  /*推荐*/
 
 -- 从含义上讲，count(1) 与 count(*) 都表示对全部数据行的查询。
 -- count(字段) 会统计该字段在表中出现的次数，忽略字段为null 的情况。即不统计字段为null 的记录。
 -- count(*) 包括了所有的列，相当于行数，在统计结果的时候，包含字段为null 的记录；
 -- count(1) 用1代表代码行，在统计结果的时候，包含字段为null 的记录 。
 /*
 很多人认为count(1)执行的效率会比count(*)高，原因是count(*)会存在全表扫描，而count(1)可以针对一个字段进行查询。其实不然，count(1)和count(*)都会对全表进行扫描，统计所有记录的条数，包括那些为null的记录，因此，它们的效率可以说是相差无几。而count(字段)则与前两者不同，它会统计该字段不为null的记录条数。
 
 下面它们之间的一些对比：
 
 1）在表没有主键时，count(1)比count(*)快
 2）有主键时，主键作为计算条件，count(主键)效率最高；
 3）若表格只有一个字段，则count(*)效率较高。
 */
 
 SELECT SUM(StudentResult) AS 总和 FROM result;
 SELECT AVG(StudentResult) AS 平均分 FROM result;
 SELECT MAX(StudentResult) AS 最高分 FROM result;
 SELECT MIN(StudentResult) AS 最低分 FROM result;
```

> 题目 

``` sql
-- 查询不同课程的平均分,最高分,最低分
 -- 前提:根据不同的课程进行分组
 
 SELECT subjectname,AVG(studentresult) AS 平均分,MAX(StudentResult) AS 最高分,MIN(StudentResult) AS 最低分
 FROM result AS r
 INNER JOIN `subject` AS s
 ON r.subjectno = s.subjectno
 GROUP BY r.subjectno
 HAVING 平均分>80; -- 此题用HAVING 
 
 /*
 where写在group by前面.
 要是放在分组后面的筛选
 要使用HAVING..
 因为having是从前面筛选的字段再筛选，而where是从数据表中的>字段直接进行的筛选的
 */
```

## 5.3 数据库级别的MD5加密

什么是MD5？

主要增强算法复杂度和不可逆性

MD5不可逆，具体的值得MD5是一样的

MD5 破解网站的原理，背后有一个字典，MD5加密后的值，加密前的值，一一地对应

> MD5即Message-Digest Algorithm 5（信息-摘要算法5），用于确保信息传输完整一致。是计算机广泛使用的杂凑算法之一（又译摘要算法、哈希算法），主流编程语言普遍已有MD5实现。将数据（如汉字）运算为另一固定长度值，是杂凑算法的基础原理，MD5的前身有MD2、MD3和MD4。

``` sql
-- 新建一个表 testmd5
 CREATE TABLE `testmd5` (
  `id` INT(4) NOT NULL,
  `name` VARCHAR(20) NOT NULL,
  `pwd` VARCHAR(50) NOT NULL,
  PRIMARY KEY (`id`)
 ) ENGINE=INNODB DEFAULT CHARSET=utf8
 
 -- 插入一些数据
  INSERT INTO testmd5 VALUES(1,'kuangshen','123456'),(2,'qinjiang','456789')
  
  -- 如果我们要对pwd这一列数据进行加密，语法是：
   update testmd5 set pwd = md5(pwd);
   
   -- 如果单独对某个用户(如kuangshen)的密码加密：
   INSERT INTO testmd5 VALUES(3,'kuangshen2','123456')
 update testmd5 set pwd = md5(pwd) where name = 'kuangshen2';
 
 -- 插入新的数据自动加密
   INSERT INTO testmd5 VALUES(4,'kuangshen3',md5('123456'));
   
 -- 查询登录用户信息（md5对比使用，查看用户输入加密后的密码进行比对）
  SELECT * FROM testmd5 WHERE `name`='kuangshen' AND pwd=MD5('123456');
```

#  6 .事务

## 6.1 什么是事务

- 事务就是将一组SQL语句放在同一批次内去执行

要么都成功，要么都失败：

--------

1、SQL执行 A给B转账 A 1000 --->200   B 200

2、 SQL执行 B收到A 的钱 A 800 --> B 400

-----

> 事务原则：ACID原则： 原子性，一致性，隔离性，持久性   （脏读 幻读）

参考链接 https://blog.csdn.net/dengjili/article/details/82468576

**原子性（Atomicity)**

要么都成功，要么都失败

**一致性（consistency）**

事务前后的数据完整性要保证一致

**持久性（Durability)**

事务一旦提交则不可逆，被持久化到数据库中

**隔离性（Isolation)**

事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务

操作所干扰，事务之间要相互隔离。

> 隔离所导致的问题

**脏读：**

指一个事务读取了另一个事务未提交的数据

**不可重复读：**

在一个事务内读取表中的某一行数据，多次读取结果不同。（这个不一定是错误，只是某些场合不对）

**幻读（虚度）：**

是指在一个事务内读取到了别的事务插入的数据，导致前后读取的不一致

> 执行事务

``` sql
-- ======================事务==================

-- MySQL 是默认开启事务自动提交的
SET autocommit = 0 -- 关闭
SET autocommit = 1 -- 开启（默认的）

-- 手动处理事务
 -- 事务开启
 START TRANSACTION  -- 标记一个事务的开始，从这个 之后的SQL都在一个事务内
 
 INSERT xx
 INSERT xx
 
 -- 提交： 持久化 （成功！）
 COMMIT
 -- 回滚：回到原来的样子（失败！）
 ROLLBACK
 
 -- 事务结束
 SET autocommit = 1 -- 开启自动提交
 
 
 -- 了解
 SAVEPOINT 保存点名 -- 设置一个事务的保存点
 ROLLBACK TO SAVEPOINT 保存点名 -- 回滚到保存点
 RELEASE SAVEPOINT 保存点名 -- 撤销保存点
```

> 模拟事务执行

``` sql
 -- 转账
 CREATE DATABASE  shop1 CHARACTER SET utf8 COLLATE utf8_general_ci
 
 USE shop
 CREATE TABLE `account`(
`id` INT(3) NOT NULL AUTO_INCREMENT,
`name` VARCHAR(30) NOT NULL,
`money` DECIMAL(9,2) NOT NULL,
PRIMARY KEY (`id`)
) ENGINE = INNODB DEFAULT CHARSET =utf8

INSERT INTO account(`name`,`money`)
VALUES ('AA',2000.00),('B',10000.00)

-- 模拟转账：事务
SET autocommit = 0; -- 关闭自动提交
START TRANSACTION -- 开启一个事务（一组事务）

UPDATE account SET money = money-500 WHERE `name` = 'AA' -- 'A减五百元'
UPDATE account SET money = money+500 WHERE `name` = 'B' -- 'B加五百元’

COMMIT; -- 提交事务，就被持久化了
ROLLBACK; -- 回滚

SET autocommit = 1; -- 恢复默认值

```

# 7 .索引

> 索引的作用

- 提高查询速度
- 确保数据的唯一性
- 可以加速表和表之间的连接 , 实现表与表之间的参照完整性
- 使用分组和排序子句进行数据检索时 , 可以显著减少分组和排序的时间
- 全文检索字段进行搜索优化.

> 分类

- 主键索引 (Primary Key)
- 唯一索引 (Unique)
- 常规索引 (Index)
- 全文索引 (FullText)

> 主键索引

主键 : 某一个属性组能唯一标识一条记录

特点 :

- 最常见的索引类型
- 确保数据记录的唯一性
- 确定特定数据记录在数据库中的位置

> 唯一索引

作用 : 避免同一个表中某数据列中的值重复

与主键索引的区别

- 主键索引只能有一个
- 唯一索引可能有多个

```sql
CREATE TABLE `Grade`(
  `GradeID` INT(11) AUTO_INCREMENT PRIMARYKEY,
  `GradeName` VARCHAR(32) NOT NULL UNIQUE
   -- 或 UNIQUE KEY `GradeID` (`GradeID`)
)
```

> 常规索引

作用 : 快速定位特定数据

注意 :

- index 和 key 关键字都可以设置常规索引
- 应加在查询找条件的字段
- 不宜添加太多常规索引,影响数据的插入,删除和修改操作

```SQL
CREATE TABLE `result`(
   -- 省略一些代码
  INDEX/KEY `ind` (`studentNo`,`subjectNo`) -- 创建表时添加
)
-- 创建后添加
ALTER TABLE `result` ADD INDEX `ind`(`studentNo`,`subjectNo`);
```

> 全文索引

百度搜索：全文索引

作用 : 快速定位特定数据

注意 :

- 只能用于MyISAM类型的数据表
- 只能用于CHAR , VARCHAR , TEXT数据列类型
- 适合大型数据集

```SQL
/*
#方法一：创建表时
  　　CREATE TABLE 表名 (
               字段名1 数据类型 [完整性约束条件…],
               字段名2 数据类型 [完整性约束条件…],
               [UNIQUE | FULLTEXT | SPATIAL ]   INDEX | KEY
               [索引名] (字段名[(长度)] [ASC |DESC])
               );


#方法二：CREATE在已存在的表上创建索引
       CREATE [UNIQUE | FULLTEXT | SPATIAL ] INDEX 索引名
                    ON 表名 (字段名[(长度)] [ASC |DESC]) ;


#方法三：ALTER TABLE在已存在的表上创建索引
       ALTER TABLE 表名 ADD [UNIQUE | FULLTEXT | SPATIAL ] INDEX
                            索引名 (字段名[(长度)] [ASC |DESC]) ;
                           
                           
#删除索引：DROP INDEX 索引名 ON 表名字;
#删除主键索引: ALTER TABLE 表名 DROP PRIMARY KEY;


#显示索引信息: SHOW INDEX FROM student;
*/

/*增加全文索引*/
ALTER TABLE `school`.`student` ADD FULLTEXT INDEX `studentname` (`StudentName`);

/*EXPLAIN : 分析SQL语句执行性能*/
EXPLAIN SELECT * FROM student WHERE studentno='1000';

/*使用全文索引*/
-- 全文搜索通过 MATCH() 函数完成。
-- 搜索字符串作为 against() 的参数被给定。搜索以忽略字母大小写的方式执行。对于表中的每个记录行，MATCH() 返回一个相关性值。即，在搜索字符串与记录行在 MATCH() 列表中指定的列的文本之间的相似性尺度。
EXPLAIN SELECT *FROM student WHERE MATCH(studentname) AGAINST('love');

/*
开始之前，先说一下全文索引的版本、存储引擎、数据类型的支持情况

MySQL 5.6 以前的版本，只有 MyISAM 存储引擎支持全文索引；
MySQL 5.6 及以后的版本，MyISAM 和 InnoDB 存储引擎均支持全文索引;
只有字段的数据类型为 char、varchar、text 及其系列才可以建全文索引。
测试或使用全文索引时，要先看一下自己的 MySQL 版本、存储引擎和数据类型是否支持全文索引。
*/
```

> 扩展：测试索引

**建表app_user：**

```SQL
CREATE TABLE `app_user` (
`id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
`name` varchar(50) DEFAULT '' COMMENT '用户昵称',
`email` varchar(50) NOT NULL COMMENT '用户邮箱',
`phone` varchar(20) DEFAULT '' COMMENT '手机号',
`gender` tinyint(4) unsigned DEFAULT '0' COMMENT '性别（0:男；1：女）',
`password` varchar(100) NOT NULL COMMENT '密码',
`age` tinyint(4) DEFAULT '0' COMMENT '年龄',
`create_time` datetime DEFAULT CURRENT_TIMESTAMP,
`update_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='app用户表'
```

**批量插入数据：100w**

```SQL
DROP FUNCTION IF EXISTS mock_data;
DELIMITER $$
CREATE FUNCTION mock_data()
RETURNS INT
BEGIN
DECLARE num INT DEFAULT 1000000;
DECLARE i INT DEFAULT 0;
WHILE i < num DO
  INSERT INTO app_user(`name`, `email`, `phone`, `gender`, `password`, `age`)
   VALUES(CONCAT('用户', i), '24736743@qq.com', CONCAT('18', FLOOR(RAND()*(999999999-100000000)+100000000)),FLOOR(RAND()*2),UUID(), FLOOR(RAND()*100));
  SET i = i + 1;
END WHILE;
RETURN i;
END;
SELECT mock_data();
```

**索引效率测试**

无索引

```SQL
SELECT * FROM app_user WHERE name = '用户9999'; -- 查看耗时
SELECT * FROM app_user WHERE name = '用户9999';
SELECT * FROM app_user WHERE name = '用户9999';

mysql> EXPLAIN SELECT * FROM app_user WHERE name = '用户9999'\G
*************************** 1. row ***************************
          id: 1
select_type: SIMPLE
       table: app_user
  partitions: NULL
        type: ALL
possible_keys: NULL
        key: NULL
    key_len: NULL
        ref: NULL
        rows: 992759
    filtered: 10.00
      Extra: Using where
1 row in set, 1 warning (0.00 sec)
```

创建索引

```SQL
CREATE INDEX idx_app_user_name ON app_user(name);
```

测试普通索引

```SQL
mysql> EXPLAIN SELECT * FROM app_user WHERE name = '用户9999'\G
*************************** 1. row ***************************
          id: 1
select_type: SIMPLE
       table: app_user
  partitions: NULL
        type: ref
possible_keys: idx_app_user_name
        key: idx_app_user_name
    key_len: 203
        ref: const
        rows: 1
    filtered: 100.00
      Extra: NULL
1 row in set, 1 warning (0.00 sec)

mysql> SELECT * FROM app_user WHERE name = '用户9999';
1 row in set (0.00 sec)

mysql> SELECT * FROM app_user WHERE name = '用户9999';
1 row in set (0.00 sec)

mysql> SELECT * FROM app_user WHERE name = '用户9999';
1 row in set (0.00 sec)
```

> 索引准则

- 索引不是越多越好
- 不要对经常变动的数据加索引
- 小数据量的表建议不要加索引
- 索引一般应加在查找条件的字段

> 索引的数据结构

```SQL
-- 我们可以在创建上述索引的时候，为其指定索引类型，分两类
hash类型的索引：查询单条快，范围查询慢
btree类型的索引：b+树，层数越多，数据量指数级增长（我们就用它，因为innodb默认支持它）

-- 不同的存储引擎支持的索引类型也不一样
InnoDB 支持事务，支持行级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
MyISAM 不支持事务，支持表级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
Memory 不支持事务，支持表级别锁定，支持 B-tree、Hash 等索引，不支持 Full-text 索引；
NDB 支持事务，支持行级别锁定，支持 Hash 索引，不支持 B-tree、Full-text 等索引；
Archive 不支持事务，支持表级别锁定，不支持 B-tree、Hash、Full-text 等索引；
```

# 8 .权限管理和备份

## 8.1权限管理

> 使用SQLyog创建用户，并授予权限演示

![1bf86eba70658e8c2d9ef4b805ccc22](C:\Users\li\AppData\Local\Temp\WeChat Files\1bf86eba70658e8c2d9ef4b805ccc22.png)

> 基本命令

``` sql
/* 用户和权限管理 */ ------------------
用户信息表：mysql.user

-- 刷新权限
FLUSH PRIVILEGES

-- 增加用户 CREATE USER kuangshen IDENTIFIED BY '123456'
CREATE USER 用户名 IDENTIFIED BY [PASSWORD] 密码(字符串)
  - 必须拥有mysql数据库的全局CREATE USER权限，或拥有INSERT权限。
  - 只能创建用户，不能赋予权限。
  - 用户名，注意引号：如 'user_name'@'192.168.1.1'
  - 密码也需引号，纯数字密码也要加引号
  - 要在纯文本中指定密码，需忽略PASSWORD关键词。要把密码指定为由PASSWORD()函数返回的混编值，需包含关键字PASSWORD

-- 重命名用户 RENAME USER kuangshen TO kuangshen2
RENAME USER old_user TO new_user

-- 设置密码
SET PASSWORD = PASSWORD('密码')    -- 为当前用户设置密码
SET PASSWORD FOR 用户名 = PASSWORD('密码')    -- 为指定用户设置密码

-- 删除用户 DROP USER kuangshen2
DROP USER 用户名

-- 分配权限/添加用户
GRANT 权限列表 ON 表名 TO 用户名 [IDENTIFIED BY [PASSWORD] 'password']
  - all privileges 表示所有权限
  - *.* 表示所有库的所有表
  - 库名.表名 表示某库下面的某表

-- 查看权限   SHOW GRANTS FOR root@localhost;
SHOW GRANTS FOR 用户名
   -- 查看当前用户权限
  SHOW GRANTS; 或 SHOW GRANTS FOR CURRENT_USER; 或 SHOW GRANTS FOR CURRENT_USER();

-- 撤消权限
REVOKE 权限列表 ON 表名 FROM 用户名
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 用户名    -- 撤销所有权限
```

> 权限解释

``` sql
-- 权限列表
ALL [PRIVILEGES]    -- 设置除GRANT OPTION之外的所有简单权限
ALTER    -- 允许使用ALTER TABLE
ALTER ROUTINE    -- 更改或取消已存储的子程序
CREATE    -- 允许使用CREATE TABLE
CREATE ROUTINE    -- 创建已存储的子程序
CREATE TEMPORARY TABLES        -- 允许使用CREATE TEMPORARY TABLE
CREATE USER        -- 允许使用CREATE USER, DROP USER, RENAME USER和REVOKE ALL PRIVILEGES。
CREATE VIEW        -- 允许使用CREATE VIEW
DELETE    -- 允许使用DELETE
DROP    -- 允许使用DROP TABLE
EXECUTE        -- 允许用户运行已存储的子程序
FILE    -- 允许使用SELECT...INTO OUTFILE和LOAD DATA INFILE
INDEX     -- 允许使用CREATE INDEX和DROP INDEX
INSERT    -- 允许使用INSERT
LOCK TABLES        -- 允许对您拥有SELECT权限的表使用LOCK TABLES
PROCESS     -- 允许使用SHOW FULL PROCESSLIST
REFERENCES    -- 未被实施
RELOAD    -- 允许使用FLUSH
REPLICATION CLIENT    -- 允许用户询问从属服务器或主服务器的地址
REPLICATION SLAVE    -- 用于复制型从属服务器（从主服务器中读取二进制日志事件）
SELECT    -- 允许使用SELECT
SHOW DATABASES    -- 显示所有数据库
SHOW VIEW    -- 允许使用SHOW CREATE VIEW
SHUTDOWN    -- 允许使用mysqladmin shutdown
SUPER    -- 允许使用CHANGE MASTER, KILL, PURGE MASTER LOGS和SET GLOBAL语句，mysqladmin debug命令；允许您连接（一次），即使已达到max_connections。
UPDATE    -- 允许使用UPDATE
USAGE    -- “无权限”的同义词
GRANT OPTION    -- 允许授予权限


/* 表维护 */

-- 分析和存储表的关键字分布
ANALYZE [LOCAL | NO_WRITE_TO_BINLOG] TABLE 表名 ...
-- 检查一个或多个表是否有错误
CHECK TABLE tbl_name [, tbl_name] ... [option] ...
option = {QUICK | FAST | MEDIUM | EXTENDED | CHANGED}
-- 整理数据文件的碎片
OPTIMIZE [LOCAL | NO_WRITE_TO_BINLOG] TABLE tbl_name [, tbl_name] ...
```

## 8.2 MySQL 备份

数据库备份必要性

- 保证重要数据不丢失
- 数据转移

MySQL数据库备份方法

- mysqldump备份工具
- 数据库管理工具,如SQLyog
- 直接拷贝数据库文件和相关配置文件

**mysqldump客户端**

作用 :

- 转储数据库
- 搜集数据库进行备份
- 将数据转移到另一个SQL服务器,不一定是MySQL服务器

``` sql
-- 导出
1. 导出一张表 -- mysqldump -uroot -p123456 school student >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表名 > 文件名(D:/a.sql)
2. 导出多张表 -- mysqldump -uroot -p123456 school student result >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 表1 表2 表3 > 文件名(D:/a.sql)
3. 导出所有表 -- mysqldump -uroot -p123456 school >D:/a.sql
　　mysqldump -u用户名 -p密码 库名 > 文件名(D:/a.sql)
4. 导出一个库 -- mysqldump -uroot -p123456 -B school >D:/a.sql
　　mysqldump -u用户名 -p密码 -B 库名 > 文件名(D:/a.sql)

可以-w携带备份条件

-- 导入
1. 在登录mysql的情况下：-- source D:/a.sql
　　source 备份文件
2. 在不登录的情况下
　　mysql -u用户名 -p密码 库名 < 备份文件
```

