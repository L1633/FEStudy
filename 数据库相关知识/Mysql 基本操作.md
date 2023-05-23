### Mysql
多对多的 情况 就需要一张中间表 来 关联！！！！！！！！！！！！

一对多 的情况， 多的那一方需要存储 一的 主键 关联！！！！！！！！

数据库设计 三范式
1 原子性
  数据表的每一列都是不可分割的基本数据项，同一列中不能有多个值，也不能存在重复的属性

2 唯一性
  数据表中的每一条记录必须式唯一的，为了实现区分，通常要为表加上一个列作为存储唯一表使，这个唯一属性被称作主键列

3 关联性
  每列都与主键有直接关系， 不存在传递依赖


### MySQL 中的字段约束共有四种


主键约束 PRIMARY KEY 字段值唯一，且不能为NULL
非空约束 NOT NULL 字段值不能为NULL
唯一约束 UNIQUE 字段值唯一，且可以为NULL
外键约束 FOREIGN KEY 保持关联数据的逻辑性 （不推荐）

外键约束 就是两张表中有字段的关联， 外键约束的定义是写在子表上的
它存子的问题就是 如果引用混乱，形成外键闭环， 那么就无法删除任何一张表的记录


### 索引
对数据进行排序， 查找数据就会翻倍

创建表的时候 设置索引
CREATE TABLE 表名称 (

  INDEX 【索引名称】（字段）,
)

对已经存在的表 设置索引
CREATE INDEX 索引名称 ON 表名(字段)
ALTER TABLE 表名称 ADD INDEX [索引名称](字段)
SHOW INDEX FROM 表名
DROP INDEX 索引名称 ON 表名



原理是 根据索引生成二叉树

索引的使用原则
◆ 数据量很大，而且经常被查询的数据表可以设置索引 （比如日志 就不需要设置索引，因为它很少去查询）
◆ 索引只添加在经常被用作检索条件的字段上面 （比如 姓名）
◆ 不要在大字段上创建索引 （比如长度比较大的 字符串 就不适合 设置索引）



### 普通查询 
SELECT * FROM 表名


别名 AS
SELECT key1, key2 AS k2  FROM 表名

查询语句的子句执行顺序
1 此法分析与优化 读取 SQL 语句
2 FROM 选择数据来源
3 SELECT 选择输出内容


### 数据分页 LIMIT
SELECT ..... FROM .... LIMIT 起始位置, 偏移量
执行顺序 FROM -> SELECT -> LIMIT

### 排序  默认升序 ASC
SELECT .... FROM .... ORDER BY 列名 [ASC(升序) | DESC(降序)], [列名 [ASC | DESC]];

排序 + 分页
ORDER BY 子句书写的时候放在 LIMIT 子句的前面
FROM -> SELECT -> ORDER BY -> LIMIT

### 去重
SELECT DISTINCT 字段 FROM .....

注意事项
◆使用DISTINCT的SELECT子句中只能查询一列数据，如果查询多列，
去除重复记录就会失效。
SELECT DISTINCT job,ename FROM t emp; # 失效

◆ DISTINCT关键字只能在SELECT子句中使用一次

SELECT DISTINCT job,DISTINCT ename FROM t emp; # 错误， 只能使用一次
SELECT ename,DISTINCT job FROM t emp; # 错误 DISTINCT 需要放在第一个字段

### 条件查询
SELECT .... FROM .... WHERE 条件 [AND | OR] 条件.....;

WHERE 语句中的条件运算符会用到一下 四种运算符
1 数学运算符
2 比较运算符
3 逻辑运算符
4 按位运算符

优化
 VHERE子句中，条件执行的顺序是从左到右的。所以我们应该把`索引条件`，或者`筛选掉记录最多`的条件写在最左侧


FROM -> WHERE -> SELECT -> ORDER BY -> LIMIT


### 聚合函数
可以对数据求和、求最大值 和 最小值、求平均值。
SUM、MAX、MIN、AVG、COUNT



COUNT函数
◆COUNT(*)用于获得`包含空值`的记录数，COUNT(列名)用于获得`包含非空值`的记录数。

聚合函数不能出现在 where 子句 中！


### 分组查询
GROUP BY 

对SELECT子句的要求

◆查询语句中如果含有GROUP BY子句，那么SELECT子句中的内容就
必须要遵守规定：SELECT子句中可以包括聚合函数，或者GROUP
BY子句的分组列，`其余内容均不可以出现在SELECT子句中`
正确
SELECT deptno,COUNT (*)AVG(sal)
FROM t emp GROUP BY deptno;

错误 出现了 sal， 它既不是 聚合函数， 也不是 GROUP BY 的分组列，
前面的 AVG(sal) 是正确的
SELECT deptno,COUNT (*),AVG(sal),sal
FROM t emp
GROUP BY deptno;

上面这种情形可以使用
GROUP CONCAT函数
◆GROUP CONCAT函数可以把分组查询中的某个字段拼接成一个字
符串
◆查询每个部门内底薪超过2000元的人数和员工姓名
SELECT deptno,GROUP CONCAT (ename),COUNT (*)
FROM t emp
WHERE sal>=2000
GROUP BY deptno;

FROM -> WHERE -> GROUP BY -> SELECT -> ORDER BY -> LIMIT


### HAVING 子句

Having 子句必须跟着 GROUP BY  使用，可以理解为 是一个 where 查询 条件。
GROUP BY ..... HAVING .....
◆查询每个部门中，1982年以后入职的员工超过2个人的部门编号
SELECT deptno FROM t emp
WHERE hiredate>="1982-01-01"
GROUP BY deptno HAVING COUNT (*)>=2;
ORDER BY deptno ASC;

