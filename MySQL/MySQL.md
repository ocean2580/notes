# MySQL结构

![image-20220414143325992](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220414143325992.png)



![image-20220414143552127](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220414143552127.png)



![image-20220414152109924](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220414152109924.png)

# DATABASE

```mysql
CREATE DATABASE t; # 创建

DROP DATABASE [IF EXISTS] t; # 删除

SHOW DATABASES; # 打印所有数据库

SHOW CREATE DATABASE dxh_db01; # 打印已创建的某一个

```

## 备份数据库（DOS）

外部环境> mysqldump -u root -p -B dxh_db01 ... ... > e:\bak.sql

## 还原（DOS）

mysql> source e:\bak.sql;

# TABLE

![image-20220415200009464](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220415200009464.png)

## 修改

```mysql
USE dxh_db01; # 选数据库

ALTER TABLE t1 ADD hobby VARCHAR(255) ; # 增列

ALTER TABLE t1 MODIFY hobby VARCHAR(64)  NOT NULL DEFAULT '' ; # 修改字段属性

ALTER TABLE t1 DROP del_1;  # 删列

RENAME TABLE t1 TO t;  # 重命表名

ALTER TABLE t CHARACTER SET utf8; # 修改字符集/核对
ALTER TABLE t COLLATE utf8_bin;

ALTER TABLE t CHANGE `name` user_name VARCHAR(32) NOT NULL DEFAULT '' ; # 取代列

DROP TABLE t;
DESC t;  # 查表
```

## 多表 ( 写好 WHERE )

![image-20220416204404428](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416204404428.png)

```mysql
SELECT *
	FROM a, c
	WHERE a.id = c.id
	;
```

表与表自由组合时用 WHERE 找==表间字段的关系==以去重



- 自连接

  ![image-20220416212536102](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416212536102.png)

  ```mysql
  SELECT worker.ename AS 'cleck', boss.ename AS 'guide'
  	FROM emp AS worker, emp AS boss
  	WHERE worker.mgr = boss.empno;
  ```

  - 子查询

  ```mysql
  -- （返回结果）单行子查询
  SELECT * FROM emp
  	WHERE deptno =(
  		SELECT deptno 
  		FROM emp
  		WHERE ename = 'SMITH'
  	);
  	
  	
  -- （返回结果）多行子查询
  --  和部门十号工作同的雇员但是不包括十号自己的雇员
  SELECT ename, job, sal, deptno
  	FROM emp
  	WHERE  job IN ( -- 集合
  		SELECT DISTINCT job 
  			FROM emp
  			WHERE deptno = 10
  	) AND deptno != 10;
  	
  -- 多列子查询
  SELECT * FROM emp
  	WHERE (deptno, job) = (
  		SELECT deptno, job
  		FROM emp
  		WHERE ename = 'SMITH'
  	);
  ```

  ==对于多行子查询的返回结果可以用 ALL, ANY 限制==

  ![image-20220417100521156](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220417100521156.png)

  

  

  - 临时表

  ![image-20220417095548038](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220417095548038.png)



- 表的复制与去重

  ```mysql
  -- 表的结构复制
  CREATE TABLE my_tab02 LIKE emp;
  
  -- 表的(数据)复制
  INSERT INTO my_tab01
  	(id, `name`, sal, job, deptno)
  	SELECT empno, ename, sal, job, deptno FROM emp;
  
  -- 表的自我复制 ( data * 2^n )
  INSERT INTO my_tab01
  	SELECT *FROM my_tab01;
  	
  -- 表的去重 ( tmp )
  CREATE TABLE tmp LIKE my_tab01;
  INSERT INTO tmp 
  	SELECT DISTINCT * FROM my_tab01;
  DELETE FROM my_tab01;
  INSERT INTO my_tab01 
  	SELECT * FROM tmp;
  DROP TABLE tmp;
  ```

## 外连接

- 左/右外连

join

![image-20220417111446080](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220417111446080.png)

==ON 后字段补充，没有则为NULL==

![image-20220417112304924](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220417112304924.png)



# CRUD

对表内数据：

```mysql
# 建表
CREATE TABLE `goods`(
id INT,
goods_name VARCHAR(10),
price DOUBLE
); 

```



## INSERT

```mysql
# 插入数据
INSERT INTO `goods` (id,goods_name,price)
  VALUES(1,'HuaWei',2000),(2,'Apple',2500); 

INSERT INTO `goods` VALUES(3,'Nokia',2000); 
```

