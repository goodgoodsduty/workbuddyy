# 操作技能 · 技能二 MySQL数据库（详解）

> **分值占比**：约16分（占操作技能20%）
> **考频**：⭐⭐⭐⭐（重点技能）
> **难度系数**：⭐⭐☆☆☆（偏记忆）
> **学习时长建议**：3-4周

---

## 一、数据库基础概念

### 1.1 数据库（DB）

**【定义】** 长期存储在计算机内、有组织、可共享的数据集合

### 1.2 数据库管理系统（DBMS）

**【功能】** 数据的**定义、操作、存储、保护、维护**

**【常见DBMS】**
- MySQL（开源、主流）
- Oracle（商业、强大）
- SQL Server（微软）
- PostgreSQL
- MongoDB（NoSQL）

### 1.3 关系数据库

**【核心概念】**

| 概念 | 说明 | 例 |
|------|------|---|
| **表（Table）** | 二维表 | 学生表 |
| **记录（Record）** | 一行数据 | 一名学生 |
| **字段（Field）** | 一列数据 | 学号、姓名 |
| **主键（PK）** | 唯一标识 | 学号 |
| **外键（FK）** | 关联其他表 | 班级号 |
| **索引（Index）** | 加快查询 | - |

**【SQL 语言分类】**

| 类别 | 全称 | 命令 |
|------|------|------|
| DDL | Data Definition Language | CREATE, ALTER, DROP |
| DML | Data Manipulation Language | INSERT, UPDATE, DELETE |
| DQL | Data Query Language | SELECT |
| DCL | Data Control Language | GRANT, REVOKE |

---

## 二、MySQL 数据类型 ⚠️ 必考

### 2.1 数值类型

| 类型 | 字节 | 范围 |
|------|------|------|
| TINYINT | 1 | -128 ~ 127 |
| SMALLINT | 2 | -32768 ~ 32767 |
| INT | 4 | -2³¹ ~ 2³¹-1 |
| BIGINT | 8 | -2⁶³ ~ 2⁶³-1 |
| FLOAT | 4 | 单精度 |
| DOUBLE | 8 | 双精度 |
| DECIMAL(M, D) | - | 精确小数 |

### 2.2 字符串类型

| 类型 | 说明 |
|------|------|
| CHAR(n) | 固定长度字符串 |
| VARCHAR(n) | 可变长度（n最大65535）|
| TEXT | 长文本 |

### 2.3 日期类型

| 类型 | 格式 |
|------|------|
| DATE | YYYY-MM-DD |
| TIME | HH:MM:SS |
| DATETIME | YYYY-MM-DD HH:MM:SS |
| TIMESTAMP | 时间戳 |

---

## 三、DDL 数据定义 ⚠️ 必考

### 3.1 创建数据库

```sql
CREATE DATABASE dbname;
CREATE DATABASE IF NOT EXISTS student_db;
```

### 3.2 删除数据库

```sql
DROP DATABASE dbname;
DROP DATABASE IF EXISTS student_db;
```

### 3.3 使用数据库

```sql
USE dbname;
```

### 3.4 创建表 ⚠️ 必考

```sql
CREATE TABLE 表名 (
    字段1 数据类型 [约束],
    字段2 数据类型 [约束],
    ...
    [PRIMARY KEY (字段)]
);
```

**【常用约束】**

| 约束 | 说明 |
|------|------|
| PRIMARY KEY | 主键 |
| NOT NULL | 非空 |
| UNIQUE | 唯一 |
| DEFAULT | 默认值 |
| AUTO_INCREMENT | 自增 |
| FOREIGN KEY | 外键 |

**【例题1】** 创建学生表
```sql
CREATE TABLE student (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL,
    age INT,
    gender CHAR(1) DEFAULT 'M',
    birthday DATE
);
```

### 3.5 修改表结构

```sql
-- 添加列
ALTER TABLE student ADD email VARCHAR(50);

-- 修改列
ALTER TABLE student MODIFY age SMALLINT;

-- 删除列
ALTER TABLE student DROP email;

-- 修改表名
ALTER TABLE student RENAME TO stu;
```

### 3.6 删除表

```sql
DROP TABLE student;
DROP TABLE IF EXISTS student;
```

---

## 四、DML 数据操作 ⚠️ 必考

### 4.1 插入数据 INSERT

**【单条插入】**
```sql
INSERT INTO student (name, age) VALUES ('Tom', 18);
```

**【多条插入】**
```sql
INSERT INTO student (name, age) VALUES 
('Alice', 20), 
('Bob', 22), 
('Charlie', 19);
```

**【全列插入】**
```sql
INSERT INTO student VALUES (NULL, 'Tom', 18, 'M', '2005-01-01');
```

### 4.2 更新数据 UPDATE ⚠️ 必考

```sql
UPDATE student SET age = 20 WHERE name = 'Tom';
UPDATE student SET age = age + 1;
UPDATE student SET age = 20, gender = 'F' WHERE id = 1;
```

