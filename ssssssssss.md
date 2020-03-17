# SQL

### 运算符

#### 算术运算符

`+ - * / ()`

**所有包含 NULL 的计算，结果肯定是 NULL**，例如`5 + NULL `等

```mysql
SELECT product_name, sale_price,
 sale_price * 2 AS "sale_price_x2"
 FROM Product;
 
product_name | sale_price | sale_price_x2
---------------+-------------+----------------
T恤衫 | 1000 | 2000
打孔器 | 500 | 1000
运动T恤 | 4000 | 8000
菜刀 | 3000 | 6000
高压锅 | 6800 | 13600
叉子 | 500 | 1000
擦菜板 | 880 | 1760
圆珠笔 | 100 | 200
```

#### 比较运算符

- =
- <>  不相等
- \>=
- \>
- <=
- <
- IS NULL
- IS NOT NULL

**一定要让不等 号在左，等号在右**

```mysql
SELECT product_name, sale_price, purchase_price
 FROM Product
 WHERE sale_price - purchase_price >= 500;
 
 product_name | sale_price | purchase_price
---------------+-------------+---------------
T恤衫 | 1000 | 500
运动T恤 | 4000 | 2800
高压锅 | 6800 | 5000
```

**字符串类型的数据原则上按照字典顺序进行排序，不能与数字的大小顺序混淆。**

**不能对NULL使用比较运算符**

#### 逻辑运算符

- NOT
- AND
- OR

AND运算符的优先级高于OR运算符。想要优先执行OR运算符时可以使用括号。

使用逻辑运算符时也需要特别对待 NULL。

三值逻辑表：

- AND

  | P    | Q    | P AND Q |
  | ---- | ---- | :------ |
  | T    | NULL | NULL    |
  | F    | NULL | F       |
  | NULL | T    | NULL    |
  | NULL | F    | F       |

- OR

  | P    | Q    | P OR Q |
  | ---- | ---- | ------ |
  | T    | NULL | T      |
  | F    | NULL | NULL   |
  | NULL | T    | T      |
  | NULL | F    | NULL   |

  







## 聚合与排序

### 聚合查询

#### 聚合函数

- COUNT：计算表中的记录数（行数） 

  COUNT函数的结果根据参数的不同而不同。COUNT(*)会得到包含NULL的数据 行数，而COUNT(<列名>)会得到NULL之外的数据行数。

- SUM： 计算表中数值列中数据的合计值 

- AVG： 计算表中数值列中数据的平均值 

- MAX： 求出表中任意列中数据的最大值 

- MIN： 求出表中任意列中数据的最小值

  MAX/MIN函数几乎适用于所有数据类型的列。SUM/AVG函数只适用于数值类型的列。

  ```mysql
  SELECT MAX(regist_date), MIN(regist_date)
   FROM Product;
   
   max       |    min
  -----------+-----------
  2009-11-11 | 2008-04-28
  ```

使用聚合函数删除重复值

```mysql
SELECT COUNT(DISTINCT product_type)
 FROM Product;
 
 count
-------
 3
```

### 分组

#### GROUP BY

SQL子句书写顺序： 1.SELECT → 2. FROM → 3. WHERE → 4. GROUP BY

```mysql
SELECT <列名1>, <列名2>, <列名3>, ……
 FROM <表名>
 GROUP BY <列名1>, <列名2>, <列名3>, ……;
```

```mysql
SELECT product_type, COUNT(*)
 FROM Product
 GROUP BY product_type;
 
 
product_type | count
--------------+------
衣服 | 2
办公用品 | 2
厨房用具 | 4
```

聚合键中包含NULL时，在结果中会以“不确定”行（空行）的形式表现出来。

#### 同时使用WHERE

FROM → WHERE → GROUP BY → SELECT

常见错误

1. 在SELECT中书写多余的列

   SELECT 子句中只能存在以下三种元素:

   - 常数
   - 聚合函数
   -  GROUP BY子句中指定的列名（也就是聚合键）

   使用GROUP BY子句时，SELECT子句中不能出现聚合键之外的列名。

2. 在GROUP BY子句中写了列的别名

3. GROUP BY子句的结果能排序吗

   不能，随机

4. 在WHERE子句中使用聚合函数

   只有 SELECT 子句和 HAVING 子句（以及之后将要学到的 ORDER BY 子句）中能够使用 COUNT 等聚合函数。

### 为聚合指定条件-HAVING

书写顺序

SELECT → FROM → WHERE → GROUP BY → HAVING

```mysql
SELECT <列名1>, <列名2>, <列名3>, ……
 FROM <表名>
 GROUP BY <列名1>, <列名2>, <列名3>, ……
HAVING <分组结果对应的条件>
```

