[TOC]

# MySQL

## 1. 数据库的基本概念

1. 数据库的英文单词：DataBase  简称：DB
2. 什么是数据库？
    * 用于存储和管理数据的仓库。
3. 数据库的特点：
    1. 持久化存储数据的。其实数据库就是一个文件系统。
    2. 方便存储和管理数据的。
    3. 使用了统一的方式操作数据库  --  SQL

## 2. MySQL数据库软件

1. 安装
    * 查看是否安装成功：打开 cmd --> `mysql -uroot -p密码`

2. 卸载
    1. 去 `mysql`的安装目录下找到`my.ini`文件
        * 复制 `datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"`
    2. 卸载 `MySQL`
    3. 删除 `C:/ProgramData`目录下的 `MySQL`文件夹

3. 配置
    * MySQL服务启动
        1. 手动。
        2. cmd --> services.msc：打开服务的窗口
        3. 使用管理员打开 cmd
            * net start mysql：启动 mysql的服务
            * net stop mysql：关闭 mysql的服务
    * MySQL登陆
        1. mysql -uroot -p密码
        2. mysql -hip地址 -uroot -p链接目标的密码 
        3. mysql --host=ip地址 --user=root --passward=连接目标的密码
    * MySQL退出
        1. exit
        2. quit
    * MySQL目录结构
        1. MySQL安装目录
            * 配置文件：my.ini
        2. MySQL数据目录
            * 数据库：文件夹
            * 表：文件
            * 数据(记录)：文件中存储的数据

## 3. SQL

### 1. 什么是MySQL？
  Structured Query Language：结构化查询语言。其实就是定义了操作所有**关系型数据库的规则**。每一种数据库操作的方式存在不一样的地方，称为“方言”。

### 2. SQL通用语法

1. SQL语句可以单行或者多行书写，以分号结尾。
2. 可使用空格和缩进来增强语句的可读性。
3. MySQL数据库的 SQL 语句是不区分大小写的，关键字建议使用大写。
4. 3种注释：
    * 单行注释：-- 注释内容 或者 # 注释内容(mysql特有)
    * 多行注释：/* 注释内容 */

### 3. SQL分类

1. DDL(Data Definition Language)数据定义语言
    用来定义数据库对象：数据库，表，列等。关键字：create, drop,alter 等
2. DML(Data Manipulation Language)数据操作语言
	用来对数据库中表的数据进行增删改。关键字：insert, delete, update 等
3. DQL(Data Query Language)数据查询语言
	用来查询数据库中表的记录(数据)。关键字：select, where 等
4.  DCL(Data Control Language)数据控制语言(了解)
	用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT， REVOKE 等

