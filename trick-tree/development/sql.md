---
description: 总结数据库相关知识
---

# SQL

由于应用需要保存用户数据，而且伴随着应用程序越来越复杂，数据量越来越大，管理这些数据面临着读写文件并且解析数据需要大量的重复代码，而且从成千上万的数据中查询出指定数据需要复杂逻辑，并且每个程序各自完成自己读写数据的代码不仅效率低，还容易出错，而且各个程序访问数据的接口都不相同，数据难以复用，因此，数据库作为一种专门管理数据的软件诞生了，应用不需要自己管理数据，通过数据库提供的接口来读写数据就好了。SQL是结构化查询语言的缩写，用来访问和操作数据库系统。SQL语句既可以查询数据库中的数据，也可以添加、更新和删除数据库中的数据，还可以对数据库进行管理和维护操作。不同的数据库，都支持SQL，这样，我们通过学习SQL这一种语言，就可以操作各种不同的数据库。

### 数据模型

数据库按照数据结构来组织、存储和管理数据，分为`层次模型` 、`网状模型` 、`关系模型` 。

层次模型是按上下级的层次关系来组织数据的一种方式。

![&#x5C42;&#x6B21;&#x6A21;&#x578B;](../../.gitbook/assets/tu-pian%20%2873%29.png)

网状模型是把每个数据节点和其他很多节点连接起来。

![&#x7F51;&#x72B6;&#x6A21;&#x578B;](../../.gitbook/assets/tu-pian%20%2822%29.png)

关系模型把数据看作一个二位表格，任何数据都可以通过行列号来确定。

![&#x5173;&#x7CFB;&#x6A21;&#x578B;](../../.gitbook/assets/tu-pian%20%2842%29.png)

### 数据类型

| 名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| INT | 整型 | 4字节整数类型，范围约+/-21亿 |
| BIGINT | 长整型 | 8字节整数类型，范围约+/-922亿亿 |
| REAL | 浮点型 | 4字节浮点数，范围约+/-1038 |
| DOUBLE | 浮点型 | 8字节浮点数，范围约+/-10308 |
| DECIMAL\(M,N\) | 高精度小数 | 由用户指定精度的小数，例如，DECIMAL\(20,10\)表示一共20位，其中小数10位，通常用于财务计算 |
| CHAR\(N\) | 定长字符串 | 存储指定长度的字符串，例如，CHAR\(100\)总是存储100个字符的字符串 |
| VARCHAR\(N\) | 变长字符串 | 存储可变长度的字符串，例如，VARCHAR\(100\)可以存储0~100个字符的字符串 |
| BOOLEAN | 布尔类型 | 存储True或者False |
| DATE | 日期类型 | 存储日期，例如，2018-06-22 |
| TIME | 时间类型 | 存储时间，例如，12:20:59 |
| DATETIME | 日期和时间类型 | 存储日期+时间，例如，2018-06-22 12:20:59 |

这里`REAL` 又可以写成`FLOAT(24)` ，还有一些不常用的数据类型，例如，`TINYINT` （范围在0-255），各数据库厂商还会支持特定的数据类型，例如`Json` ，通常来说，`BIGINT` 能满足整数存储的需求，`VARCHAR(N)` 能满足字符串存储的需求。

### 主流关系数据库

目前，主流的关系数据库主要分为以下几类：

1. 商用数据库，例如：[Oracle](https://www.oracle.com)，[SQL Server](https://www.microsoft.com/sql-server/)，[DB2](https://www.ibm.com/db2/)等；
2. 开源数据库，例如：[MySQL](https://www.mysql.com/)，[PostgreSQL](https://www.postgresql.org/)等；
3. 桌面数据库，以微软[Access](https://products.office.com/access)为代表，适合桌面应用程序使用；
4. 嵌入式数据库，以[Sqlite](https://sqlite.org/)为代表，适合手机应用和桌面程序

### SQL

SQL定义了如下操作数据库的能力：

1. DDL（ Data Definition Language）：允许用户定义数据（创建表、删除表、修改表结构），通常由数据库管理员执行。
2. DML（Data Manipulation Language）：用户添加、删除、更新数据
3. DQL（Data Query Language）：用户查询数据库

### 语法特点

不区分大小写，但是针对不同的数据库，对于表名、列名有的区分大小写，有的不区分大小写。同一个数据库，有的在Linux区分大小写，有的在Windows上不区分大小写。一般的，SQL的关键字总是大写，表名，列名均小写。

### 安装

请参考[安装MySQL](https://www.liaoxuefeng.com/wiki/1177760294764384/1179611020917408)

### 关系模型

我们将表的每一行称为记录（record），每一列称为字段（column）

### 主键

关系表中，一个很重要的约束就是，任意两条记录不能重复，能通过某个字段唯一区分出不同的记录，这个字段被称为`主键` 。

| id | class\_id | name | gender | score |
| :--- | :--- | :--- | :--- | :--- |
| 1 | 1 | 小明 | M | 90 |
| 2 | 1 | 小红 | F | 95 |

例如我们将`name` 设为`主键` ，那么通过名字`小明` 或者`小红` 就可以唯一确定一条记录，但是这样就没法存储同名的同学了，因为插入`相同` 主键的两条记录是不被允许的。

由于`主键` 是用来唯一定位定位记录的，修改了主键，会造成一些列影响，因此，选取主键的基本原则是：**不使用任何业务相关的字段作为主键**，我们一般选取`id` 作为`主键` ，可以作为`id` 的字段类型分为以下两种。

1. 自增整数类型：数据库会在插入数据时自动为每一条记录分配一个自增整数，这样我们就完全不用担心主键重复，也不用自己预先生成主键；
2. 全局唯一GUID类型：使用一种全局唯一的字符串作为主键，类似`8f55d96b-8acc-4636-8cb8-76bf8abc2f57`。GUID算法通过网卡MAC地址、时间戳和随机数保证任意计算机在任意时间生成的字符串都是不同的，大部分编程语言都内置了GUID算法，可以自己预算出主键。

   对于大部分应用来说，通常自增类型的主键就能满足需求。我们在`students`表中定义的主键也是`BIGINT NOT NULL AUTO_INCREMENT`类型。

### 联合主键

关系型数据库也允许通过多个字段唯一标识记录，通过两个及以上的字段都设置成主键，这种主键被称为`联合主键`，对于联合主键，允许有一列重复，只要不是所有的主键列都重复即可。

| id\_num | id\_type | other columns... |
| :--- | :--- | :--- |
| 1 | A | ... |
| 2 | A | ... |
| 2 | B | ... |

如果我们把上述表的`id_num`和`id_type`这两列作为联合主键，那么上面的3条记录都是允许的，因为没有两列主键组合起来是相同的。

没有必要的情况下，我们尽量不使用联合主键，因为它给关系表带来了复杂度的上升。

### 外键

在一个数据表中，通过一个字段可以把数据与另一张表关联起来，这种列成为外键，外键不是通过列名实现的，而是通过定义外键约束实现的。

```text
ALTER TABLE students
ADD CONSTRAINT fk_class_id
FOREIGN KEY (class_id)
REFERENCES classes (id);
```

其中，外键约束的名称`fk_class_id`可以任意，`FOREIGN KEY (class_id)`指定了`class_id`作为外键，`REFERENCES classes (id)`指定了这个外键将关联到`classes`表的`id`列（即`classes`表的主键）。

通过定义外键约束，关系数据库可以保证无法插入无效的数据。即如果`classes`表不存在`id=99`的记录，`students`表就无法插入`class_id=99`的记录。

如果我们想删除一个外键约束我们可以利用`ALTER TABLE` 实现

```text
ALTER TABLE students
DROP FOREIGN KEY fk_class_id;
```

这里注意，删除外键约束并没有删除外键这一列，删除列是通过`DROP COLUMN` 实现的。

### 索引

如果想在大量数据中快速查找记录，就要使用索引，这样数据库系统不用扫描整个表，直接可以定位到条件符合的记录。

例如，对于`students`表：

| id | class\_id | name | gender | score |
| :--- | :--- | :--- | :--- | :--- |
| 1 | 1 | 小明 | M | 90 |
| 2 | 1 | 小红 | F | 95 |
| 3 | 1 | 小军 | M | 88 |

如果要经常根据`score`列进行查询，就可以对`score`列创建索引：

```text
ALTER TABLE students
ADD INDEX idx_score (score);
```

使用`ADD INDEX idx_score (score)`就创建了一个名称为`idx_score`，使用列`score`的索引。索引名称是任意的，索引如果有多列，可以在括号里依次写上，例如：

```text
ALTER TABLE students
ADD INDEX idx_name_score (name, score);
```

索引的效率取决于索引列的值是否`散列`，即该列的值如果越互不相同，那么索引效率越高。反过来，如果记录的列存在大量相同的值，例如`gender`列，大约一半的记录值是`M`，另一半是`F`，因此，对该列创建索引就没有意义。

可以对一张表创建多个索引。索引的优点是提高了查询效率，缺点是在插入、更新和删除记录时，需要同时修改索引，因此，索引越多，插入、更新和删除记录的速度就越慢。

对于主键，关系数据库会自动对其创建主键索引。使用主键索引的效率是最高的，因为主键会保证绝对唯一。

### 唯一索引

在设计关系数据表的时候，看上去唯一的列，例如身份证号、邮箱地址等，因为他们具有业务含义，因此不宜作为主键。

但是，这些列根据业务要求，又具有唯一性约束：即不能出现两条记录存储了同一个身份证号。这个时候，就可以给该列添加一个唯一索引。例如，我们假设`students`表的`name`不能重复：

```text
ALTER TABLE students
ADD UNIQUE INDEX uni_name (name);
```

通过`UNIQUE`关键字我们就添加了一个唯一索引。

也可以只对某一列添加一个唯一约束而不创建唯一索引：

```text
ALTER TABLE students
ADD CONSTRAINT uni_name UNIQUE (name);
```

这种情况下，`name`列没有索引，但仍然具有唯一性保证。

无论是否创建索引，对于用户和应用程序来说，使用关系数据库不会有任何区别。这里的意思是说，当我们在数据库中查询时，如果有相应的索引可用，数据库系统就会自动使用索引来提高查询效率，如果没有索引，查询也能正常执行，只是速度会变慢。因此，索引可以在使用数据库的过程中逐步优化。

### 基本查询

`SELECT * FROM <表名>`

其中`SELECT` 是关键字，代表执行一个查询，`*` 表示所有列，`FROM` 表示从哪个表查询，查询结果是一个二维表，`SELECT` 不仅可以查询，还可以做运算。

![SELECT 100+200](../../.gitbook/assets/tu-pian%20%2865%29.png)

`SELECT` 还可以判断当前到数据库的连接是否有效，许多检测工具会执行一条`SELECT 1;` 来测试数据库的连接性。

### 条件查询

`SELECT * FROM <表名> WHERE <条件表达式>`

条件表达式可以用`<条件1> AND <条件2>` 表达多个条件的`与` 关系，同理，也可以使用`OR` 表示`或` 关系，也可以用`NOT` 表示`非` ，其中`NOT A=2` 等价于`A<>2` 。

如果需要组合三个或者更多条件，我们就需要用小括号`()`来表示 。

`SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender = 'M';`

### 优先级

如果不加括号，条件运算按照`NOT`、`AND`、`OR`的优先级进行，即`NOT`优先级最高，其次是`AND`，最后是`OR`。加上括号可以改变优先级。

### 常用的条件表达式

| 条件 | 表达式举例1 | 表达式举例2 | 说明 |
| :--- | :--- | :--- | :--- |
| 使用=判断相等 | score = 80 | name = 'abc' | 字符串需要用单引号括起来 |
| 使用&gt;判断大于 | score &gt; 80 | name &gt; 'abc' | 字符串比较根据ASCII码，中文字符比较根据数据库设置 |
| 使用&gt;=判断大于或相等 | score &gt;= 80 | name &gt;= 'abc' |  |
| 使用&lt;判断小于 | score &lt; 80 | name &lt;= 'abc' |  |
| 使用&lt;=判断小于或相等 | score &lt;= 80 | name &lt;= 'abc' |  |
| 使用&lt;&gt;判断不相等 | score &lt;&gt; 80 | name &lt;&gt; 'abc' |  |
| 使用LIKE判断相似 | name LIKE 'ab%' | name LIKE '%bc%' | %表示任意字符，例如'ab%'将匹配'ab'，'abc'，'abcd' |

### 投影查询

我们可以通过指定列名的方式来指定返回某些列的数据，而不是所有数据。

`SELECT 列1, 列2, 列3 FROM ...`

同时我们也可以给每一列起一个别名，这样得到的结果集的列名就可以和原表的列名不同。

`SELECT 列1 别名1, 列2 别名2, 列3 别名3 FROM ...`

### 排序

我们可以使用`order by` 子句对结果排序，会按照我们指定的参数从低到高进行排序，如果我们想要从高到低排序，我们可以加上`DESC` 表示倒序。

`SELECT id, name, gender, score FROM students ORDER BY score DESC;`

如果`score` 列中有相同数据，我们想要进一步排序，可以继续添加列名，例如， 使用`ORDER BY score DESC, gender`表示先按`score`列倒序，如果有相同分数的，再按`gender`列排序。

默认的排序规则是`ASC`：“升序”，即从小到大。`ASC`可以省略，即`ORDER BY score ASC`和`ORDER BY score`效果一样。

如果有`WHERE`子句，那么`ORDER BY`子句要放到`WHERE`子句后面。例如，查询一班的学生成绩，并按照倒序排序：

```text
SELECT id, name, gender, score
FROM students
WHERE class_id = 1
ORDER BY score DESC;
```

### 分页

如果我们查询的结果集数据很大，放到一个页面太多了，我们就可以分页显示，即从中截取相关记录，我们可以通过`LIMIT <M> OFFSET <N>` 实现。

例如我们要将下列语句查询的结果分页，每页`3` 条记录，取第`1`页数据。要注意的是索引从`0` 开始。

```text
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 0;
```

如果我们想取第`2` 页的数据，我们只需要跳过第一页的三条记录即可，也就是跳过`0` 、`1` 、`2` ，所以我们将`OFFSET` 设定为`3` 即可。

```text
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 3;
```

注意的是，`LIMIT 3` 表示的是最多3条记录，如果某页数据量不够3条会按照实际数量显示。

可见，分页查询的关键在于，首先要确定每页需要显示的结果数量`pageSize`（这里是3），然后根据当前页的索引`pageIndex`（从1开始），确定`LIMIT`和`OFFSET`应该设定的值：

* `LIMIT`总是设定为`pageSize`；
* `OFFSET`计算公式为`pageSize * (pageIndex - 1)`。

这样就能正确查询出第N页的记录集。

`LIMIT` 如果设定的值超过了查询的最大数量，并不会报错，而是会得到一个空的结果集，如果没有写`OFFSET` ，例如，`LIMIT 15` 等价于`LIMIT 15 OFFSET 0` ，在MySQL中，`LIMIT 15 OFFSET 30` 可以简写为`LIMIT 30,15` 。 使用`LIMIT <M> OFFSET <N>`分页时，随着`N`越来越大，查询效率也会越来越低。

### 聚合查询

`COUNT(*)`表示查询所有列的行数，要注意聚合的计算结果虽然是一个数字，但查询的结果仍然是一个二维表，只是这个二维表只有一行一列，并且列名是`COUNT(*)`

`SELECT COUNT(*) num FROM students;`

![](../../.gitbook/assets/tu-pian%20%28102%29.png)

通常，使用聚合查询时，我们应该给列名设置一个别名，便于处理结果：

`SELECT COUNT(*) num FROM students;`

![](../../.gitbook/assets/tu-pian%20%2840%29.png)

聚合查询同样可以使用`WHERE`条件，因此我们可以方便地统计出有多少男生、多少女生、多少80分以上的学生等：

`SELECT COUNT(*) boys FROM students WHERE gender = 'M';`

除了`COUNT()`函数外，SQL还提供了如下聚合函数：

| 函数 | 说明 |
| :--- | :--- |
| SUM | 计算某一列的合计值，该列必须为数值类型 |
| AVG | 计算某一列的平均值，该列必须为数值类型 |
| MAX | 计算某一列的最大值 |
| MIN | 计算某一列的最小值 |

注意，`MAX()`和`MIN()`函数并不限于数值类型。如果是字符类型，`MAX()`和`MIN()`会返回排序最后和排序最前的字符。

要特别注意：如果聚合查询的`WHERE`条件没有匹配到任何行，`COUNT()`会返回0，而`SUM()`、`AVG()`、`MAX()`和`MIN()`会返回`NULL`

### 分组

对于聚合查询，SQL还提供了`分组聚合`的功能。我们观察下面的聚合查询，我们可以指定字段让`GROUP BY` 分组。

`SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;`

我们可以看到， `GROUP BY`子句指定了按`class_id`分组，因此，执行该`SELECT`语句时，会把`class_id`相同的列先分组，再分别计算，因此，得到了3行结果。

![](../../.gitbook/assets/tu-pian%20%2896%29.png)

### 多表查询

`SELECT * FROM <表1> <表2>`

这样一次查询两个表的数据，查询的结果也是一个二维表，他是表1和表2的乘积，即表1的每一行与表2的每一行两两拼到一起返回，列数是表1和表2的`和`，行数是表1和表2 的`积` 。

这种多表查询又称笛卡尔查询，使用笛卡尔查询时要非常小心，由于结果集是目标表的行数乘积，对两个各自有100行记录的表进行笛卡尔查询将返回1万条记录，对两个各自有1万行记录的表进行笛卡尔查询将返回1亿条记录。

### 连接查询

连接查询是另一种类型的多表查询。连接查询对多个表进行JOIN运算，简单地说，就是先确定一个主表作为结果集，然后，把其他表的行有选择性地“连接”在主表结果集上。

注意INNER JOIN查询的写法是：

1. 先确定主表，仍然使用`FROM <表1>`的语法；
2. 再确定需要连接的表，使用`INNER JOIN <表2>`的语法；
3. 然后确定连接条件，使用`ON <条件...>`，这里的条件是`s.class_id = c.id`，表示`students`表的`class_id`列与`classes`表的`id`列相同的行需要连接；
4. 可选：加上`WHERE`子句、`ORDER BY`等子句。

   `RIGHT OUTER JOIN`，返回右表都存在的行。如果某一行仅在右表存在，那么结果集就会以`NULL`填充剩下的字段。

   `LEFT OUTER JOIN`，返回左表都存在的行。如果我们给students表增加一行，并添加class\_id=5，由于classes表并不存在id=5的行，所以，LEFT OUTER JOIN的结果会增加一行，对应的`class_name`是`NULL`

`FULL OUTER JOIN`，它会把两张表的所有记录全部选择出来，并且，自动把对方不存在的列填充为`NULL`

对于这么多种JOIN查询，到底什么使用应该用哪种呢？其实我们用图来表示结果集就一目了然了，假设查询语句是

```text
SELECT ... FROM tableA ??? JOIN tableB ON tableA.column1 = tableB.column2;
```

我们把tableA看作左表，把tableB看成右表，那么INNER JOIN是选出两张表都存在的记录

![](../../.gitbook/assets/tu-pian%20%2829%29.png)

LEFT OUTER JOIN是选出左表存在的记录

![](../../.gitbook/assets/tu-pian%20%2824%29.png)

RIGHT OUTER JOIN是选出右表存在的记录：

![](../../.gitbook/assets/tu-pian%20%28107%29.png)

FULL OUTER JOIN则是选出左右表都存在的记录

![](../../.gitbook/assets/tu-pian%20%2863%29.png)

### 插入数据

```text
INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);
```

我们并没有列出`id`字段，也没有列出`id`字段对应的值，这是因为`id`字段是一个自增主键，它的值可以由数据库自己推算出来。此外，如果一个字段有默认值，那么在`INSERT`语句中也可以不出现。 字段顺序不必和数据库表的字段顺序一致，但值的顺序必须和字段顺序一致。也就是说，可以写`INSERT INTO students (score, gender, name, class_id) ...`，但是对应的`VALUES`就得变成`(80, 'M', '大牛', 2)`。

### 更改数据

```text
PDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;
```

`UPDATE`语句的`WHERE`条件和`SELECT`语句的`WHERE`条件其实是一样的，因此完全可以一次更新多条记录。

`UPDATE students SET name='小牛', score=77 WHERE id>=5 AND id<=7;`

`SET score=score+10`就是给当前行的`score`字段的值加上了10。

如果`WHERE`条件没有匹配到任何记录，`UPDATE`语句不会报错，也不会有任何记录被更新。

`UPDATE`语句可以没有`WHERE`条件，例如：

```text
UPDATE students SET score=60;
```

这时，整个表的所有记录都会被更新。所以，在执行`UPDATE`语句时要非常小心，最好先用`SELECT`语句来测试`WHERE`条件是否筛选出了期望的记录集，然后再用`UPDATE`更新。

### 删除数据

```text
DELETE FROM <表名> WHERE ...;
```

`DELETE`语句的`WHERE`条件也是用来筛选需要删除的行，因此和`UPDATE`类似，`DELETE`语句也可以一次删除多条记录， 如果`WHERE`条件没有匹配到任何记录，`DELETE`语句不会报错，也不会有任何记录被删除，和`UPDATE`类似，不带`WHERE`条件的`DELETE`语句会删除整个表的数据。

### MySQL

列出所有数据库：

```text
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| shici              |
| sys                |
| test               |
| school             |
+--------------------+
```

其中，`information_schema`、`mysql`、`performance_schema`和`sys`是系统库，不要去改动它们。其他的是用户创建的数据库。

创建新的数据库：

```text
mysql> CREATE DATABASE test;
Query OK, 1 row affected (0.01 sec)
```

删除数据库：

```text
mysql> DROP DATABASE test;
Query OK, 0 rows affected (0.01 sec)
```

注意：删除一个数据库将导致该数据库的所有表全部被删除。

对一个数据库进行操作时，要首先将其切换为当前数据库：

```text
mysql> USE test;
Database changed
```

列出当前数据库的所有表，使用命令：

```text
mysql> SHOW TABLES;
+---------------------+
| Tables_in_test      |
+---------------------+
| classes             |
| statistics          |
| students            |
| students_of_class1  |
+---------------------+
```

要查看一个表的结构，使用命令：

```text
mysql> DESC students;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | bigint(20)   | NO   | PRI | NULL    | auto_increment |
| class_id | bigint(20)   | NO   |     | NULL    |                |
| name     | varchar(100) | NO   |     | NULL    |                |
| gender   | varchar(1)   | NO   |     | NULL    |                |
| score    | int(11)      | NO   |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)
```

还可以使用以下命令查看创建表的SQL语句：

```text
mysql> SHOW CREATE TABLE students;
+----------+-------------------------------------------------------+
| students | CREATE TABLE `students` (                             |
|          |   `id` bigint(20) NOT NULL AUTO_INCREMENT,            |
|          |   `class_id` bigint(20) NOT NULL,                     |
|          |   `name` varchar(100) NOT NULL,                       |
|          |   `gender` varchar(1) NOT NULL,                       |
|          |   `score` int(11) NOT NULL,                           |
|          |   PRIMARY KEY (`id`)                                  |
|          | ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 |
+----------+-------------------------------------------------------+
1 row in set (0.00 sec)
```

创建表使用`CREATE TABLE`语句，而删除表使用`DROP TABLE`语句：

```text
mysql> DROP TABLE students;
Query OK, 0 rows affected (0.01 sec)
```

修改表就比较复杂。如果要给`students`表新增一列`birth`，使用：

```text
ALTER TABLE students ADD COLUMN birth VARCHAR(10) NOT NULL;
```

要修改`birth`列，例如把列名改为`birthday`，类型改为`VARCHAR(20)`：

```text
ALTER TABLE students CHANGE COLUMN birth birthday VARCHAR(20) NOT NULL;
```

要删除列，使用：

```text
ALTER TABLE students DROP COLUMN birthday;
```

#### 退出MySQL

使用`EXIT`命令退出MySQL：

```text
mysql> EXIT
Bye
```

注意`EXIT`仅仅断开了客户端和服务器的连接，MySQL服务器仍然继续运行。

### 实用SQL语句

#### 插入或替换

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就先删除原记录，再插入新记录。此时，可以使用`REPLACE`语句，这样就不必先查询，再决定是否先删除再插入：

```text
REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```

若`id=1`的记录不存在，`REPLACE`语句将插入新记录，否则，当前`id=1`的记录将被删除，然后再插入新记录。

#### 插入或更新

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就更新该记录，此时，可以使用`INSERT INTO ... ON DUPLICATE KEY UPDATE ...`语句：

```text
INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99) ON DUPLICATE KEY UPDATE name='小明', gender='F', score=99;
```

若`id=1`的记录不存在，`INSERT`语句将插入新记录，否则，当前`id=1`的记录将被更新，更新的字段由`UPDATE`指定。

#### 插入或忽略

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就啥事也不干直接忽略，此时，可以使用`INSERT IGNORE INTO ...`语句：

```text
INSERT IGNORE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```

若`id=1`的记录不存在，`INSERT`语句将插入新记录，否则，不执行任何操作。

#### 快照

如果想要对一个表进行快照，即复制一份当前表的数据到一个新表，可以结合`CREATE TABLE`和`SELECT`：

```text
-- 对class_id=1的记录进行快照，并存储为新表students_of_class1:
CREATE TABLE students_of_class1 SELECT * FROM students WHERE class_id=1;
```

新创建的表结构和`SELECT`使用的表结构完全一致。

#### 写入查询结果集

如果查询结果集需要写入到表中，可以结合`INSERT`和`SELECT`，将`SELECT`语句的结果集直接插入到指定表中。

例如，创建一个统计成绩的表`statistics`，记录各班的平均成绩：

```text
CREATE TABLE statistics (
    id BIGINT NOT NULL AUTO_INCREMENT,
    class_id BIGINT NOT NULL,
    average DOUBLE NOT NULL,
    PRIMARY KEY (id)
);
```

然后，我们就可以用一条语句写入各班的平均成绩：

```text
INSERT INTO statistics (class_id, average) SELECT class_id, AVG(score) FROM students GROUP BY class_id;
```

确保`INSERT`语句的列和`SELECT`语句的列能一一对应，就可以在`statistics`表中直接保存查询的结果：

```text
> select * from statistics;
+----+----------+--------------+
| id | class_id | average      |
+----+----------+--------------+
|  1 |        1 |         86.5 |
|  2 |        2 | 73.666666666 |
|  3 |        3 | 88.333333333 |
+----+----------+--------------+
3 rows in set (0.00 sec)
```

### 事务

在执行SQL语句的时候，某些业务要求，一系列操作必须全部执行，而不能仅执行一部分。例如，一个转账操作：

```text
-- 从id=1的账户给id=2的账户转账100元
-- 第一步：将id=1的A账户余额减去100
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
-- 第二步：将id=2的B账户余额加上100
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
```

这两条SQL语句必须全部执行，或者，由于某些原因，如果第一条语句成功，第二条语句失败，就必须全部撤销。

这种把多条语句作为一个整体进行操作的功能，被称为数据库_事务_。数据库事务可以确保该事务范围内的所有操作都可以全部成功或者全部失败。如果事务失败，那么效果就和没有执行这些SQL一样，不会对数据库数据有任何改动。

可见，数据库事务具有ACID这4个特性：

* A：Atomic，原子性，将所有SQL作为原子工作单元执行，要么全部执行，要么全部不执行；
* C：Consistent，一致性，事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了100；
* I：Isolation，隔离性，如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离；
* D：Duration，持久性，即事务完成后，对数据库数据的修改被持久化存储。

对于单条SQL语句，数据库系统自动将其作为一个事务执行，这种事务被称为_隐式事务_。

要手动把多条SQL语句作为一个事务执行，使用`BEGIN`开启一个事务，使用`COMMIT`提交一个事务，这种事务被称为_显式事务_，例如，把上述的转账操作作为一个显式事务：

```text
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

很显然多条SQL语句要想作为一个事务执行，就必须使用显式事务。

`COMMIT`是指提交事务，即试图把事务内的所有SQL所做的修改永久保存。如果`COMMIT`语句执行失败了，整个事务也会失败。

有些时候，我们希望主动让事务失败，这时，可以用`ROLLBACK`回滚事务，整个事务会失败：

```text
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
ROLLBACK;
```

数据库事务是由数据库系统保证的，我们只需要根据业务逻辑使用它就可以。

#### 隔离级别

对于两个并发执行的事务，如果涉及到操作同一条记录的时候，可能会发生问题。因为并发操作会带来数据的不一致性，包括脏读、不可重复读、幻读等。数据库系统提供了隔离级别来让我们有针对性地选择事务的隔离级别，避免数据不一致的问题。

SQL标准定义了4种隔离级别，分别对应可能出现的数据不一致的情况：

| Isolation Level | 脏读（Dirty Read） | 不可重复读（Non Repeatable Read） | 幻读（Phantom Read） |
| :--- | :--- | :--- | :--- |
| Read Uncommitted | Yes | Yes | Yes |
| Read Committed | - | Yes | Yes |
| Repeatable Read | - | - | Yes |
| Serializable | - | - | - |

**Read Uncommitted**

Read Uncommitted是隔离级别最低的一种事务级别。在这种隔离级别下，一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读到的数据就是脏数据，这就是脏读（Dirty Read）。

我们来看一个例子。

首先，我们准备好`students`表的数据，该表仅一行记录：

```text
mysql> select * from students;
+----+-------+
| id | name  |
+----+-------+
|  1 | Alice |
+----+-------+
1 row in set (0.00 sec)
```

然后，分别开启两个MySQL客户端连接，按顺序依次执行事务A和事务B：

| 时刻 | 事务A | 事务B |
| :--- | :--- | :--- |
| 1 | SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED; | SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED; |
| 2 | BEGIN; | BEGIN; |
| 3 | UPDATE students SET name = 'Bob' WHERE id = 1; |  |
| 4 |  | SELECT \* FROM students WHERE id = 1; |
| 5 | ROLLBACK; |  |
| 6 |  | SELECT \* FROM students WHERE id = 1; |
| 7 |  | COMMIT; |

当事务A执行完第3步时，它更新了`id=1`的记录，但并未提交，而事务B在第4步读取到的数据就是未提交的数据。

随后，事务A在第5步进行了回滚，事务B再次读取`id=1`的记录，发现和上一次读取到的数据不一致，这就是脏读。

可见，在Read Uncommitted隔离级别下，一个事务可能读取到另一个事务更新但未提交的数据，这个数据有可能是脏数据。

**Read Committed**

\*\*\*\*

在Read Committed隔离级别下，一个事务可能会遇到不可重复读（Non Repeatable Read）的问题。

不可重复读是指，在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据就可能不一致。

我们仍然先准备好`students`表的数据：

```text
mysql> select * from students;
+----+-------+
| id | name  |
+----+-------+
|  1 | Alice |
+----+-------+
1 row in set (0.00 sec)
```

然后，分别开启两个MySQL客户端连接，按顺序依次执行事务A和事务B：

| 时刻 | 事务A | 事务B |
| :--- | :--- | :--- |
| 1 | SET TRANSACTION ISOLATION LEVEL READ COMMITTED; | SET TRANSACTION ISOLATION LEVEL READ COMMITTED; |
| 2 | BEGIN; | BEGIN; |
| 3 |  | SELECT \* FROM students WHERE id = 1; |
| 4 | UPDATE students SET name = 'Bob' WHERE id = 1; |  |
| 5 | COMMIT; |  |
| 6 |  | SELECT \* FROM students WHERE id = 1; |
| 7 |  | COMMIT; |

当事务B第一次执行第3步的查询时，得到的结果是`Alice`，随后，由于事务A在第4步更新了这条记录并提交，所以，事务B在第6步再次执行同样的查询时，得到的结果就变成了`Bob`，因此，在Read Committed隔离级别下，事务不可重复读同一条记录，因为很可能读到的结果不一致。

**Repeatable Read**

在Repeatable Read隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。

幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。

我们仍然先准备好`students`表的数据：

```text
mysql> select * from students;
+----+-------+
| id | name  |
+----+-------+
|  1 | Alice |
+----+-------+
1 row in set (0.00 sec)
```

然后，分别开启两个MySQL客户端连接，按顺序依次执行事务A和事务B：

| 时刻 | 事务A | 事务B |
| :--- | :--- | :--- |
| 1 | SET TRANSACTION ISOLATION LEVEL REPEATABLE READ; | SET TRANSACTION ISOLATION LEVEL REPEATABLE READ; |
| 2 | BEGIN; | BEGIN; |
| 3 |  | SELECT \* FROM students WHERE id = 99; |
| 4 | INSERT INTO students \(id, name\) VALUES \(99, 'Bob'\); |  |
| 5 | COMMIT; |  |
| 6 |  | SELECT \* FROM students WHERE id = 99; |
| 7 |  | UPDATE students SET name = 'Alice' WHERE id = 99; |
| 8 |  | SELECT \* FROM students WHERE id = 99; |
| 9 |  | COMMIT; |

事务B在第3步第一次读取`id=99`的记录时，读到的记录为空，说明不存在`id=99`的记录。随后，事务A在第4步插入了一条`id=99`的记录并提交。事务B在第6步再次读取`id=99`的记录时，读到的记录仍然为空，但是，事务B在第7步试图更新这条不存在的记录时，竟然成功了，并且，事务B在第8步再次读取`id=99`的记录时，记录出现了。

可见，幻读就是没有读到的记录，以为不存在，但其实是可以更新成功的，并且，更新成功后，再次读取，就出现了。

**Serializable**

Serializable是最严格的隔离级别。在Serializable隔离级别下，所有事务按照次序依次执行，因此，脏读、不可重复读、幻读都不会出现。

虽然Serializable隔离级别下的事务具有最高的安全性，但是，由于事务是串行执行，所以效率会大大下降，应用程序的性能会急剧降低。如果没有特别重要的情景，一般都不会使用Serializable隔离级别。

#### 默认隔离级别

如果没有指定隔离级别，数据库就会使用默认的隔离级别。在MySQL中，如果使用InnoDB，默认的隔离级别是Repeatable Read。

## 参考资料

1. [SQL教程](https://github.com/Echocipher/Magic-Book-Of-High-Salary/tree/2296c80f23e9602d03b15ada82f82ef4204f1969/trick-tree/development/**[**https:/www.liaoxuefeng.com/wiki/1177760294764384**]%28https:/www.liaoxuefeng.com/wiki/1177760294764384%29**/README.md)

\*\*\*\*