```mysql
SELECT product_type, COUNT(*)
 FROM Product
 GROUP BY product_type
HAVING COUNT(*) = 2;

product_type | count
--------------+------
衣服 | 2
办公用品 | 2
```

HAVING 子句构成

- 常数
- 聚合函数
- GROUP BY子句中指定的别名

聚合键所对应的条件（即既可以写在WHERE又可以写在HAVING）不应该书写在HAVING子句当中，而应该书写在WHERE子句 当中。



### 对查询结果排序

ORDER BY

默认升序ASC，DESC降序。

排序键中包含NULL时，会在开头或末尾进行汇总。

```mysql
SELECT <列名1>, <列名2>, <列名3>, ……
 FROM <表名>
 ORDER BY <排序基准列1>, <排序基准列2>, ……
```

``` mysql
SELECT product_id, product_name, sale_price, purchase_price
 FROM Product
ORDER BY sale_price;

product_id | product_name | sale_price | purchase_price
----------+---------------+-------------+---------------
 0008 | 圆珠笔 | 100 |
 0006 | 叉子 | 500 |
 0002 | 打孔器 | 500 | 320
 0007 | 擦菜板 | 880 | 790
 0001 | T恤衫 | 1000 | 500
 0004 | 菜刀 | 3000 | 2800
 0003 | 运动T恤 | 4000 | 2800
 0005 | 高压锅 | 6800 | 5000
```

SQL书写顺序

1. SELECT 子句 → 2. FROM 子句 → 3. WHERE 子句 → 4. GROUP BY 子句 → 5. HAVING 子句 → 6. ORDER BY 子句

SQL执行顺序

FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY



## 数据更新

### 插入

```mysql
INSERT INTO <表名> (列1, 列2, 列3, ……) VALUES (值1, 值2, 值3, ……);
```

插入默认值

```mysql
INSERT INTO ProductIns (product_id, product_name, product_type, 
sale_price, purchase_price, regist_date) VALUES ('0007', 
'擦菜板', '厨房用具', DEFAULT, 790, '2009-04-28');
```

从其他表中复制

```mysql
INSERT INTO ProductCopy (product_id, product_name, product_type, 
sale_price, purchase_price, regist_date)
SELECT product_id, product_name, product_type, sale_price,
purchase_price, regist_date
 FROM Product;
```

INSERT语句的SELECT语句中，可以使用WHERE子句或者GROUP BY子句等任 何 SQL语法（但使用ORDER BY子句并不会产生任何效果）。

### 删除

SQL删除语句：

1. DROP TABLE 语句可以将表完全删除

2. DELETE 语句会留下表（容器），而删除表中的全部数据

   `DELETE FROM Product;`

3. `TRUNCATE <表名>;`

   与 DELETE 不同的是，TRUNCATE 只能删除表中的全部数据，而不能通过 WHERE 子句指定条件来删除部分数据。也正是因为它不能具体地控制删除对象， 所以其处理速度比 DELETE 要快得多。

指定删除对象

```mysql
DELETE FROM <表名>
WHERE <条件>;
```

### 更新

```mysql
UPDATE <表名>
 SET <列名> = <表达式>;
```

指定条件

```mysql
UPDATE <表名>
 SET <列名> = <表达式>
WHERE <条件>;
```

```mysql
UPDATE Product
 SET sale_price = sale_price * 10
 WHERE product_type = '厨房用具';
```

多列更新

```mysql
UPDATE Product
 SET sale_price = sale_price * 10,
 purchase_price = purchase_price / 2
 WHERE product_type = '厨房用具';

UPDATE Product
 SET (sale_price, purchase_price) = (sale_price * 10, 
purchase_price / 2)
 WHERE product_type = '厨房用具';
```

### 事务

**事务就是需要在同一个处理单元中执行的一系列更新处理的集合。**

#### 事务处理流程

```mysql
事务开始语句;
 DML语句①;
 DML语句②;
 DML语句③;
 . . .
事务结束语句（COMMIT或者ROLLBACK）;

/*
sql--BEGIN TRANSACTION
*/
START TRANSACTION;
 -- 将运动T恤的销售单价降低1000日元
 UPDATE Product
 SET sale_price = sale_price - 1000
 WHERE product_name = '运动T恤';
 -- 将T恤衫的销售单价上浮1000日元
 UPDATE Product
 SET sale_price = sale_price + 1000
 WHERE product_name = 'T恤衫';
COMMIT;
```

事务回滚

ROLLBACK;

#### ACID特性

