
## 第1章 数据库概述
### 1.2 数据库范式

* 第一范式：数据库需要满足的最低要求的范式是第一范式。第一范式的要求表中不能有重复字段，并且每个字段不能再拆分。  
* 第二范式：  
* 第三范式：  
* BCN范式： 

### 1.3 SQL语言
SQL语言分为三个部分：  

* 数据定义语言（Data Definition Language，简称为DDL）: CREATE TABLE ...  
* 数据操作语言（Data Manipulation Language，简称为DML）: SELECT, INSERT, UPDATE, DELETE  
* 数据控制语言（Data Control Language，简称为DCL）:GRANT, REVOKE  


## 第4章  MySQL数据类型
### 4.3 日期和时间

* year 年 'YYYY'
* date 日期  'YYYY-MM-DD'
* time 时间 'HH:MM:SS' 获取当前时间NOW()
* datetime 日期时间 'YYYY-MM-DD HH:MM:SS'
* timestamp 时间（区 ），范围小，支持时间（区 ）
* datetimedatetime 最通用， year,date,time可以节省一些空间。

### 字符串

### 4.5 二进制

## 第5章  操作数据库
### 5.1 显示、创建、删除数据库

CREATE DATABASE mydb1; 创建一个名称为mydb1的数据库。

SHOW CREATE DATABASE mydb1; 查看数据库的创建细节

CREATE DATABASE mydb2 CHARACTER SET utf8; 创建一个使用utf8字符集的mydb2数据库。

CREATE DATABASE IF NOT EXISTS mydb3 CHARACTER SET utf8 COLLATE utf8_general_ci; 创建一个使用utf8字符集，并带校对规则的mydb3数据库。

SHOW DATABASES;  查看当前数据库服务器中的所有数据库

SHOW CREATE DATABASE mydb1; 查看前面创建的mydb2数据库的定义信息

DROP DATABASE mydb3;  删除前面创建的mydb3数据库

#### 备份： 
mysqldump -u root -p mydb2 > d:\mydb2.sql回车(可以无分号结束) 
#### 恢复： 
source d:\mydb2.sql;回车(需要分号结束) 


### 5.3 数据库存储引擎
show engines;  
show variables like '%engine%';  
#### innodb
* 最常用，支持事务，回滚，自增，外键
* 表结构存在.frm文件中
* 数据和索引存在表空间中
* 读写效率稍差，占用空间大

#### myisam
* 表结构存在.frm文件中
* .myd 存储数据
* .myi 存储索引
* 快速，占空间小，不支持事务和并发

### 5.4 数据库操作实践
#### DDL：数据定义语言 Data Definition Language
	作用：定义数据库或者表结构的。
	操作的对象：数据库或表的结构的。
	关键字：CREATE ALTER DROP
CREATE DATABASE test CHARACTER SET utf8;  
USE test;  创建表之前一定先选择数据库  
```
CREATE TABLE employee(  
	id int,  
	name varchar(200),  
	gender varchar(10),  
	birthday date,  
	entry_date date,  
	job varchar(200),  
	salary float(8,2),  
	resume text  
); 创建一个员工表  
```  
SHOW TABLES; 显示当前数据库中的所有表格  
ALTER TABLE employee ADD image blob; 在上面员工表的基本上增加一个image列  
DESC employee; 查看表结构的定义  
ALTER TABLE employee MODIFY job varchar(60); 修改job列，使其长度为60  
ALTER TABLE employee DROP image; 删除image列  
RENAME TABLE employee TO user; 表名改为user  
SHOW CREATE TABLE user; 查看表的创建细节  
ALTER TABLE user CHARACTER SET utf8; 修改表的字符集为utf8  
ALTER TABLE user CHANGE name username varchar(100); 列名name修改为username  


#### DML:数据操作语言 Data Manipulation Language 
	作用：操作表中的数据的。
	关键：INSERT UPDATE DELETE SELECT

**INSERT**  
SELECT * FROM user; 查看表中的所有记录  
使用insert语句向表中插入三个员工的信息  
insert into user(username,id,birthday,entry_date,job,salary,resume)
values('杰克',2,'2011-10-8','2011-12-31','software',5000.1,'你好');
insert into user values(3,'马利','2011-10-8','2011-12-31','software',5000.1,'你好',NULL);
insert into user values(4,'马利','2011-10-8','2011-12-31','software',5000.1,NULL,NULL);
insert into user(id,username,birthday,entry_date,job,salary,image) 
values(5,'马利','2011-10-8','2011-12-31','software',5000.1,NULL);

插入中文时的问题：（编码问题）  
SHOW VARIABLES LIKE 'character%'; 查看数据库目前的各种编码  
SET character_set_client=utf8; 通知服务器客户端使用的编码字符集  
SET character_set_results=utf8; 显示时乱码  

