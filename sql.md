数据库管理
---
```bash
# 进入数据库
mysql -u root -p
# 查看数据库
SHOW DATABASES;
# 创建数据库
CREATE DATABASE test;
# 如果数据库不存在则创建数据库
CREATE DATABASE IF NOT EXISTS test;
# 删除数据库
DROP DATABASE test;
# 切换到某个数据库
USE test;
# 查看表
SHOW TABLES;
# 创建表
CREATE TABLE students;
# 快照(复制一份数据到一个新表)
CREATE TABLE students_of_class1 SELECT * FROM students WHERE class_id = 1;
# 创建表的同时创建字段
CREATE TABLE students (
    id BIGINT NOT NULL AUTO_INCREMENT,
    class_id BIGINT NOT NULL,
    name VARCHAR(100) NOT NULL,
    gender VARCHAR(1) NOT NULL,
    score INT NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
# 删除表
DROP TABLE students;
# 查看数据库的列
DESC students;
# 查看创建表的 SQL 语句
SHOW CREATE TABLE students;
# 创建列
ALTER TABLE students ADD COLUMN birth VARCHAR(10) NOT NULL;
# 修改列
ALTER TABLE students CHANGE COLUMN birth birthday VARCHAR(20) NOT NULL;
# 删除列
ALTER TABLE students DROP COLUMN birthday;
# 退出数据库
EXIT;
```

创建外键约束
ALTER TABLE students
ADD CONSTRAINT fk_class_id
FOREIGN KEY (class_id)
REFERENCES classes (id);

外键约束的名称fk_class_id可以任意，FOREIGN KEY (class_id)指定了class_id作为外键，REFERENCES classes (id)指定了这个外键将关联到classes表的id列（即classes表的主键）

删除外键约束
ALTER TABLE students
DROP FOREIGN fk_class_id;

一对一
| id | 学生姓名 | 其他字段 |
| --- | --- | --- |
| 1 | 小红 | ... |

| id | 学生id | mobile |
| --- | --- | --- |
| 1 | 1 | 135xxx123 |

一对多
| id | 班级id | 学生姓名 | 其他字段 |
| --- | --- | --- | --- |
| 1 | 1 | 小红 | ... |
| 2 | 1 | 小绿 | ... |
| 3 | 2 | 小黑 | ... |

多对多(通过中间表关联两个一对多)
| id | 老师姓名 | 其他字段 |
| --- | --- | --- |
| 1 | 小红 | ... |

| id | 班级 | 其他字段 |
| --- | --- | --- |
| 1 | 一班 | ... |

| id | 班级id | 老师id |
| --- | --- | --- |
| 1 | 1 | 1 |

Navicat Premium 连接 mysql 时中文显示乱码：创建连接时，选择高级选项卡，将编码修改为 650001 utf8

常用数据类型
| 名称                | 类型              | 说明                                                         |
| ------------------- | ----------------- | ------------------------------------------------------------ |
| INT                 | 整型              | 4字节整数类型，范围约+/-21亿                                 |
| BIGINT              | 长整型            | 8字节整数类型，范围约+/-922亿亿                              |
| REAL                | 浮点型            | 4字节浮点数，范围约+/-1038                                   |
| DOUBLE              | 浮点型            | 8字节浮点数，范围约+/-10308                                  |
| DECIMAL(M,N)        | 高精度小数        | 由用户指定精度的小数，例如，DECIMAL(20,10)表示一共20位，其中小数10位，通常用于财务计算 |
| CHAR(N)             | 定长字符串        | 存储指定长度的字符串，例如，CHAR(100)总是存储100个字符的字符串 |
| VARCHAR(N)          | 变长字符串        | 存储可变长度的字符串，例如，VARCHAR(100)可以存储0~100个字符的字符串 |
| BOOLEAN	布尔类型 | 存储True或者False |                                                              |
| DATE                | 日期类型          | 存储日期，例如，2018-06-22                                   |
| TIME                | 时间类型          | 存储时间，例如，12:20:59                                     |
| DATETIME            | 日期和时间类型    | 存储日期+时间，例如，2018-06-22 12:20:59                     |

```sql
-- 索引、唯一索引、联合索引
-- 创建索引
-- score 为列名
ALTER TABLE students
ADD INDEX idx_score (score);

-- 创建唯一索引
ALTER TABLE students
ADD UNIQUE INDEX uni_name (name);
```