DBMS 的事务都遵循四种特性，将这四种特性的首字母结合起来统 称为 ACID 特性。

原子性是指在事务结束时，其中所包含的更新处理要么全部执行，要 么完全不执行，也就是要么占有一切要么一无所有。

一致性指的是事务中包含的处理要满足数据库提前设置的约束，如主 键约束或者 NOT NULL 约束等。

隔离性指的是保证不同事务之间互不干扰的特性。该特性保证了事务 之间不会互相嵌套。此外，在某个事务中进行的更改，在该事务结束之前， 对其他事务而言是不可见的。

持久性也可以称为耐久性，指的是在事务（不论是提交还是回滚）结 束后，DBMS 能够保证该时间点的数据状态会被保存的特性。即使由于系 统故障导致数据丢失，数据库也一定能通过某种手段进行恢复。



## 复杂查询

### 视图

视图究竟是什么呢？如果用一句话概述的话，就是“从 SQL 的角度 来看视图就是一张表”。

视图与表的区别---是否保存了实际的数据：

- 创建表时，会通过 INSERT 语句将数据保存到数据库 之中，而数据库中的数据实际上会被保存到计算机的存储设备（通常是硬 盘）中。

- 使用视图时并不会将数据保存到存储设备之中，而且也不会将数 据保存到其他任何地方。实际上视图保存的是 SELECT 语句。 我们从视图中读取数据时，视图会在内部执行该 SELECT 语句并创建出 一张临时表。

优点

1. 于视图无需保存数据，因此可以节省存储设备的容量。
2. 可以将频繁使用的 SELECT 语句保存成视图，这样 就不用每次都重新书写了。创建好视图之后，只需在 SELECT 语句中进 行调用，就可以方便地得到想要的结果了。特别是在进行汇总以及复杂的 查询条件导致 SELECT 语句非常庞大时，使用视图可以大大提高效率。

#### 创建

```mysql
CREATE VIEW 视图名称(<视图列名1>, <视图列名2>, ……)
AS
<SELECT语句>


CREATE VIEW ProductSum (product_type, cnt_product)
AS
SELECT product_type, COUNT(*)
FROM Product
GROUP BY product_type;
```

#### 使用

```mysql
SELECT product_type, cnt_product
 FROM ProductSum;
```

#### 查询

1. 首先执行定义视图的 SELECT 语句 
2. 根据得到的结果，再执行在 FROM 子句中使用视图的 SELECT 语句

```mysql
CREATE VIEW ProductSumJim (product_type, cnt_product)
AS
SELECT product_type, cnt_product
 FROM ProductSum
 WHERE product_type = '办公用品';
```

**应该避免在视图的基础上创建视图。**

#### 视图限制

1. 定义视图时不能使用ORDER BY子句	

   因为视图和表一样，数据行都是没有顺序的

2. 对视图进行更新

#### 删除

```mysql
DROP VIEW 视图名称(<视图列名1>, <视图列名2>, ……)

DROP VIEW ProductSum; 
```



### 子查询

```mysql
SELECT product_type, cnt_product
 FROM ( SELECT product_type, COUNT(*) AS cnt_product
 FROM Product
 GROUP BY product_type ) AS ProductSum;-
```

先内层再外层

#### 标量子查询

**标量子查询**就是返回单一值的子查询。

标量子查询的书写位置并不仅仅局限于 WHERE 子句中，通常任何可 以使用单一值的位置都可以使用。也就是说，能够使用常数或者列名的 地方，无论是 SELECT 子句、GROUP BY 子句、HAVING 子句，还是 ORDER BY 子句，几乎所有的地方都可以使用。 例如，在 SELECT 子句当中使用之前计算平

```mysql
SELECT AVG(sale_price)
 FROM Product;
 
  avg
----------------------
 2097.5000000000000000
 
 
 
 SELECT product_id,
 product_name,
 sale_price,
 (SELECT AVG(sale_price)
 FROM Product) AS avg_price
 FROM Product;
```

#### 关联子查询

**在细分的组内进行比较时，需要使用关联子查询。**

```mysql
SELECT product_type, product_name, sale_price
 FROM Product AS P1 
 WHERE sale_price > (SELECT AVG(sale_price)
 					FROM Product AS P2 
					 WHERE P1.product_type = P2.product_type
 					GROUP BY product_type);
```

## 函数

### 算术函数

- `+ - * /`
- ABS
- MOD
- ROUND

### 字符串函数

- 字符串1||字符串2    拼接
- LENGTH(字符串)
- LOWER(字符串)
- UPPER(字符串)
- REPLACE(对象字符串，替换前的字符串，替换后的字符串)
- SUBSTRING（对象字符串 FROM 截取的起始位置 FOR 截取的字符数）