### 4.3 删除数据 DELETE ⚠️ 必考

```sql
DELETE FROM student WHERE name = 'Tom';
DELETE FROM student;       -- 删除所有记录
TRUNCATE TABLE student;    -- 清空表（更快）
```

**【DELETE vs TRUNCATE】**

| 特点 | DELETE | TRUNCATE |
|------|--------|---------|
| 类型 | DML | DDL |
| 条件 | 可加 WHERE | 不可加 |
| 自增 | 不重置 | 重置 |
| 回滚 | 可回滚 | 不可回滚 |
| 速度 | 慢 | 快 |

---

## 五、DQL 数据查询 ⚠️ 必考（最重要）

### 5.1 基本查询

```sql
SELECT * FROM student;              -- 查询所有
SELECT name, age FROM student;      -- 查询指定列
SELECT DISTINCT age FROM student;   -- 去重
```

### 5.2 条件查询 WHERE ⚠️ 必考

| 运算符 | 说明 |
|--------|------|
| =, != | 等于/不等于 |
| >, <, >=, <= | 大小 |
| BETWEEN...AND | 范围 |
| IN(...) | 集合 |
| LIKE | 模糊 |
| IS NULL / IS NOT NULL | 空值 |
| AND, OR, NOT | 逻辑 |

**【例题2】** 各种条件查询
```sql
SELECT * FROM student WHERE age = 20;
SELECT * FROM student WHERE age BETWEEN 18 AND 22;
SELECT * FROM student WHERE name IN ('Tom', 'Alice');
SELECT * FROM student WHERE name LIKE 'T%';      -- 以T开头
SELECT * FROM student WHERE name LIKE '%m';      -- 以m结尾
SELECT * FROM student WHERE name LIKE '%o%';     -- 包含o
SELECT * FROM student WHERE age IS NULL;
```

**【LIKE 通配符】** ⚠️ 必背

| 通配符 | 含义 |
|--------|------|
| % | 任意长度字符 |
| _ | 单个字符 |

### 5.3 排序 ORDER BY

```sql
SELECT * FROM student ORDER BY age ASC;       -- 升序（默认）
SELECT * FROM student ORDER BY age DESC;      -- 降序
SELECT * FROM student ORDER BY age DESC, name ASC;  -- 多列排序
```

### 5.4 限制 LIMIT

```sql
SELECT * FROM student LIMIT 5;          -- 前5条
SELECT * FROM student LIMIT 5, 10;      -- 从第6条开始取10条
SELECT * FROM student LIMIT 10 OFFSET 5;
```

### 5.5 聚合函数 ⚠️ 必考

| 函数 | 功能 |
|------|------|
| COUNT() | 计数 |
| SUM() | 求和 |
| AVG() | 平均 |
| MAX() | 最大 |
| MIN() | 最小 |

```sql
SELECT COUNT(*) FROM student;
SELECT AVG(age) FROM student;
SELECT MAX(score), MIN(score) FROM score;
```

### 5.6 分组 GROUP BY ⚠️ 必考

```sql
SELECT gender, COUNT(*) AS 人数, AVG(age) AS 平均年龄
FROM student
GROUP BY gender;
```

**【HAVING】** 分组后筛选（WHERE 是在分组前）

```sql
SELECT gender, AVG(age)
FROM student
GROUP BY gender
HAVING AVG(age) > 20;
```

**【WHERE 和 HAVING 区别】**

| 子句 | 作用 | 位置 |
|------|------|------|
| WHERE | 分组前筛选 | 跟在 FROM 后面 |
| HAVING | 分组后筛选 | 跟在 GROUP BY 后面 |

### 5.7 多表查询 ⚠️ 必考

**【内连接】** 只返回匹配的行

```sql
SELECT s.name, c.name
FROM student s
INNER JOIN class c ON s.class_id = c.id;
```

**【左连接】** 返回左表所有行 + 右表匹配行

```sql
SELECT s.name, c.name
FROM student s
LEFT JOIN class c ON s.class_id = c.id;
```

**【右连接】** 返回右表所有行 + 左表匹配行

**【交叉连接】** 笛卡尔积

**【自连接】** 表与自身连接

### 5.8 子查询 ⚠️

```sql
-- 子查询作为WHERE条件
SELECT * FROM student 
WHERE age > (SELECT AVG(age) FROM student);

-- 子查询作为表
SELECT * FROM (SELECT * FROM student WHERE age > 18) AS t;
```

### 5.9 完整查询语句结构

```sql
SELECT [DISTINCT] 字段列表
FROM 表名
[WHERE 条件]
[GROUP BY 分组字段]
[HAVING 分组条件]
[ORDER BY 排序字段 [ASC|DESC]]
[LIMIT 数字];
```

**【执行顺序】** ⚠️ 必考

```
FROM → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```

---

## 六、综合查询例题 ⚠️ 必考

**【题目】** 有如下表结构：