查询
---
```sql
-- 查询
SELECT * FROM <表名>;
-- 测试数据库连接
SELECT 1;
-- 条件查询
SELECT * FROM <表名> WHERE <条件表达式>
-- 强制使用索引进行查询(前提是要创建索引)
SELECT * FROM students FORCE INDEX (idx_class_id) WHERE class_id=1 ORDER BY id DESC;
-- 条件表达式
<列名> >= <值>
<列名> > <值>
<列名> <= <值>
<列名> < <值>
<列名> = <值>
-- 与
AND
-- 或
OR
-- 非
NOT
-- 例子
SELECT * FROM students WHERE id = 2;
SELECT * FROM students WHERE score >= 80 AND gender = 'M';
SELECT * FROM students WHERE score >= 80 OR gender = 'M';
SELECT * FROM students WHERE NOT class_id = 2;
SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender = 'M';

```
| 条件 | 表达式举例1 | 表达式举例2 | 说明 |
| --- | --- | --- | --- |
| 使用=判断相等 | score = 80 | name = 'abc' | 字符串需要用单引号括起来 |
| 使用>判断大于 | score > 80 | name > 'abc' | 字符串比较根据ASCII码，中文字符比较根据数据库设置 |
| 使用>=判断大于或相等 | score >= 80 | name >= 'abc' | --- |
| 使用<判断小于 | score < 80 | name <= 'abc' | --- |
| 使用<=判断小于或相等 | score <= 80 | name <= 'abc' | --- |
| 使用<>判断不相等 | score <> 80 | name <> 'abc' | --- |
| 使用LIKE判断相似 | name LIKE 'ab%' | name LIKE '%bc%'	| %表示任意字符，例如'ab%'将匹配'ab'，'abc'，'abcd' |

投射查询
```sql
SELECT <列名1> <别名>, <列名2> FROM <表名>
SELECT id, score points, name FROM students;
SELECT id, class_id '班级id', score '分数' FROM students;
```

排序查询(默认升序)
```sql
SELECT id, name, gender, score FROM students ORDER BY score;
-- 升序(低到高d)
SELECT id, name, gender, score FROM students ORDER BY score ASC;
-- 降序(高到低)
SELECT id, name, gender, score FROM students ORDER BY score DESC;
-- 查询一班的学生成绩，并按照倒序排序(ORDER BY 要放在 WHERE 后面)
SELECT id, name, gender, score
FROM students
WHERE class_id = 1
ORDER BY score DESC;
```

分页查询
```sql
-- 第一页(每页 3 条，从索引 0 开始)
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 0;
-- 第二页(每页 3 条，从索引 3 开始)
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 3;
```
LIMIT总是设定为pageSize
OFFSET计算公式为pageSize * (pageIndex - 1)
OFFSET是可选的，如果只写LIMIT 15，那么相当于LIMIT 15 OFFSET 0。
在MySQL中，LIMIT 15 OFFSET 30还可以简写成LIMIT 30, 15。
使用LIMIT <M> OFFSET <N>分页时，随着N越来越大，查询效率也会越来越低。

聚合查询
```sql
SELECT COUNT(*) FROM students;
-- 添加别名
SELECT COUNT(*) num FROM students;
SELECT COUNT(*) boys FROM students WHERE gender = 'M';
-- 每页 3 条记录，获取总页数
SELECT CEILING(COUNT(*) / 3) FROM students;
-- 分组
SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;
-- 计算每个班级的平均分
SELECT class_id, ROUND(AVG(score), 2) average_score FROM students GROUP BY class_id;
```
| 函数 | 说明 | 零值 |
| --- | --- | --- |
| COUNT | 计算列的行数 | 0 |
| SUM | 计算某一列的合计值，该列必须为数值类型 | NULL |
| AVG | 计算某一列的平均值，该列必须为数值类型 | NULL |
| MAX | 计算某一列的最大值 | NULL |
| MIN | 计算某一列的最小值 | NULL |
| FLOOR | 向下取整 | -  |
| CEILING | 向上取整 | - |
| ROUND | 数据四舍五入 | -  |

多表查询(多个表的乘积 列数是多个表之和 行数是多个表之积)
```sql
SELECT * FROM students, classes;
-- 设置列别名
SELECT
    students.id sid,
    students.name,
    students.gender,
    students.score,
    classes.id cid,
    classes.name cname
FROM students, classes;
-- 设置表别名
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c;
-- 查询 1 班男生
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c
WHERE s.gender = 'M' AND c.id = 1;
```

连接查询
内连接
先查询出一个表的结果，再根据结果选择性地“连接”在主表结果集上
注意INNER JOIN查询的写法是：

先确定主表，仍然使用FROM <表1>的语法；
再确定需要连接的表，使用INNER JOIN <表2>的语法；
然后确定连接条件，使用ON <条件...>，这里的条件是s.class_id = c.id，表示students表的class_id列与classes表的id列相同的行需要连接；
可选：加上WHERE子句、ORDER BY等子句。
```sql
-- 选出全部学生，同时返回班级名称
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
INNER JOIN classes c
ON s.class_id = c.id;
-- 选出男生，同时返回班级名称
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
INNER JOIN classes c
ON s.class_id = c.id
WHERE s.gender = 'M'
```