==全插入可省略列名==，可插入NULL(如果允许)

字符和日期放在 ' ' 中，字段 ` 

==选择的列有默认值则 (欲赋值字段集合 ) 可以不添加==





## UPDATE

```mysql
# 更新数据
UPDATE goods 
	SET price = 1000, goods_name = 'Apple' 
	WHERE id = 2 AND price = 2000
```

set 后的修改值用   ， 并列 ( 可NULL )

where 后的修改条件用 ==运算符== 连接



## DELETE

```mysql
# 删除中的记录
DELETE FROM goods 
	WHERE id = 2;

-- 表中所有记录
DELETE FROM goods;
```



## SELECT

```mysql
# 查询
SELECT [DISTINCT] * FROM goods;

SELECT `id`, `goods_name` FROM goods;

SELECT (a+b+c) AS total_score FROM goods;
```

==DISTINCT 去重==

全列 *

==还可以对列运算==  , 用AS取列的别名



- where子句 ( 过滤 ) 中常用运算符：

![image-20220416113508459](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416113508459.png)



... id LIKE '1%'    以1开头的不限制长度的id 

... id LIKE '1_'      以1开头的两位字符  



![image-20220416114734033](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416114734033.png)

BETWEEN ... AND ..  是==双闭==区间

IN (a,b,c) 是==集合==



- ORDER BY 子句排序查询结果

```mysql
SELECT (id+price) AS 'a' FROM goods
	WHERE id IN (1,2,3)
	ORDER BY a DESC;
```

==默认升序，放于句尾==

ASC, DESC



- GROUP BY 子句分组统计

```mysql
SELECT id, price, COUNT(id), COUNT(price) FROM goods 
	GROUP BY id, price;
```

SELECT 中的==各列名==一定要作为 GROUP BY 的标准

```mysql
SELECT id, COUNT(price) FROM goods 
	GROUP BY id 
	HAVING COUNT(id) > 1; 
```

==HAVING 过滤==

可使用别名



- 分页查询（展示符合条件的记录的==一部分==）

![image-20220416200303244](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416200303244.png)

```mysql
SELECT * FROM `one` 
	ORDER BY id
	LIMIT 0, 2;
```



![image-20220416202401921](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416202401921.png)

==分组，排序，分页显示==



- 合并查询

  ![image-20220417110104110](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220417110104110.png)

  ==UNION ALL 不去重==
  
  

# 函数

## 数字



- COUNT 

```mysql
SELECT COUNT(*) FROM goods
	WHERE id = 1;
```

满足条件的记录的行数  （*）

满足条件的某列有多少个，==不计NULL==（列名）



- SUM （数字型列值和）

```mysql
SELECT SUM(id) AS total_id,SUM(price) FROM goods;
```

```mysql
SELECT SUM(id)/COUNT(*) AS aver FROM goods;
```



- AVG ( 数字型列均和 )

```mysql
SELECT AVG(id) FROM goods;
```



- MAX/MIN ( 数字型列最值 )

```mysql
SELECT MAX(id) FROM goods;
```

![image-20220416153702235](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416153702235.png)



## 字符串

![image-20220416151101123](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416151101123.png)

```mysql
SELECT CONCAT(goods_name, ' is sold for ', price) AS Info FROM goods;

SELECT LENGTH(goods_name) FROM goods;

SELECT REPLACE(goods_name, 'HuaWei', 'HW') FROM goods; 

SELECT STRCMP('1', '1') FROM DUAL;
# DUAL 是 测试表

SELECT SUBSTRING(goods_name, 1, 2) FROM goods;

SELECT CONCAT(LCASE(SUBSTRING(goods_name,1,1)), 
			SUBSTRING(goods_name,2)) AS new_name FROM goods;
```



## 时间日期

![image-20220416153952634](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416153952634.png)

![image-20220416160606543](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416160606543.png)

```mysql
CREATE TABLE msg(
	id INT,
	content TEXT,
	send_time DATETIME);
	
INSERT INTO msg VALUES(1, '123',CURRENT_TIMESTAMP());

-- 日期格式化处理
SELECT id, content, DATE(send_time)
	FROM msg;
	
-- 十分钟之内发布的新闻
SELECT * FROM msg
	WHERE DATE_ADD(send_time, INTERVAL 10 MINUTE) >= NOW();
	