能用 where 就用 where， 它会优先执行筛选数据， 只是它不能使用聚合函数的 情况下  才用 HAVING


### 表连接查询

表连接的分类
◆表连接分为两种：内连接和外连接
◆内连接是结果集中只保留符合连接条件的记录
◆外连接是不管符不符合连接条件，记录都要保留在结果集中

内连接的多种语法形式
SELECT FROM 表1 JOIN 表2 ON连接条件;
SELECT FROM 表1 JOIN 表2 WHERE 连接条件;
SELECT FROM 表1, 表2 WHERE 连接条件;

相同的数据表也可以做表连接
◆查询与SCOTT相同部门的员工都有谁？
SELECT e2.ename
FROM t_emp el JOIN t_emp e2
ON e1.deptno=e2.deptno
WHERE e1.ename="SCOTT" AND e2.ename !="SCOTT"

#查询底薪超过公司平均底薪的员工信息？
把查询的结果当作一个`临时表`
SELECT e.empno,e.ename,e.sal
FROM t_emp e JOIN
(SELECT AVG(sal) avg FROM t_emp) t
ON e.sal>=t.avg;

外连接

◆外连接与内连接的区别在于，除了符合条件的记录之外，结果集中
还会保留不符合条件的记录。
SELECT
  e.empno,e.ename,d.dname
FROM
  t_emp e
LEFT JOIN t_dept d ON e.deptno d.deptno;

左连接和右连接
◆左外连接就是保留左表所有的记录，与右表做连接。如果右表有符
合条件的记录就与左表连接。如果右表没有符合条件的记录，就用
NULL与左表连接。右外连接也是如此。


UNION关键字可以将多个查询语句的结果集进行合并
(查询语句) UNION (查询语句) UNION (查询语句)


外连接的注意事项
内连接只保留符合条件的记录，所以查询条件写在0N子句和
VHERE子句中的效果是相同的。但是外连接里，条件写在WHERE
子句里，不合符条件的记录是会被过滤掉的，而不是保留下来。


### 子查询

子查询的分类
◆子查询可以写在三个地方：VHERE子句、FROM子句、SELECT子
句，但是只有FROM子句子查询是最可取的

不推荐 在 WHERE 中使用 子查询， 效率低下

可以将子查询当作一张`临时表`用, 推荐 FROM 的 子查询

WHERE子查询
◆这种子查询最简单，最容易理解，但是却是效率很低的子查询
◆查询底薪超过公司平均底薪的员工的信息
SELECT
empno,ename,sal
FROM t emp
WHERE sal>=(SELECT AVG(sal)FROM t emp)

这个 子查询 比较每条记录都要重新执行子查询


FROM 子查询
这种子查询只会执行一次， 所以查询效率很高


SELECT 子查询
这种子查询每输出一条记录的时候都要执行一次，查询效率很低


单行子查询和多行子查询
◆单行子查询的结果集只有一条记录，多行子查询结果集有多行记录
◆多行子查询只能出现在 WHERE 子句 和 FROM 子句中

### INSERT
INSERT INTO 表名(字段1，字段2) VALUES(值1, 值2)

INSERT INTO 表名(字段1，字段2) VALUES(值1, 值2)，(值1, 值2)

方言
INSERT INTO 表名 SET 字段1=值1，字段2= 值2

### UPDATE
UPDATE [IGNORE] 表名
SET 字段1=值1，字段2=值2，
[WHERE 条件1.]
[ORDER BY .....]
[LIMIT …];


UPDATE语句的表连接（一）
◆因为相关子查询效率非常低，所以我们可以利用表连接的方式来改
造 UPDATE语句
UPDATE 表1 JOIN 表2 ON 条件
SET 字段1=值1，字段2=值2，…

◆UPDATE语句的表连接可以演变成下面的样子
UPDATE 表1，表2
SET 字段1=值1，字段2=值2，…
WHERE连接条件；


UPDATE语句的表连接（三）
◆UPDATE语句的表连接既可以是内连接，又可以是外连接
UPDATE 表1 [LEFT | RIGHT] JOIN 表2 ON条件
SET字段1=值1，字段2=值2，…；


### DELETE
DELETE [IGNORE] FROM 表名
[WHERE 条件1.]
[ORDER BY .....]
[LIMIT …];

执行顺序 FROM---- WHERE --- ORDER BY ---- LIMIT  -----  DELETE


因为相关子查询效率非常低，所以我们可以利用表连接的方式来改
造DELETE语句
DELETE 表1,... FROM 表1 JOIN 表2 ON 条件
[WHERE 条件1，条件2，…]
[ORDER BY......]
[LIMIT ......]


外连接
DELETE 表1,... FROM 表1 [LEFT | RIGHT] JOIN 表2 
ON 条件


快速删除数据表全部记录
◆DELETE语句是在事务机制下删除记录，删除记录之前，先把将要删
除的记录保存到日志文件里，然后再删除记录。
◆TRUNCATE语句在事务机制之外删除记录，速度远超过DELETE语句
TRUNCATE TABLE 表名；




### 事务机制
事务机制(Transaction)
◆RDBMS = SQL语句 + 事务(ACID)
事务是一个或者多个SQL语句组成的整体，要么全部执行成功，要
么全都执行失败

管理事务
◆默认情况下，M小ySQL执行每条SQL语句都会自动开启和提交事务
◆为了让多条SQL语句纳入到一个事务之下，可以手动管理事务
START TRANSACTION
SQL语句
[COMMIT | ROLLBACK ]