外连接
RIGHT OUTER JOIN 右外连接
LEFT OUTER JOIN 左外连接
FULL OUTER JOIN 全外连接
```sql
-- 右外连接
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
RIGHT OUTER JOIN classes c
ON s.class_id = c.id;
-- 左外连接
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
LEFT OUTER JOIN classes c
ON s.class_id = c.id;
```

内连接，外连接区别

INNER JOIN(交集)只返回同时存在于两张表的行数据，由于students表的class_id包含1，2，3，classes表的id包含1，2，3，4，所以，INNER JOIN根据条件s.class_id = c.id返回的结果集仅包含1，2，3。

RIGHT OUTER JOIN(交集加上右边内容)返回右表都存在的行。如果某一行仅在右表存在，那么结果集就会以NULL填充剩下的字段。

LEFT OUTER JOIN(交集加上左表内容)则返回左表都存在的行。如果我们给students表增加一行，并添加class_id=5，由于classes表并不存在id=5的行，所以，LEFT OUTER JOIN的结果会增加一行，对应的class_name是NULL：

FULL OUTER JOIN(并集)

修改数据(增删改)

插入数据
---
```sql
INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);

-- 插入一条新生信息
INSERT INTO students (class_id, name, gender, score) VALUES (5, '新生', 'M', 88);
-- 插入两条新生信息
INSERT INTO students (class_id, name, gender, score) VALUES
  (1, '大宝', 'M', 87),
  (2, '二宝', 'M', 81);
-- 插入或替换(需要填写全字段或者字段有默认值) 存在则替换(更新)，不存在则插入
REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小黑', 'F', 100);
-- 插入或更新(插入新记录，如果记录已存在则更新)
INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 60) ON DUPLICATE KEY UPDATE name='小绿', gender='M', score=20;
-- 插入或忽略(不存在则插入，存在则不操作)
INSERT IGNORE INTO students (id, name, class_id, score, gender) VALUES (1, '小紫', 2, 50, 'F');

-- 写入查询结果集
-- 创建成绩统计表
CREATE TABLE statistics (
    id BIGINT NOT NULL AUTO_INCREMENT,
    class_id BIGINT NOT NULL,
    average DOUBLE NOT NULL,
    PRIMARY KEY (id)
);
-- 插入各班平均成绩
INSERT INTO statistics (class_id, average) SELECT class_id, AVG(score) FROM students GROUP BY class_id;
```

更新数据
---
```sql
UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;

-- 将 id 为 1 的学生名字改为小牛，分数改为 66
UPDATE students SET name='小牛', score=66 WHERE id=1;
-- 修改多条记录
UPDATE students SET name='小牛', score=77 WHERE id>=5 AND id<=7;
-- 给分数低于 80 分的学生，加上 10 分
UPDATE students SET score=score+10 WHERE score<80;
```
如果WHERE条件没有匹配到任何记录，UPDATE语句不会报错，也不会有任何记录被更新。
如果 UPDATE 不带上 WHERE，则整个表的数据都会被修改，最好在修改前先用 SELECT 测试 WHERE 是否符合预期

删除数据
---
```sql
DELETE FROM <表名> WHERE ...;
```
不带 WHERE 也会删除全部数据，需要注意

事务
---
只有使用 Innodb 的库或表才支持事务
数据库事务具有ACID这4个特性：

A：Atomic，原子性，将所有SQL作为原子工作单元执行，要么全部执行，要么全部不执行；
C：Consistent，一致性，事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了100；
I：Isolation，隔离性，如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离；
D：Duration，持久性，即事务完成后，对数据库数据的修改被持久化存储。
```sql
-- 查看自动提交是否开启
SHOW VARIABLES LIKE 'autocommit'
-- 开启事务自动提交
SET AUTOCOMMIT=1
-- 关闭事务自动提交
SET AUTOCOMMIT=0
-- 开始事务(两种方式)
BEGIN; -- 方式1
START TRANSACTION; -- 方式2
-- 提交事务
COMMIT;
-- 回滚事务
ROLLBACK;
```


其他
---
```bash
# 修改数据库默认编码为 UTF-8/utf8mb4
# docker 下，修改 /etc/mysql/my.cnf
[client]
default-character-set = utf8mb4

[mysqld]
default-storage-engine = INNODB
character-set-server = utf8mb4
collation-server = utf8mb4_general_ci
# 然后重启 MySQL
# 查看 MySQL 整体字符集
SHOW VARIABLES LIKE "%char%";
# 查看表的字符集
SHOW TABLE STATUS FROM test;
# 查看列的字符集
USE test;
SHOW FULL COLUMNS FROM students;
```