SELECT * FROM msg
	WHERE send_time >= DATE_SUB(NOW(), INTERVAL 10 MINUTE);
	
	
-- 日期 -> 天数差
SELECT DATEDIFF(NOW(),'2003-10-10') FROM DUAL;

SELECT DATEDIFF(DATE_ADD('2003-10-10', INTERVAL 80 YEAR), NOW())
	FROM DUAL;

```

![image-20220416161917020](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416161917020.png)



## 加密和系统

![image-20220416162521726](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416162521726.png)

![image-20220416163336404](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416163336404.png)



## 流程控制

![image-20220416193559386](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416193559386.png)

```mysql
SELECT IF( 1+1 = 2, TRUE, FALSE) FROM DUAL;

SELECT  IFNULL (NULL, 'default') FROM DUAL;

-- 返回一个值
SELECT CASE
	WHEN TRUE THEN 'jack'
	WHEN FALSE THEN 'tom'
	ELSE 'mary' END;
```

![image-20220416193720966](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220416193720966.png)



# 约束

## PRIMARY KEY

主键列值不可重不可NULL，每表最多一个（可复合）

字段末 / 表末（列名）

![image-20220418185747995](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220418185747995.png)

==复合主键仍然是一个整体==

唯一标识



## NOT NULL

非空



## UNIQUE

==列值不可重，如果没有NOT NULL 约束，字段可多NULL==

一表可多 UNIQUE

```mysql
id INT PRIMARY KEY,
id INT UNIQUE NOT NULL, -- equal
```

## FOREIGN KEY

定义在**从表**，主表有主键/UNIQUE，==外键列数据在主键列存在/NULL==

对应主外键==至少数据类型==一致

外键表型 INNODB

![image-20220418191409724](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220418191409724.png)

```mysql
CREATE TABLE m_c(
	id INT PRIMARY KEY,
	`name` VARCHAR(32) NOT NULL DEFAULT '');
	
CREATE TABLE m_s(
	id INT PRIMARY KEY,
	`name` VARCHAR(32) NOT NULL DEFAULT '',
	class_id INT,
	-- 
	FOREIGN KEY (class_id) REFERENCES m_c(id));
	
INSERT INTO m_c 
	VALUES (100, 'java'), (200, 'web');
	
INSERT INTO m_s
	VALUES (1, 'tom', 100),
	(2, 'jack', 200);

INSERT INTO m_s	-- fail
	(3, 'hhh', 300);
	
DELETE FROM m_c -- fail
	WHERE id =100;
	
SELECT * FROM m_c, m_s;

	
```

## CHECK

![image-20220418193615261](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220418193615261.png)



## AUTO_INCREMENT

```MySQL
CREATE TABLE k(
	id INT PRIMARY KEY AUTO_INCREMENT,
	n INT);

-- 修改默认值
ALTER TABLE k
	AUTO_INCREMENT = 2;
	
INSERT INTO k 
	VALUES (NULL, 1);
	
-- 指定值为准
INSERT INTO k -- 
	VALUES(9,2);

-- 从上一个值开始 +1 
INSERT INTO k
	(n) VALUES(3);
	
SELECT * FROM k;
	

```





# 索引

==索引只对创建的列有效==

没有索引进行==全表扫描==，索引会形成==索引数据结构==

代价：磁盘占用；dml语句效率影响 (维护) 。

![image-20220418211333753](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220418211333753.png)

==主键自带索引==



## 创建

```mysql
CREATE TABLE a(
	id INT,
	`name` VARCHAR(32));
	
SHOW INDEXES FROM a;

-- UNIQUE
CREATE UNIQUE INDEX id_index ON a(id);
ALTER TABLE a ADD UNIQUE INDEX id_index(id); 

-- 普通(字段可重)
CREATE INDEX id_index ON a(id);
ALTER TABLE a ADD INDEX id_index(id); 

-- PRIMARY KEY
CREATE TABLE b(
	id INT,
	`name` VARCHAR(32));
	
ALTER TABLE b ADD PRIMARY KEY (id);

SHOW INDEXES FROM b;
```

建立索引后，进行 SELECT 会大大提速



## 删除

```MySQL
-- drop
DROP INDEX n ON b;

ALTER TABLE b DROP PRIMARY KEY;
```



## 修改

```mysql
-- modify = delete + create 
```



## 查询

```mysql
-- select
SHOW INDEX FROM b;
SHOW INDEXES FROM b;

SHOW KEYS FROM b;