### 日期函数

- CURRENT_DATE 返回 SQL 执行的日期，也就是该函数执 行时的日期
- CURRENT_TIME
- CURRENT_TIMESTAMP——当前日期和时间
- EXTRACT(日期元素 FROM 日期)

### 转换函数

- CAST（转换前的值 AS 想要转换的数据类型）

  ```mysql
  SQL
  SELECT CAST('0001' AS INTEGER) AS int_col;
  MySQL
  SELECT CAST('0001' AS SIGNED INTEGER) AS int_col;
  ```

- COALESCE(数据1，数据2，数据3……) ——将NULL转换为其他值

### 聚合函数





## 谓词

谓词的返回值全都是真值（TRUE/ FALSE/UNKNOWN）。这也是谓词和函数的最大区别。

###	LIKE 

当需要进行字符串的部分一致查询时需要使用该谓词。

```mysql
SELECT *
 FROM SampleLike
 WHERE strcol LIKE '%ddd%';
 
 
  strcol
--------
 abcddd
 dddabc
 abdddc
 
 
 SELECT *
 FROM SampleLike
 WHERE strcol LIKE 'abc_ _';
 
  strcol
--------
 abcdd
```

### BETWEEN 

```mysql
SELECT product_name, sale_price
 FROM Product
 WHERE sale_price BETWEEN 100 AND 1000;
 
 
 
 product_name | sale_price
------------+-------------
T恤衫 | 1000
打孔器 | 500
叉子 | 500
擦菜板 | 880
圆珠笔 | 100
```

### IS NULL     IS NOT NULL 

### IN

```mysql
SELECT product_name, purchase_price
 FROM Product
 WHERE purchase_price IN (320, 500, 5000);
 
 
 SELECT product_name, purchase_price
 FROM Product
 WHERE purchase_price NOT IN (320, 500, 5000);
 
 SELECT product_name, sale_price
 FROM Product
 WHERE product_id IN (SELECT product_id
 FROM ShopProduct
 WHERE shop_id = '000C');
```

### EXISTS

EXIST 的使用方法与之前的都不相同,语法理解起来比较困难,实际上即使不使用 EXIST，基本上也都可以使用 IN（或者 NOT IN）来代替。

通常指定关联子查询作为EXIST的参数。

### IF  CASE

## 集合运算

集合在数学领域表示“（各 种各样的）事物的总和”，在数据库领域表示记录的集合。具体来说，表、 视图和查询的执行结果都是记录的集合。

### 表的加法--UNION

```mysql
SELECT product_id, product_name
 FROM Product
UNION
SELECT product_id, product_name
 FROM Product2;
```

注意：

- 作为运算对象的记录的列数必须相同
- 作为运算对象的记录中列的类型必须一致
- 可以使用任何SELECT语句，但ORDER BY子句只 能在最后使用一次

包含重复行的集合运算——ALL选项

```mysql
SELECT product_id, product_name
 FROM Product
UNION ALL
SELECT product_id, product_name
 FROM Product2;
 
 
 product_id | product_name
----------+--------------
 0001 | T恤衫
 0002 | 打孔器
 0003 | 运动T恤
 0004 | 菜刀
 0005 | 高压锅
 0006 | 叉子
 0007 | 擦菜板
 0008 | 圆珠笔
 0001 | T恤衫
 0002 | 打孔器
 0003 | 运动T恤
 0009 | 手袋
 0010 | 水壶
```

选取表中公共部分——INTERSECT

```mysql
SELECT product_id, product_name
 FROM Product
INTERSECT
SELECT product_id, product_name
 FROM Product2
ORDER BY product_id;

product_id | product_name
----------+--------------
 0001 | T恤衫
 0002 | 打孔器
 0003 | 运动T恤
```

记录的减法——EXCEPT

```mysql
SELECT product_id, product_name
 FROM Product
EXCEPT
SELECT product_id, product_name
 FROM Product2
ORDER BY product_id;


product_id | product_name
----------+--------------
 0004 | 菜刀
 0005 | 高压锅
 0006 | 叉子
 0007 | 擦菜板
 0008 | 圆珠笔
```

### 联结

我们学习了 UNION 和 INTERSECT 等集合运算，这些集合 运算的特征就是以行方向为单位进行操作。通俗地说，就是进行这些集合 运算时，会导致记录行数的增减。使用 UNION 会增加记录行数，而使用 INTERSECT 或者 EXCEPT 会减少记录行数 。

但是这些运算不会导致列数的改变。作为集合运算对象的表的前提就 是列数要一致。因此，运算结果不会导致列的增减。