![SQL分类.jpg](https://ae01.alicdn.com/kf/H87e06f71f0134727989ca24efc38941fi.jpg)


> 小结：
> DDL：操作数据库和表
> DML：增删改表中的数据
> DQL：查询表中的数据
> DCL：授权

### 4. DDL：操作数据库、表

####  操作数据库：CRUD
1. C(Create)：创建
    * 创建数据库：
        * `create database 数据库名称;`
    * 创建数据库，判断不存在，再创建：
        * `create database if not exists 数据库名称;`
    * 创建数据库，指定字符集：
        * `create database 数据库名称 character set 字符集名称;`
    * 练习：创建db4数据库，判断是否存在，并制定字符集 gbk：
        * `create database if not exists 数据库名称 character set gbk;`

2. R(Retrieve)：查询
    * 查询所有数据库的名称：
        * `show databases;`
    * 查询某个数据库的字符集：查询某个数据库的创建语句
        * `show create database 数据库名称;`
3. U(Update)：修改
    * 修改数据库的字符集
        * `alter database 数据库名称 character set 字符集名称;`
4. D(Delete)：删除
    * 删除数据库
        * `drop database 数据库名称;`
    * 判断数据库存在，再删除
        * `drop database if exists 数据库名称;`
5. 使用数据库
    * 查询当前正在使用的数据库名称
        * `select database();`
    * 使用数据库
        * `use 数据库名称;`


#### 操作表

1. C(Create)：创建
    * 语法：
        ```mysql
        create table 表名(
            列名1 数据类型1，
            列名2 数据类型2，
            ...
            列名n 数据类型n
        );
        ```
> **注意：最后一列，不需要加逗号(,)**   
> 

* 数据库类型
    1. `int`:整数类型
        * `age int,`
    2. `double`：小数类型
        * `score double(5,2),`：指定为5位小数，小数点后保留2位。
    3. `date`：日期，只包含年月日，yyyy-MM-dd
    4. `datatime`：日期，包含年月日时分秒 yyyy-MM-dd HH:mm:ss
    5. `datestamp`：时间错类型，包含年月日时分秒，yyyy-MM-dd HH:mm:ss
        * 如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间，来自动赋值。
    6. `varchar`：字符串
        * `name varchar(20)`：姓名最大为20个字符
        * zhangsan 8个字符，张三 2个字符

* 创建学生表：

```mysql
create table student(
    id int,
    name varchar(32),
    age int,
    score double(4, 1),
    birthday date,
    insert_time timestamp
);

```

* 复制表
    * `create table 表名 like 被复制的表名称;`

2. R(Retrieve)：查询
    * 查询某个数据库中所有的表名称
        * `show tables;`
    * 查询表的结构
        * `desc 表名;`
3. U(Update)：修改
    1. 修改表名
        `alter table 表名 rename to 新的表名;`
    2. 修改表的字符集
        `alter table 表名 character set 字符集名称;`
    3. 添加一列
        `alter table 表名 add 列名;`
    4. 修改列名称 类型
        `alter table 表名 change 列名 新列名 新数据类型;`
        `alter table 表名 modify 列名 新数据类型;`
    5. 删除列
        `alter table 表名 drop 列名;`


4. D(Delete)：删除
    * `drop table 表名;`
    * `drop table if exists 表名;`

### 5. DML：增删改表中数据

1. 添加数据
    * 语法：
        * `insert into 表名(列名1,列名2,...,列名n) values(值1,值2,..,值n);`

> 注意事项：
> 1. 列名和值要一一对应。
> 2. 如果表名后不定义列名，则默认会给所有的列添加值。
> `insert into 表名 values(值1,值2,..,值n);`
> 3. 除了数字类型，其他类型都需要使用引号(单双都可以)引起来。

2. 删除数据
    * 语法：
        * `delete from 表名 [where 条件];`

> 注意：
> 1. 如果不加条件，则删除表中的所有记录，即清空表。
> 2. 如果要删除所有记录
> > 1. `delete from 表名;` -- 不推荐使用。有多少条记录就会执行多少次删除操作。
> > 2. `truncate table 表名;` -- 推荐使用，效率更高。先删除(drop)表，然后再创建(create)一张一样的空表。

3. 修改数据
    * 语法：
        * `update 表名 set 列名1 = 值1, 列名2 = 值2,...,列名n = 值n [where 条件];`

> 注意：
>
> 1. 如果不加任何条件，则会将表中的所有记录全部修改。
> 


```mysql

INSERT INTO stu(id, NAME, age) VALUES(1, "张无忌", 19);
SELECT * FROM stu;

INSERT INTO stu VALUES(2, "赵敏",20 ,100.0, NULL, NULL);

INSERT INTO stu VALUES(3, '张三丰',80, 90.0, '1873-10-1', NULL);

DELETE FROM stu; -- 删除指定的表。有多少条记录，就会执行多少次删除操作。

DELETE FROM stu WHERE id = 1;

TRUNCATE TABLE stu; -- 删除指定的表。先删除表，在创建一张空表。

SELECT * FROM stu; -- 查询表中的所有记录

UPDATE stu SET age = 117 WHERE id = 3; -- 修改表中的记录

UPDATE stu SET age = 118, score = 91 WHERE id = 3;

UPDATE stu SET age = 20; -- 会将表中的所有此条记录修改


-- 创建一张新的表 student

CREATE TABLE student(
	id INT, -- 编号
	NAME VARCHAR(20), -- 姓名
	age INT, -- 年龄
	sex VARCHAR(5), -- 性别
	address VARCHAR(100), -- 地址
	math INT, 
	english INT
);

-- 向 student 中添加数据

INSERT INTO student(id, NAME, age, sex, address, math, english) VALUES (1,'马云',50,'男','杭州',2,98),(2,'俞敏洪',40,'男','北京', 69, 99), (3,'刘青',60,'男','杭州', 70, 80), (4, '比尔盖茨', 70, '男','北京', 90, 90);
```


### 6. DQL:查询表中的记录

#### 1. 语法:
```mysql
select 
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段
having
	分组之后的条件
order by
	排序
limit
	分页限定
```

#### 2.查询基础
1. 多个字段的查询
    `select 字段名1, 字段名2, ... from 表名;`
    * 注意：如果查询所有字段，则可以使用 * 来替代字段列表。
2. 去除重复
    * distinct
3. 计算列
    * 一般可以使用四则运算计算一些列的值(一般只会进行数值型的计算)
    * `ifnull(表达式1, 表达式2)`：null参与的运算，计算结果都为null
        * 表达式1：哪个字段需要判断是否为 null
        * 如果该字段为 null 后的替换值

4. 起别名 
    * `as`：as也可以省略。


```mysql
-- 查询 姓名 和 年龄
select 
	name, -- 姓名
	age   -- 年龄
from
	student; -- 学生表

-- 去除重复的结果集
select distinct address from student;

select distinct name, address from student;
```


#### 3.条件查询
1. where子句后跟条件
2. 运算符
    * <、>、<=、>=、=、!=、<>
    * between ... and
    * in(集合)
    * like:`模糊查询`
        * 占位符：
            * _:单个的任意字符
            * %:多个的任意字符
    * is null
    * is not null
    * and 或 &&
    * or 或 ||
    * not 或 !

```mysql
-- 查询所有人中年龄大于等于 20的
SELECT * FROM student WHERE age >= 20;

-- 查询所有人中年龄等于 20的
SELECT * FROM student WHERE age = 19;

-- 查询年龄不等于19的
SELECT * FROM student WHERE age != 19;
SELECT * FROM student WHERE age <> 19;

-- 查询年龄大于等于 20 小于等于 60的

SELECT * FROM student WHERE age >= 20 AND age <= 60;
SELECT * FROM student WHERE age >= 20 && age <= 60;
SELECT * FROM student WHERE age BETWEEN 20 AND 60;

-- 查询年龄 40,50,19
SELECT * FROM student WHERE age = 19 OR age = 40 OR age = 50;
SELECT * FROM student WHERE age IN(19,40,50);

-- 查询英语成绩为null
SELECT * FROM student WHERE english = NULL; -- 是不对的

SELECT * FROM student WHERE english IS NULL;

-- 查询英语成绩不为null
SELECT * FROM student WHERE english IS NOT NULL;

INSERT INTO student VALUES(7,'李三',30,'男','天津',NULL,80);

-- 查询姓马的有哪些人？
SELECT * FROM student WHERE NAME LIKE '马%';
-- 查询第二个字是三的人?
SELECT * FROM student WHERE NAME LIKE "_三%";
-- 查询三个字的人
SELECT * FROM student WHERE NAME LIKE "___";
-- 查询姓名中包含三的人
SELECT * FROM student WHERE NAME LIKE "%三%";

```

#### 4.排序查询

* 语法：`order by 子句;`
    * `order by 排序字段1 排序方式1,排序字段2 排序方式2,...`
* 排序方式：
    * `ASC`:升序，默认的。
    * `DESC`:降序。
* 注意：
    * 如果有多个排序条件，当前边的条件值一样时，才会判断第二条件。

```mysql
-- 根据数学成绩按降序排序
select * from student order by math desc;
-- 根据数学成绩按升序排序
select * from student order by math ASC;
-- 先按照数学成绩排序，如果一样，再按照英语成绩排序
select * from student order by math Desc,english;

```

#### 5.聚合函数

* **将一列数据作为一个整体，进行纵向计算**

1. `count`：计算个数
    * 一般选择非空的列：主键
    * `count(*)` -- 不推荐
2. `max`：计算最大值
3. `min`：计算最小值
4. `sum`：计算和
5. `avg`：计算平均值

```mysql
-- count
SELECT COUNT(IFNULL(math,0)) FROM student;

SELECT COUNT(id) FROM student;

SELECT MAX(math) FROM student;
SELECT MIN(math) FROM student;

SELECT SUM(math) FROM student;
SELECT AVG(math) FROM student;

```



> 注意：**聚合函数的计算，排除 null 值**
> 解决方案：
>
> 1. 选择不包含非空的列进行计算
> 2. IFNULL函数

#### 6.分组查询

* 语法：`group by 分组字段;`

> 注意：
> 1. 分组之后查询的字段：**分组字段，聚合函数**
> 2. `where`和`having`的区别？
> > 1. `where`在分组之前进行限定，如果不满足条件，则不参与分组；`having`在分组之后进行限定，如果不满足结果，则不会被查询出来。
> > 2. `where`后**不可以**跟聚合函数，`having`**可以**进行聚合函数的判断。


```mysql
-- 按照性别分组，分别查询男、女同学的平均分
SELECT sex,AVG(math) FROM student GROUP BY sex;
-- 按照性别分组，分别查询男、女同学的平均分、人数
SELECT sex,AVG(math),COUNT(id) FROM student GROUP BY sex;
-- 按照性别分组，分别查询男、女同学的平均分、人数.
-- 要求：分数低于70分的人，不参与分组
SELECT sex,AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex;
-- 按照性别分组，分别查询男、女同学的平均分、人数.
-- 要求：分数低于70分的人，不参与分组,分组之后，人数要大于2人
SELECT sex,AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex HAVING COUNT(id) > 2;

SELECT sex 性别,AVG(math) AS 平均分,COUNT(id) 人数 FROM student WHERE math > 70 GROUP BY 性别 HAVING 人数 > 2;

```


#### 7.分页查询

* 语法：`limit 开始的索引,每页显示的条数`
* **公式：开始的索引 = (当前的页码 - 1) * 每页显示的条数***
* `limit` 是一个MySQL的**方言**

```mysql
-- 每页显示3条记录
SELECT * FROM student LIMIT 0,3; -- 第一页
SELECT * FROM student LIMIT 3,3; -- 第二页
SELECT * FROM student LIMIT 6,3; -- 第三页
```


### 7.约束

* 概念：对表中的数据进行限定，保证数据的正确性、有效性和完整性。
* 分类：
    1. 主键约束：primary key
    2. 非空约束：not null
    3. 唯一约束：unique
    4. 外键约束：foreign key

#### 1.非空约束:not null，值不能为null

1. 创建表时添加非空约束

    ```mysql
    CREATE TABLE stu(
        id INT,
        NAME VARCHAR(20) NOT NULL -- name的非空约束
    );
    ```

2. 创建完表后，添加非空约束

    ```mysql
    ALTER TABLE stu MODIFY NAME VARCHAR(20) NOT NULL;
    ```

3. 删除name的非空约束
    ```mysql
    ALTER TABLE stu MODIFY NAME VARCHAR(20);
    ```


#### 2.唯一约束：unique，值不能重复

1. 创建表时，添加唯一约束

    ```mysql
    CREATE TABLE stu(
        id INT,
        phone_number VARCHAR(20) UNIQUE
    );
    ```

2. 在创建表后，添加唯一约束

    ```mysql
    ALTER TABLE stu MODIFY phone_number VARCHAR(20) UNIQUE;
    ```

> 注意：mysql中，唯一约束 限定的列的值可以有多个 null


3. 删除唯一约束(唯一约束删除方式不同)

    ```mysql
    -- 正确删法
    ALTER TABLE stu DROP INDEX phone_number;
    -- 错误删法 
    -- alter table stu modify phone_number varchar(20);
    ```

#### 3.主键约束:primary key

> 注意事项：
> 1. 主键约束的含义：非空且唯一
> 2. 一张表中只能有一个字段为主键
> 3. 主键就是表中记录的唯一标识


1. 创建表时添加主键约束

    ```mysql
    CREATE TABLE stu(
        id INT PRIMARY KEY,
        NAME VARCHAR(20)
    );
    ```

2. 创建表后，添加主键

    ```mysql
    ALTER TABLE stu MODIFY id INT PRIMARY KEY;
      
    ```

3. 删除主键

    ```mysql
     -- (主键只有一个，不需要指定字段，直接删除主键即可)
    -- 正确写法
    ALTER TABLE stu DROP PRIMARY KEY;
    -- 错误写法 
    -- alter table stu modify id int primary key;
    ```

4. 自动增长
    1. 概念：如果某一列是数值类型的，使用 `auto_increment`可以来完成值的自动增长
    2. 在创建表时，添加主键约束，并且完成主键自增长
    ```mysql
    CREATE TABLE stu(
        id INT PRIMARY KEY AUTO_INCREMENT,
        NAME VARCHAR(20) 
    );
    ```
    3. 删除自动增长

    ```mysql
    ALTER TABLE stu MODIFY id INT;
    ```

    4. 添加自动增长

    ```mysql
    ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;
    ```

#### 4. 外键约束:foreign key

* 表与表产生关系，从而保证数据的正确性。

1. 在创建表时，添加外键
    * 语法：
        ```mysql
        create table 表名(
            ...
            外键列,
            constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称)
        );
        ```


2. 删除外键

    ```mysql
    alter table 表名 drop foreign key 外键名称;
    ```

3. 创建表之后，添加外键

    ```mysql
    alter table 表名 add constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称);
    ```

4. **级联操作**
   
    1. 添加级联操作
        * 语法：
        ```mysql
        alter table 表名 add constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称) on update cascade on delete casacade;
        ```
    2. 分类：
        1. 级联更新:on update cascade
        2. 级联删除:on delete casacade

## 4.数据库设计

### 1.多表之间的关系

1. **分类**：
    1. 一对一（了解）：
        * 如：人和身份证
        * 分析：一个人只有一个身份证，一个身份证只能对应一个人 
    2. 一对多（多对一）
        * 如：部门和员工
        * 分析：一个部门有多个员工，一个员工只能对应一个部门

    3. 多对多
        * 如：学生和课程
        * 分析：一个学生可以选择很多门课程，一个课程也可以被很多学生选择

2. **实现关系**：

    1. 一对一（了解）：
        * 如：人和身份证
        * 实现方式：可以在任意一方添加***唯一外键***指向另一方的主键。
            ![多表之间的关系_一对一](https://ae01.alicdn.com/kf/H1ef9b96749694a2297c975f6587271b7U.jpg)
    2. 一对多（多对一）
        * 如：部门和员工
        *  实现方式：在**多**的一方建立**外键**，指向**一**的一方的**主键**。
            ![多表之间的关系_一对多](https://ae01.alicdn.com/kf/H04597d831c5e4d24bdfd310488673f744.jpg)
    3. 多对多
        * 如：学生和课程
        * 实现方式：**多对多关系实现，需要借助第三张中间表。中间表至少包含两个字段，这两张表作为第三张表的外键，分别指向两张表的主键。**
            ![多表之间的关系_多对多](https://ae01.alicdn.com/kf/Hb40a6ac5a6114dfda6b8fcc7edddf363C.jpg)


#### 案例:

```mysql
-- 创建旅游线路分类表 tab_category
-- cid 旅游线路分类主键，自动增长
-- cname 旅游线路分类名称非空，唯一，字符串 100
CREATE TABLE tab_category(
	cid INT PRIMARY KEY AUTO_INCREMENT,
	cname VARCHAR(100) NOT NULL UNIQUE
);
        
-- 创建旅游线路表 tab_route
/*
rid 旅游线路主键，自动增长
rname 旅游线路名称非空，唯一，字符串 100
price 价格
rdate 上架时间，日期类型
cid 外键，所属分类
*/
CREATE TABLE tab_route(
	rid INT PRIMARY KEY AUTO_INCREMENT,
	rname VARCHAR(100) NOT NULL UNIQUE,
	price DOUBLE,
	rdate DATE,
	cid INT,
	FOREIGN KEY (cid) REFERENCES tab_category(cid)
);
        
/*
创建用户表 tab_user
uid 用户主键，自增长
username 用户名长度 100，唯一，非空
password 密码长度 30，非空
name 真实姓名长度 100
birthday 生日
sex 性别，定长字符串 1
telephone 手机号，字符串 11
email 邮箱，字符串长度 100
*/
CREATE TABLE tab_user (
	uid INT PRIMARY KEY AUTO_INCREMENT,
	username VARCHAR(100) UNIQUE NOT NULL,
	PASSWORD VARCHAR(30) NOT NULL,
	NAME VARCHAR(100),
	birthday DATE,
	sex CHAR(1) DEFAULT '男',
	telephone VARCHAR(11),
	email VARCHAR(100)
);

/*
创建收藏表 tab_favorite
rid 旅游线路 id，外键
date 收藏时间
uid 用户 id，外键
rid 和 uid 不能重复，设置复合主键，同一个用户不能收藏同一个线路两次
*/
CREATE TABLE tab_favorite (
	rid INT, -- 线路id
	DATE DATETIME,
	uid INT, -- 用户id
	-- 创建复合主键
	PRIMARY KEY(rid,uid), -- 联合主键
	FOREIGN KEY (rid) REFERENCES tab_route(rid),
	FOREIGN KEY(uid) REFERENCES tab_user(uid)
);
```

### 2.【数据库设计的范式】

* **概念：设计数据库时，需要遵循一定的规范。要遵循后边的范式要求，必须先遵循前边的所有范式要求。**
  
    * 设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范，越高的范式数据库冗余越小。
* 目前关系数据库有六种范式：第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，又称完美范式）。
  
* 分类：
    1. **第一范式(1NF)**:数据库表的每一列都是不可分割的**原子数据项**
    2. **第二范式(2NF)**:在1NF的基础上，非码属性必须完全依赖于码（在1NF基础上消除非主属性对主码的部分函数依赖）
        * **几个概念**：
            1. **函数依赖**：A --> B,如果通过A属性(属性组)的值，可以确定唯一B属性的值。则称B依赖于A 或 A被B依赖。
                例如：学号 --> 姓名。(学号，课程名称) --> 分数
            2. **完全函数依赖**：A --> B，如果A是一个**属性组**，则B属性值的确定需要依赖于A属性组中所有的属性值。
            例如：(学号，课程名称) --> 分数
            3. **部分函数依赖**：A --> B，如果A是一个**属性组**，则B属性值的确定**只**需要依赖于A属性组中的**某一些值**即可。
            例如：(学号，课程名称) --> 姓名
            4. **传递函数依赖**：A --> B,B --> C，如果通过A属性(属性组)的值，可以确定唯一B属性的值，再通过B属性(属性组)的值k可以确定唯一C属性的值，则称C函数依赖于A函数。
            5. **码**：如果在一张表中，一个属性或属性组，被其他属性所完全依赖，则称这个属性(属性组)为该表的码。
            例如：该表码为：(学号，课程名称)
            * 主属性：码属性组中的所有属性
            * 非主属性：除过码属性组的属性

    3. **第三范式(3NF)**:在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）




## 5.数据库的备份和还原

1. 命令行：
    * 语法：
        * 备份：mysqldump -u用户名 -p密码 数据库名称 > 保存的路径
        * 还原：
            1. 登录数据库
            2. 创建数据库
            3. 使用数据库
            4. 执行文件。`source` 文件路径

2. 图形化工具：SQLyog


## 6.多表查询

### 1.查询语法
~~~mysql
select 
    字段列表
from
    表名列表
where
    条件列表
group by
    分组列表
having
    分组之后的条件
ordered by
    排序
limit
    分页限定
~~~
### 2.准备sql

~~~mysql
-- 创建部门表
CREATE TABLE dept(
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(20)
);

INSERT INTO dept(NAME) VALUES("开发部"),("市场部"),("财务部");

-- 创建员工表
CREATE TABLE emp(
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(10),
    gender CHAR(1),
    salary DOUBLE,
    join_date DATE,
    dept_id INT,
    FOREIGN KEY(dept_id) REFERENCES dept(id) -- 外键，关联部门表(部门表的主键)	
);

INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('孙悟空','男',7200,'2013-02-24',1);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('猪八戒','男',3600,'2010-12-02',2);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('唐僧','男',9000,'2008-08-08',2);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('白骨精','女',5000,'2015-10-07',3);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('蜘蛛精','女',4500,'2011-03-14',1);
~~~

* 笛卡尔积：
    * 有两个集合A、B，取这两个集合的所有组成情况。
    * 要完成多表查询，需要消除**无用的数据**

### 3.多表查询的分类
#### 1.内链接查询

1. 隐式内链接：使用`where`条件消除无用数据
    * 例如：
    ```mysql
    -- 多表查询
    SELECT * FROM dept,emp; -- 笛卡尔积

    -- 查询所有员工信息和对应部门的信息
    SELECT * FROM emp,dept WHERE emp.`dept_id`= dept.`id`;
    -- 查询员工表的名称，性别。部门表的名称
    SELECT emp.`name`,emp.`gender`,dept.`name`FROM emp,dept WHERE emp.`dept_id`=dept.`id`;

    -- 正式的sql写法
    SELECT 
        t1.`name`, -- 员工表的姓名
        t1.`gender`, -- 员工表的性别
        t2.`name` -- 部门表的名称
    FROM
        emp t1,
        dept t2 -- 起别名
    WHERE
        t1.`dept_id`=t2.`id`;
    ```
2. 显式内链接
    * 语法：`select 字段列表 from 表名1 [inner] join 表名2 on 条件`
    * 例如：
      
    ```mysql
    -- 显式内链接
    SELECT 
        * 
    FROM 
        emp
    INNER JOIN
        dept 
    ON 
        emp.`dept_id`=dept.`id`;
    ```

3. **内链接查询需要明白的**：
        1. 从那些表中查询数据？
            2. 条件是什么？
            3. 查询那些字段？
#### 2.外连接查询

1. **左外连接**：

    * 语法：`select 字段列表 from 表1 left [outer] join 表2 on 条件;`
* 含义：查询的是左表所有数据以及其交集部分。

* 例子:
    ```mysql
    /*查询所有员工信息，如果员工有部门，则查询部门名称，没有部门，则不显示部门名称*/
        SELECT
            t1.*,
            t2.`name` 
        FROM
            emp t1
        LEFT JOIN 
            dept t2 
        ON 
            t1.`dept_id`= t2.`id;
    ```

2. 右外连接：
    * 语法：`select 字段列表 from 表1 right [outer] join 表2 on 条件;`
    * 含义：查询的是右表所有数据以及其交集部分。
    * 例子:

    ```mysql
    /*查询所有员工信息，如果员工有部门，则查询部门名称，没有部门，则不显示部门名称*/
    SELECT
        t1.*,
t2.`name` 
    FROM
emp t1
    RIGHT JOIN 
dept t2 
    ON 
        t1.`dept_id`= t2.`id`;
    ```

#### 3.子查询(3种情况)
* 概念：查询中嵌套查询，称嵌套查询 或 子查询。
* 示例：
    ```mysql
    -- 1.查询工资最高的员工信息
    SELECT MAX(salary) FROM emp;
    -- 2.查询员工信息，并且工资等于最高工资
    SELECT * FROM emp WHERE emp.`salary`=9000;

    -- 一条sql就可以完成 子查询(嵌套查询)
    SELECT * FROM emp WHERE emp.`salary`=(SELECT MAX(salary) FROM emp);
    ```

* 子查询不同情况
    1. 子查询的结果是**单行单列**
        * 子查询可以作为条件，使用运算符`>、<、>=、<=、=`去判断。
        ```mysql
        -- 查询员工工资小于平均工资的人
        SELECT * FROM emp WHERE emp.`salary` < (SELECT AVG(salary) FROM emp);
        ```

    2. 子查询的结果是**多行单列**
        * 子查询可以作为条件，使用运算符`in`去判断
        ```mysql
        -- 查询'财务部' 和 '市场部'的所有员工信息
        SELECT id FROM dept WHERE NAME IN('财务部','市场部');
        SELECT * FROM emp WHERE emp.`dept_id` IN (2,3);
-- 一步到位
        SELECT * FROM emp WHERE emp.`dept_id` IN (SELECT id FROM dept WHERE NAME IN('财务部','市场部'));
        ```
    3. 子查询的结果是**多行多列**
        * 子查询可以作为一张**虚拟表**参与查询

        ```mysql
        -- 查询员工入职日期是2011-11-11日之后的员工信息和部门信息
        -- 子查询
        SELECT 
            *
        FROM
            dept t1,
            (SELECT * FROM emp WHERE emp.`join_date` > '2011-11-11') t2
        WHERE
            t1.`id` = t2.dept_id;
        -- 普通查询
        SELECT
            * 
        FROM
            dept t1,
            emp t2
        WHERE
            t1.`id` = t2.`dept_id`
        AND 
            t2.`join_date` > "2011-11-11";
        ```

### 4.多表查询练习

- sql准备

~~~mysql
-- 部门表
CREATE TABLE dept(
	id INT PRIMARY KEY, -- 部门id
	dname VARCHAR(50), -- 部门名称
	loc VARCHAR(50) -- 部门所在地
);

-- 添加4个部门
INSERT INTO dept(id,dname,loc) VALUES
(10,'教研部','北京'),
(20,'学工部','上海'),
(30,'销售部','广州'),
(40,'财务部','深圳');

-- 职务表，职务名称，职务描述
CREATE TABLE job(
	id INT PRIMARY KEY,
	jname VARCHAR(20),
	description VARCHAR(50)
);

-- 添加4个职务
INSERT INTO job(id,jname,description) VALUES
(1,'董事长','管理整个公司，接单'),
(2,'经理','管理部门员工'),
(3,'销售员','向客人推销产品'),
(4,'文员','使用办公软件');

-- 员工表
CREATE TABLE emp(
	id INT PRIMARY KEY, -- 员工id
	ename VARCHAR(50), -- 员工姓名
	job_id INT, -- 职务id
	mgr INT, -- 上级领导
	joindate DATE, -- 入职日期
	salary DECIMAL(7,2), -- 工资
	bonus DECIMAL(7,2), -- 奖金
	dept_id INT, -- 所在部门id
	CONSTRAINT emp_jobid_ref_job_id_fk FOREIGN KEY (job_id) REFERENCES job(id),
	CONSTRAINT emp_deptid_ref_dept_id_fk FOREIGN KEY (dept_id) REFERENCES dept(id) 
);

-- 添加员工
INSERT INTO emp(id,ename,job_id,mgr,joindate,salary,bonus,dept_id) VALUES 
(1001,'孙悟空',4,1004,'2000-12-17','8000.00',NULL,20),
(1002,'卢俊义',3,1006,'2001-02-20','16000.00','3000.00',30),
(1003,'林冲',3,1006,'2001-02-22','12500.00','5000.00',30),
(1004,'唐僧',2,1009,'2001-04-02','29750.00',NULL,20),
(1005,'李逵',4,1006,'2001-09-28','12500.00','14000.00',30),
(1006,'宋江',2,1009,'2001-05-01','28500.00',NULL,30),
(1007,'刘备',2,1009,'2001-09-01','24500.00',NULL,10),
(1008,'猪八戒',4,1004,'2007-04-19','30000.00',NULL,20),
(1009,'罗贯中',1,NULL,'2001-11-17','50000.00',NULL,10),
(1010,'吴用',3,1006,'2001-09-08','15000.00','0.00',30),
(1011,'沙僧',4,1004,'2007-05-23','11000.00',NULL,20),
(1012,'李逵',4,1006,'2001-12-03','9500.00',NULL,30),
(1013,'小白龙',4,1004,'2001-12-03','30000.00',NULL,20),
(1014,'关羽',4,1007,'2002-01-23','13000.00',NULL,10);

-- 工资等级表
CREATE TABLE salarygrade(
	grade INT PRIMARY KEY, -- 级别
	losalary INT, -- 最低工资
	hisalary INT -- 最高工资
);
DROP TABLE salarygrade;
-- 添加5个工资等级
INSERT INTO salarygrade(grade,losalary,hisalary) VALUES
(1,7000,12000),
(2,12010,14000),
(3,14010,20000),
(4,20010,30000),
(5,30010,99990);
~~~

- 需求

~~~mysql
-- 需求：
-- 1.查询所有员工信息。查询员工编号，员工姓名，工资，职务名称，职务描述
/*
分析：
	1.员工编号，员工姓名，工资 需要查询 emp表 职务名称，职务描述 查询job表
	2.查询条件：emp.job_id = dept.id
*/

SELECT 
	t1.`id`, -- 员工id
	t1.`ename`, -- 员工姓名
	t1.`salary`, -- 员工工资
	t2.`jname`, -- 职务名称
	t2.`description` -- 职务描述
FROM
	emp t1,
	job t2
WHERE 
	t1.job_id = t2.id;
	
-- 2.查询所有员工信息。查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置
/*
分析：
	1.员工：编号，姓名，工资 --> emp; 职务：名称，描述 --> job; 部门：名称，位置 --> dept;
	2.条件：emp.job_id = job.id and emp.dept_id = dept.id
*/
SELECT 
	t1.`id`, -- 员工id
	t1.`ename`, -- 员工姓名
	t1.`salary`, -- 员工工资
	t2.`jname`, -- 职务名称
	t2.`description`, -- 职务描述
	t3.`dname`, -- 部门名称
	t3.`loc` -- 部门位置
FROM
	emp t1,
	job t2,
	dept t3
WHERE 
	t1.job_id = t2.id AND t1.`dept_id` = t3.`id`;

-- 3.查询员工姓名，工资，工资等级
/*
分析：
	1.emp:员工姓名，工资 salarygrade:工资等级
	2.条件：emp.salary between salarygrade.hisalary and salarygrade.hisalary;
*/

SELECT 
	t1.`ename`, -- 员工姓名
	t1.`salary`, -- 员工工资
	t2.`grade` -- 工资等级
FROM 
	emp t1,
	salarygrade t2
WHERE 
	t1.`salary` BETWEEN t2.`losalary` AND t2.`hisalary`;

-- 4.查询员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级
SELECT 
	t1.`ename`, -- 员工姓名
	t1.`salary`, -- 员工工资
	t2.`jname`, -- 职务名称
	t2.`description`, -- 职务描述
	t3.`dname`, -- 部门名称
	t3.`loc`, -- 部门位置
	t4.`grade` -- 工资等级
FROM 
	emp t1,
	job t2,
	dept t3,
	salarygrade t4
WHERE
	t1.`job_id` = t2.`id` 
	AND t1.`dept_id` = t3.`id` 
	AND t1.`salary` BETWEEN t4.`losalary` AND t4.`hisalary`;
	

-- 5.查询出部门编号、部门名称、部门位置、部门人数
/*
分析：
	1.部门编号，部门名称，部们位置 job 部门人数 emp
	2.使用分组查询，按照emp.dept_id完成分组，查询count(id)
	3.使用子查询将第2步的查询结果和dept表进行关联查询
*/

SELECT 
	t1.`id`, -- 部门编号
	t1.`dname`, -- 部门名称
	t1.`loc`, -- 部门位置
	t2.total -- 部门人数
FROM
	dept t1,
	(SELECT 
		dept_id, 
		COUNT(id) total
	FROM
		emp
	GROUP BY
		emp.`dept_id`) t2
WHERE
	t1.`id` = t2.dept_id;

-- 6.查询所有员工的姓名及其直接上级的姓名,没有领导的员工也需要查询
/*
分析：
	1.姓名：emp 直接上级的姓名：emp
	emp表的 id 和 mgr 是自关联的
	2.条件：emp.id = emp.mgr
	3.查询左表的所有数据以及其交集数据，使用左外连接查询
*/

/*
-- 查询了一半
select 
	t1.`ename`,
	t1.`mgr`,
	t2.`id`,
	t2.`ename`
from 
	emp t1,
	emp t2
where 
	t1.`mgr` = t2.`id`
*/
-- 完整查询
SELECT 
	t1.`ename`, -- 员工姓名
	t1.`mgr`, -- 员工的管理者id
	t2.`id`, -- 管理者id
	t2.`ename` -- 管理者姓名
FROM
	emp t1
LEFT JOIN
	emp t2
ON 
	t1.`mgr` = t2.`id`
~~~


## 7.事务

### 1.事务的基本介绍

1. 概念：
   
    - 如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败。
2. 操作：
    1. 开启事务:`start transaction;`
    2. 回滚:`rollback;`
    3. 提交:`commit;`
    - 例子：
    ```mysql
    CREATE TABLE account(
        id INT PRIMARY KEY AUTO_INCREMENT,
        NAME VARCHAR(20),
        banlance DOUBLE
    );
    -- 添加数据
    INSERT INTO account(id,NAME,banlance) VALUES(1,'zhangsan',1000),(2,'lisi',1000);

    -- 张三给李四转账 500元
    -- 0. 开启事务
    START TRANSACTION;
    -- 1. 张三账户 -500
    UPDATE account SET banlance = banlance - 500 WHERE NAME = 'zhangsan';
    -- 2. 李四账户 +500
UPDATE account SET banlance = banlance + 500 WHERE NAME = 'lisi';
    -- 发现执行没有问题
    COMMIT;

    -- 发现有异常,回滚事务
ROLLBACK;
    UPDATE account SET banlance = 1000;
    SELECT * FROM account;
    ```
    4. MySQL数据库中事务默认自动提交
        * 事务提交的两种方式：
            * 自动提交：
                * `mysql` 就是自动提交的
                * 一条DML(增删改)语句就会自动提交一次事务
            * 手动提交：
                * `oracle` 数据库默认是手动提交事务
                * 需要先开启事务，再提交
        * 修改事务的默认提交方式：
            * 查看事务的默认提交方式：`select @@autocommit; -- 1 代表自动提交 0 代表手动提交`
            * 修改默认提交方式： `set @@autocommit = 0;`


### 2.事务的四大特征(重点)

1. **原子性**：是不可分割的最小操作单位，要么同时成功，要么同时失败。
2. **持久性**：当事务提交或回滚后，数据库会持久化保存数据。
3. **隔离性**：多个事务之间。相互独立。
4. **一致性**：事务操作前后，数据总量不变。


### 3.事务的隔离级别(了解)

* 概念：多个事务之间隔离的，相互独立的。但是如果多个事务操作同一批数据，则会*引发一些问题*，设置不同的隔离级别就可以解决这些问题。
* 存在问题：
    1. **脏读**：一个事务，读取到另一个事务中没有提交的数据。
    2. **不可重复读(虚读)**：在同一个事务中，两次读取到的数据不一样。
    3. **幻读**：一个事务操作(DML)数据表中所有记录，另一个事务添加了一条数据，但第一个事务查询不到自己的修改。

* 隔离界别：
    1. `read uncommitted`：读未提交的
        * 产生的问题：脏读、不可重复读、幻读
    2. `read committed`：读已提交(Oracle默认)
        * 产生的问题：不可重复读、幻读
    3. `repeatable read`：可重复读(MySQl默认)
        * 产生的问题：幻读
    4. `serializable`：串行化
        * 可以解决所有问题

    > 注意事项：
    > 隔离级别从小到大，安全性越来越高，但是效率越来越低
    > 数据库查询隔离级别语法：
    > 	`select @@tx_isolation;`
    > 数据库设置隔离级别语法：
    > 	`set global transaction isolation level 级别字符串`
    > 	例如：`SET GLOBAL TRANSACTION ISOLATION READ UNCOMMITTED;`
    > 	

    * 例子：
    ```mysql
    -- 转账操作
    UPDATE account SET banlance = banlance - 500 WHERE id = 1;
UPDATE account SET banlance = banlance + 500 WHERE id = 2;
    UPDATE account SET banlance = 1000;
SELECT * FROM account;
    ```


## 8.DCL：管理用户、授权

### 1.管理用户

1. 添加用户：

    ```mysql
    create user '用户名'@'主机名' identified by '密码';
    ```
2. 删除用户：

    ```mysql
    drop user '用户名'@'主机名';
    ```
3. 修改用户密码：
    ```mysql
    -- 第1种方式 ???
    update user password = password('新密码') where user = '用户名';
-- 第2种方式
    SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');
SET PASSWORD FOR 'lisi'@'localhost' = PASSWORD('abc');
    ```
* MySQL中忘记了root用户的密码？
    ~~~mysql
    /*
        1.cmd --> net stop mysql 停止mysql服务(需要使用管理员方式运行)
        2.使用无验证方式启动mysql服务：mysqld --skip-grant-tables
        3.打开新的cmd窗口，直接输入mysql命令，敲回车。就可以登录成功。
        4.use mysql;
        5.set password for '用户名'@'主机' = password('新密码');
        6.关闭两个窗口
        7.打开任务管理器，手动结束mysqld.exe 的进程
        8.启动mysql服务
        9.使用新密码登录
    */
    ~~~

4. 查询用户：
    ```mysql
    -- 1. 切换到mysql数据库
    use mysql;
    -- 2. 查询 user表
    select * from user;
    -- 通配符：`%` 表示可以在任意的主机使用用户登录
    ```

### 2.权限管理

1. 查询权限：
    ```mysql
    show grants for '用户名'@'主机名';
    show grants for 'lisi'@'%';
    ```

2. 授予权限：
	```mysql
	grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
	-- 给zhangsan 用户授予所有权限，在任意数据库任意表上
	grant all on *.* to 'zhangsan'@'%';
   ```
3. 撤销权限：
    ```mysql
    revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
    revoke update on db3.account from 'lisi'@'%';
    ```
    
    