DESC b;


```



## 建议

频繁使用 （WHERE）建立   id



- 不适：

  唯一性差（频繁作为查询条件） sex

  经常更新 logincount

  



# 事务

类似于**原语**，==将多个dml语句当作一个整体== ，以保证数据的一致性

==InnoDB存储引擎默认且唯一支持事务==

![image-20220419090309541](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419090309541.png)



```mysql
CREATE TABLE c(
	id INT,
	`name` VARCHAR(32));
	
SELECT * FROM c;

--  默认保存点（初始状态）
START TRANSACTION;

SAVEPOINT a;

INSERT INTO c VALUES (1, 'l');

SAVEPOINT b;

INSERT INTO c VALUES (2, 'i');

ROLLBACK TO b;

ROLLBACK TO a;

-- 回退到初始状态
ROLLBACK;

--
COMMIT;
```



- 回退

  保存点是事务中的点，commit 后会自动删除该事务定义的所有保存点，==回退途经保存点会被删除==

  

  

- 提交

  ![image-20220419092223907](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419092223907.png)



## 隔离级别

==事务间（不同客户端的操作）==的隔离程度

默认 REPEATABLE READ

![image-20220419093809307](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419093809307.png)

![image-20220419094156689](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419094156689.png)

![image-20220419094330223](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419094330223.png)



隔离级别==自上而下==逐渐增强



加锁：如果某事务正在操作未提交，则加锁端事务会==等待其提交后==再进行。

![image-20220419104804112](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419104804112.png)



## 指令

```mysql
-- 查看当前会话/系统隔离级别
-- mysql>
SELECT @@tx_isolation;
SELECT @@global.tx_isolation;

-- 设置
-- mysql>
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SET GLOBAL TRANSACTION ISOLATION LEVEL [ ];
```

## ACID

- Atomicity

  事务是一个不可分割的工作单位

  

- Consistency

  一致状态

  

- Isolation

  多个并发事务之间要相互隔离

  

- Durability

  事务一旦提交，对数据库的数据改变是==永久的==



# 存储引擎

==表类型由存储引擎决定==

 ![image-20220419112047466](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419112047466.png)



![image-20220419112546080](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419112546080.png)



## 重点

- ![image-20220419112952583](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419112952583.png)

![image-20220419182302192](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419182302192.png)

MyISAM, InnoDB, Memory



# 视图

虚拟表，由==查询范围==定义，数据来自对应的真实表（基表）

视图/基表修改后**相互影响**，视图最终都映射到基表

可多个基表

![image-20220419185044746](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419185044746.png)

```mysql
-- 视图中可以再使用视图
CREATE VIEW myView
	AS 
	SELECT id, `name` FROM account;

CREATE VIEW my
	AS 
	SELECT id FROM myView;
```

## 应用

- 安全（保密字段）

- 性能（避免使用 JOIN ）

- 灵活（）

![image-20220419190425304](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419190425304.png)



# MySQL管理



![image-20220419191606906](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419191606906.png)





## 建/删 用户

```MySQL
-- 建
create user 'hhh' @'localhost' identified by 'dxh';

-- 删
drop user 'hhh' @'localhost';
```

![image-20220419202101793](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419202101793.png)

![image-20220419192756927](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419192756927.png)

不同数据库用户登录DBMS后，根据相应的权限，可操作的数据库和数据对象（表， 视图， 触发器）不同



## 修改用户密码

root 中：

```mysql
-- 修改他人
SET PASSWORD FOR 'hhh'@'localhost' = PASSWORD('1314');

-- 自己
SET PASSWORD = PASSWORD('dxh');
```

```mysql
-- 查看用户
select `host`, `user` from mysql.user;
```

![image-20220419201949118](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419201949118.png)

==mysql.user==

## 用户权限管理

### 授予

![image-20220419195408166](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419195408166.png)

```mysql
-- 几乎root权限
GRANT ALL ON *.* TO 'hhh'@'localhost' ;

-- 对应用户获得权限
GRANT SELECT, INSERT 
	ON dxh_db01.a
	TO 'hhh'@'localhost';
```



### 回收

![image-20220419195557221](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220419195557221.png)

```mysql
-- 回收对应用户的权限
REVOKE SELECT, INSERT  -- ALL
	ON dxh_db01.a
	FROM 'hhh'@'localhost';
```



### 权限生效

```MYSQL
FLUSH PRIVILEGES;
```

