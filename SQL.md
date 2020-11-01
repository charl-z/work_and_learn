### SQL学习

### 数据表结构

```sql
SET CHARACTER  SET utf8;
DROP DATABASE IF EXISTS SQLearn;
CREATE DATABASE SQLearn;
USE SQLearn;

CREATE TABLE Student(
  Sid INT PRIMARY KEY,
  Sname VARCHAR(32),
  Sage SMALLINT
);

-- CREATE INDEX Sid ON TABLE (Sid)

CREATE TABLE Course(
  Cid INT PRIMARY KEY,
  Cname  VARCHAR(32),
  Tid SMALLINT
);

CREATE TABLE Score(
  Sid INT,
  Cid INT,
  Score SMALLINT
);

查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩
SELECT * FROM Student RIGHT JOIN Score ON Student.Sid = Score.Sid;
SELECT Student.Sid, Student.Sname, SUM(Score.Score) AS score, COUNT(Score.Cid)  FROM Student RIGHT JOIN Score ON Student.Sid = Score.Sid GROUP BY Student.Sid; 
CREATE TABLE Teacher(
  Tid INT,
  Tname VARCHAR (32)
);

INSERT INTO Student VALUES (1,'赖春梅',16),(2,'郑丽',17),(3,'胡斌',15),(4,'刘桂花',16),(5,'王小红',17),(6,'熊玉',15),(7,'王慧',16),(8,'李小红',17),(9,'张秀芳',15),(10,'李涛',16),(11,'文冬梅',17),(12,'潘文',15),(13,'苏洋',16),(14,'蒙柳',17),(15,'陈玉英',15),(16,'张建',16),(17,'李亮',17),(18,'梁淑珍',15),(19,'刘柳',16) (20,'刘文',16);

INSERT INTO Score VALUES (1,0,62),(1,1,53),(1,2,96),(1,3,54),(1,4,90),(2,0,67),(2,1,53),(2,2,52),(2,3,95),(2,4,98),(3,0,69),(3,1,81),(3,2,60),(3,3,69),(3,4,79),(4,0,86),(4,1,99),(4,2,70),(4,3,51),(4,4,79),(5,0,50),(5,1,97),(5,2,71),(5,3,63),(5,4,65),(6,0,64),(6,1,77),(6,2,58),(6,3,61),(6,4,57),(7,0,61),(7,1,94),(7,2,89),(7,3,87),(7,4,61),(8,0,80),(8,1,96),(8,2,72),(8,3,78),(8,4,95),(9,0,89),(9,1,62),(9,2,53),(9,3,59),(9,4,66),(10,0,65),(10,1,92),(10,2,90),(10,3,85),(10,4,88),(11,0,59),(11,1,80),(11,2,92),(11,3,98),(11,4,63),(12,0,85),(12,1,51),(12,2,89),(12,3,96),(12,4,89),(13,0,52),(13,1,52),(13,2,78),(13,3,57),(13,4,56),(14,0,93),(14,1,66),(14,2,60),(14,3,98),(14,4,51),(15,0,62),(15,1,54),(15,2,81),(15,3,68),(15,4,53),(16,0,78),(16,1,59),(16,2,82),(16,3,65),(16,4,94),(17,0,80),(17,1,58),(17,2,65),(17,3,76),(17,4,86),(18,0,80),(18,1,98),(18,2,68),(18,3,92),(18,4,50),(19,0,66),(19,1,54),(19,2,62),(19,3,53),(19,4,97),(20,4,100);

INSERT INTO Course VALUES (0,'语文',0),(1,'数学',1),(2,'外语',2),(3,'物理',3),(4,'化学',4);
INSERT INTO Teacher VALUES(0,'朱成'),(1,'王明'),(2,'唐琳'),(3,'戴秀珍'),(4,'赖峰');

```

### WHERE连表查询

- 查询学生对应的姓名、年龄、课程、分数、老师信息

```sql
SELECT Student.Sname, Student.`Sage`, Course.Cname, `Score`.Score, Teacher.`Tname`  FROM Teacher, Course, Score, Student where Teacher.Tid = Course.Tid and Score.Cid=Course.Cid and Student.Sid=Score.Sid;

+--------+------+-------+-------+--------+
| Sname  | Sage | Cname | Score | Tname  |
+--------+------+-------+-------+--------+
| 赖春梅 | 16   | 语文  | 62    | 朱成   |
| 赖春梅 | 16   | 数学  | 53    | 王明   |
| 赖春梅 | 16   | 外语  | 96    | 唐琳   |
| 赖春梅 | 16   | 物理  | 54    | 戴秀珍 |
| 赖春梅 | 16   | 化学  | 90    | 赖峰   |
| 郑丽   | 17   | 语文  | 67    | 朱成   |
| 郑丽   | 17   | 数学  | 53    | 王明   |
| 郑丽   | 17   | 外语  | 52    | 唐琳   |
| 郑丽   | 17   | 物理  | 95    | 戴秀珍 |
| 郑丽   | 17   | 化学  | 98    | 赖峰   |
......                   
```

- ###### 查询没学过"王明"老师授课的同学的信息

```sql
select * from `Student` where `Sname` not in (SELECT Student.Sname  FROM Teacher, Course, Score, Student where Teacher.Tid = Course.Tid and Score.Cid=Course.Cid and Student.Sid=Score.Sid and Teacher.`Tname` = '王明');
+-----+-------+------+
| Sid | Sname | Sage |
+-----+-------+------+
0 rows in set
Time: 0.012s
                           
```

####  JOIN 连表查询

**RIGHT JOIN 关键字从右表（table2）返回所有的行，即使左表（table1）中没有匹配。如果左表中没有匹配，则结果为 NULL。**

**JOIN连表查询，条件查询关键字是`ON`**

- 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩

```sql
SELECT Student.Sid, Student.Sname, SUM(Score.Score) AS score, COUNT(Score.Cid) as total  FROM Student RIGHT JOIN Score ON Student.Sid = Score.Sid GROUP BY Student.Sid;
+-----+--------+-------+-------+
| Sid | Sname  | score | total |
+-----+--------+-------+-------+
| 1   | 赖春梅 | 355   | 5     |
| 2   | 郑丽   | 365   | 5     |
| 3   | 胡斌   | 358   | 5     |
| 4   | 刘桂花 | 385   | 5     |
| 5   | 王小红 | 346   | 5     |
| 6   | 熊玉   | 317   | 5     |
| 7   | 王慧   | 392   | 5     |
| 8   | 李小红 | 421   | 5     |
| 9   | 张秀芳 | 329   | 5     |
| 10  | 李涛   | 420   | 5     |
| 11  | 文冬梅 | 392   | 5     |
| 12  | 潘文   | 410   | 5     |
| 13  | 苏洋   | 295   | 5     |
| 14  | 蒙柳   | 368   | 5     |
| 15  | 陈玉英 | 318   | 5     |
| 16  | 张建   | 378   | 5     |
| 17  | 李亮   | 365   | 5     |
| 18  | 梁淑珍 | 388   | 5     |
| 19  | 刘柳   | 332   | 5     |
+-----+--------+-------+-------+
19 rows in set
```