```sql
学生表 student(sid, sname, sage, sgender)
课程表 course(cid, cname, ccredit)
成绩表 sc(sid, cid, score)
```

**【例题3】** 查询所有学生的姓名和年龄。
```sql
SELECT sname, sage FROM student;
```

**【例题4】** 查询年龄在18-22之间的女同学。
```sql
SELECT * FROM student 
WHERE sage BETWEEN 18 AND 22 AND sgender = 'F';
```

**【例题5】** 查询每个学生选修课程的门数。
```sql
SELECT sid, COUNT(*) AS 课程数
FROM sc
GROUP BY sid;
```

**【例题6】** 查询选课门数超过3门的学生。
```sql
SELECT sid, COUNT(*) AS 课程数
FROM sc
GROUP BY sid
HAVING COUNT(*) > 3;
```

**【例题7】** 查询选了"数据库"课程的学生姓名。
```sql
SELECT sname 
FROM student
WHERE sid IN (
    SELECT sid FROM sc
    WHERE cid = (SELECT cid FROM course WHERE cname = '数据库')
);
```

**【例题8】** 查询每个学生的姓名和选课总成绩。
```sql
SELECT s.sname, SUM(sc.score) AS 总分
FROM student s
LEFT JOIN sc sc ON s.sid = sc.sid
GROUP BY s.sid, s.sname;
```

**【例题9】** 查询每门课程的平均分。
```sql
SELECT c.cname, AVG(sc.score) AS 平均分
FROM course c
JOIN sc ON c.cid = sc.cid
GROUP BY c.cid, c.cname;
```

**【例题10】** 查询选了所有课程的学生姓名。
```sql
SELECT sname FROM student s
WHERE NOT EXISTS (
    SELECT * FROM course c
    WHERE NOT EXISTS (
        SELECT * FROM sc
        WHERE sc.sid = s.sid AND sc.cid = c.cid
    )
);
```

---

## 七、MySQL 函数

### 7.1 字符串函数

| 函数 | 作用 |
|------|------|
| CONCAT(s1, s2) | 拼接 |
| LENGTH(s) | 长度（字节）|
| CHAR_LENGTH(s) | 字符数 |
| UPPER(s) / LOWER(s) | 大小写 |
| SUBSTRING(s, start, len) | 截取 |
| TRIM(s) | 去空格 |
| REPLACE(s, old, new) | 替换 |

### 7.2 数值函数

| 函数 | 作用 |
|------|------|
| ABS(x) | 绝对值 |
| CEIL(x) | 向上取整 |
| FLOOR(x) | 向下取整 |
| ROUND(x, n) | 四舍五入 |
| MOD(a, b) | 取余 |
| RAND() | 随机数 |

### 7.3 日期函数

| 函数 | 作用 |
|------|------|
| NOW() | 当前时间 |
| CURDATE() | 当前日期 |
| CURTIME() | 当前时间 |
| YEAR(d) | 年 |
| MONTH(d) | 月 |
| DAY(d) | 日 |
| DATEDIFF(d1, d2) | 差值 |

---

## 八、视图与索引

### 8.1 视图

**【创建视图】**
```sql
CREATE VIEW v_student AS
SELECT id, name, age FROM student;
```

**【使用视图】**
```sql
SELECT * FROM v_student;
```

**【删除视图】**
```sql
DROP VIEW v_student;
```

### 8.2 索引

**【创建索引】**
```sql
CREATE INDEX idx_name ON student(name);
```

**【删除索引】**
```sql
DROP INDEX idx_name ON student;
```

---

## 九、上机环境

### 9.1 phpStudy 集成环境

- 一键启动 MySQL
- 包含 phpMyAdmin 可视化工具

### 9.2 MySQL Workbench

- 官方客户端
- 图形化管理

### 9.3 命令行登录

```bash
mysql -u root -p
```

---

## 十、考点速记表

| 序号 | 考点 | 重要度 |
|------|------|--------|
| 1 | 数据类型 | ⭐⭐⭐⭐ |
| 2 | CREATE TABLE | ⭐⭐⭐⭐⭐ |
| 3 | INSERT/UPDATE/DELETE | ⭐⭐⭐⭐⭐ |
| 4 | SELECT 基础 | ⭐⭐⭐⭐⭐ |
| 5 | WHERE 条件 | ⭐⭐⭐⭐⭐ |
| 6 | 模糊查询 LIKE | ⭐⭐⭐⭐ |
| 7 | 聚合函数 | ⭐⭐⭐⭐⭐ |
| 8 | GROUP BY + HAVING | ⭐⭐⭐⭐⭐ |
| 9 | 多表连接 | ⭐⭐⭐⭐⭐ |
| 10 | 子查询 | ⭐⭐⭐⭐ |
| 11 | LIMIT | ⭐⭐⭐ |

---

> 📌 **学习建议**：
> 1. SQL 语句多练多写
> 2. SELECT 是重点，要熟练
> 3. 多表查询必须掌握
> 4. 在 phpStudy 中实操
> 5. 背熟执行顺序