**UPDATE**  
UPDATE user SET salary=5000; 将所有员工薪水修改为5000元  
UPDATE user SET salary=3000 WHERE username='王翔云'; 将姓名为’王翔云’的员工薪水修改为3000元  
UPDATE user SET salary=4000,job='CMO' WHERE username='王翔云';将姓名为’王翔云’的员工薪水修改为4000元,job改为CMO  
UPDATE user SET salary=salary+1000 WHERE username='zql'; 将zql的薪水在原有基础上增加1000元  

**DELETE**  
DELETE FROM user WHERE username='王翔云'; 删除表中名称为’王翔云’的记录  
DELETE FROM user;  按行删除表中的所有记录，但会保留表，适合删除数据量不大的数据，可按条件删除  
TRUNCATE user;  复制原表结构-〉一次性删除整表 -> 自动恢复原表结构，适合删除数据量较大的数据，不能按条件删除  
drop table：删除表本身  
**注意！**删除记录时，一定要留意表间的关联关系
     
**SELECT**  
SELECT * FROM student;(不建议使用) 查询表中所有学生的信息  
SELECT id,name,chinese,english,math FROM student;  
SELECT name,english FROM student; 查询表中所有学生的姓名和对应的英语成绩  
SELECT DISTINCT english FROM student; 过滤表中重复数据  
SELECT id,name,math+10 FROM student;在所有学生数学分数上加10分特长分  
SELECT name,chinese+english+math FROM student;统计每个学生的总分  
SELECT name AS 姓名,chinese+english+math 总分 FROM student;使用别名表示学生分数  
SELECT name,english,chinese,math FROM student WHERE name='王五';查询姓名为王五的学生成绩  
SELECT name,english,chinese,math FROM student WHERE english>90;查询英语成绩大于90分的同学  
SELECT name,chinese+english+math FROM student WHERE (chinese+english+math)>200;查询总分大于200分的所有同学  

**WHERE语句**   
SELECT * FROM student WHERE english BETWEEN 84 AND 85;查询英语分数在 80－90之间的同学  
SELECT * FROM student WHERE math IN (89,90,91);查询数学分数为89,90,91的同学  
SELECT * FROM student WHERE name LIKE '李%';查询所有姓李的学生成绩  
SELECT * FROM student WHERE math>80 AND chinese>80;查询数学分>80，语文分>80的同学  
SELECT * FROM student ORDER BY math;//默认是升序 对数学成绩排序后输出  
SELECT name,chinese+english+math FROM student ORDER BY (chinese+english+math) DESC;对总分排序后输出，然后再按从高到低的顺序输出  
SELECT name,math FROM student WHERE name LIKE '李%' ORDER BY math;对姓李的学生数学成绩排序输出  


### 数据完整性
三个方面：  
#### 1、实体完整性：规定表中的一行在表中是唯一的实体。
	一般是通过定义主键的形式来实现的。
	关键字：PRIMARY KEY
	特点：不能为null，必须唯一
	
	CREATE TABLE SHANG_HAI1(
		id int PRIMARY KEY,
		name varchar(100)
	);
	//实际开发中不建议使用。
	CREATE TABLE shanghai2(
		id int PRIMARY KEY auto_increment,
		name varchar(100)
	);
	insert into SHANG_HAI1(id,name) values(select user_id,name);
	insert into shanghai2 (name) values('aa');

#### 2、域完整性
	指数据库表的列（即字段）必须符合某种特定的数据类型或约束。
	NOT NULL：不能为空
	UNIQUE：必须唯一
	CREATE TABLE shanghai3(
		id int PRIMARY KEY,
		name varchar(100) NOT NULL,
		idnum varchar(100) unique
	);
	
	关于主键：
	（建议）逻辑主键：给编程人员用的。与具体业务无关
	业务主键：用户也可以用。与具体业务有关了。
	
#### 3、参照完整性（多表设计）	
		一对多
		create table department(
			id int primary key,
			name varchar(100)
		);
		create table employee(
			id int primary key,
			name varchar(100),
			salary float(8,2),
			dept_id int,
			constraint dept_id_fk foreign key(dept_id) references department(id)
		);
		
		多对多
		create table teacher(
			id int primary key,
			name varchar(100),
			salary float(8,2)
		);
		create table student1(
			id int primary key,
			name varchar(100),
			grade varchar(10)
		);
		create table teacher_student1(
			t_id int,
			s_id int,
			primary key(t_id,s_id),
			constraint t_id_fk foreign key(t_id) references teacher(id),
			constraint s_id_fk foreign key(s_id) references student1(id)
		);
		
		一对一
		create table human(
			id int primary key,
			name varchar(100)
		);
		create table idcard(
			id int primary key,
			num varchar(100),
			constraint huanm_id_fk foreign key(id) references human(id)
		);
		
## 第6章  创建、修改和删除表