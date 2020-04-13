# MySQL 数据库命令行 值得你拥有收藏

- 数据库管理工程师（DBA）：从事管理和维护数据库管理系统的相关工作人员的统称

- MySQL是一个软件，管理着硬盘上的数据，并且进行了很多的优化。

 - 数据定义语言（DDL）：用户定义和管理数据对象（创建数据库，创建数据表，create，drop，alter）

 - 数据操作语言（DML）：用户操作数据库对象中所包含的数据（INSERT, UPDATE,DELETE）

 - 数据查询语言（DQL）：用于查询数据库对象中所包含的数据，能够进行单表查询，连接查询，嵌套查询，以及集合查询等各种复杂程序不同的数据库查询，并将数据返回

 - 数据控制语言（DCL）：管理数据库的语言（管理权限，数据更改，GRANT REVOKE COMMIT ROLLBACK）

 - mysql 数据库修改密码：  set password for 'root'@'localhost' = password('123456');

- Mysql命令查询一个表的记录总数(三种方法)

  - select count(*) from tablename;

  - select count(*) as num from tablename;

  - select count(*) as total from tablename;

###  DDL：数据定义语言：用于定义和管理数据对象    CREATE DROP ALTER

- 创建数据库：CREATE DATABASE [IF NOT EXISTS] lamp;

- 删除数据库：DROP DATABASE [IF EXISTS] lamp;

- 展示数据库：SHOW DATABASES;

- 使用数据库：USE lamp;

- 创建数据表：

    ```mysql
    CREATE TABLE user(

    id INT UNSIGNED NOT NULL PRIMARY KEY AUTO_INCREMENT,

    name VARCHAR(60) NOT NULL UNIQUE,

    sex TINYINT NOT NULL

    );
    ```

- 展示数据表：SHOW TABLES;

- 删除数据表：DROP TABLE user;

- 查看表结构：DESC user;

- 查看表结构：SHOW CREATE TABLE user\G;

- 设置表的字符集为utf8：ALTER TABLE user default charset=utf8;

- 更改表字段：ALTER TABLE user change name username VARCHAR(16) NOT NULL UNIQUE;

- 在字段后面添加一个字段：ALTER TABLE user ADD age TINYINT UNSIGNED NOT NULL AFTER username;

- 在表的第一个字段处添加一个字段：ALTER TABLE user ADD aa INT NOT NULL FIRST;

- 删除某个字段：ALTER TABLE user DROP 字段：

- 添加普通索引：ALTER TABLE user add INDEX index_age(age);

- 删除普通索引：ALTER TABLE user DROP INDEX index_age;

- rename 命令用于修改表名； for example : rename table 原表


- 多字段添加：

```mysql
alter table a_user ADD(

`threeconstantsys` varchar(50) DEFAULT NULL COMMENT '三恒系统',

`landcertificate` varchar(50) DEFAULT NULL COMMENT '土地证',

`buildingplanlicence` varchar(50) DEFAULT NULL COMMENT '建筑工程规划许可证',

`buildingexecutionlicence` varchar(50) DEFAULT NULL COMMENT '建筑工程施工许可证',

`buildingopenlicence` varchar(50) DEFAULT NULL COMMENT '建筑工程开工许可证',

`commodityhousedeallicence` varchar(50) DEFAULT NULL COMMENT '商品房买卖售许可证'

);
```

### DML：数据操作语言：用于操作数据库对象中所包含的数据  INSERT UPDATE DELETE

- 添加数据：insert into stu(name,sex,age,classid)    values('qq','w',21,'lamp81'),

- 修改数据：update 表名 set 字段名=修改值[,字段名=修改值[,....]] [where 条件] [order by 排序] [limit 部分数据]

- 删除数据：delete from 表名 [where 条件] [order by 排序] [limit 部分数据]

### 3. DQL：数据查询语言：SELECT

- 查询语句：select 字段名|* from 表名

```mysql
[ where 搜索条件]

[ group by 分组列名[having 分组后的子条件]]

[ order by 排序列名 [desc降序|asc升序(默认)]]

[ limit m[,n] 获取部分数据（分页） ]
```

- select * from user where username="zhangsan" limit 1;

- 查看数据，将name字段名换成username （其中as关键字可以省略不写）
- select name as username,sex,age from stu;

- 查看所有学生信息，并追加一列（年龄都加5的值），起个别名age2
- select *,age+5 age2 from stu;

- 在查看学生信息时，追加一列。值为beijing，字段名为city
- select *,"beijing" city from stu;

- 将stu表中班级和姓名字段合并到一列输出，起字段名为uname
- mysql> select concat(classid,":",name) as uname from stu;

- 查询数据时去除重复的数据
- mysql> select distinct classid from stu;

- like模糊查询只支持两个通配符: '%'表示任意长度的任意值, '_'表示1位的任意值
- 统计查询（mysql的聚合函数：count() /sum() /max() /min() /avg()

- 统计stu表的数据条数11，年龄最大29，最小19，平均年龄23.7273，年龄总和261
- mysql> select count(*),max(age),min(age),avg(age),sum(age) from stu;

- 按班级分组，统计每个班的平均年龄，并获取平均年龄在23及以上信息。
- mysql> select classid,avg(age) from stu group by classid having avg(age)>=23;

  - 获取年龄最大学生信息
  - mysql> select * from stu where age=(select max(age) from stu);

  - 获取zhangsan的考试信息（因为学生姓名唯一，固获取学生信息为单条）
  - mysql> select * from grade where sid=(select id from stu where name='zhangsan');

  - 获取lamp80期学生的成绩信息（因为学生信息结果为多条，故使用in查询）
  - mysql> select * from grade where sid in(select id from stu where classid='lamp80');

- 查询成绩表中所有数据，要求关联学生表中的姓名
- mysql> select grade.*,stu.name from grade,stu where stu.id=grade.sid;

- 内联查询 inner join（查两表的共同，等价于where）
- mysql> select s.id,s.name,s.classid,g.php,g.mysql from stu s inner join grade g  on s.id=g.sid;

- left join 左联（表示以前面的表为主，后面找不到的数据补null显示）
- mysql> select s.id,s.name,s.classid,g.php,g.mysql from stu s left join grade g  on s.id=g.sid;

- 查询类别id号为2的对应商品信息以及子类对应的所有商品信息。
- select * from goods where typeid in(select id from type where path like '%,2,%' or id=2);

### DCL：数据控制语言：用于管理数据库的语言 GRANT REVOKE COMMIT ROLLBACK

```mysql
1.给指定用户赋予相应的权限：GRANT all ON lamp.* to zhangsan@'%' IDENTIFIED BY '123';

all:所有权限(select,insert)

lamp.*:lamp数据库中的所有的数据表

to 'zhangsan'@'192.168.125.55'

identified by '123456':设置密码

2.刷新生效，否则就要重启MySQL服务才可以：FLUSH PRIVILEGES;

3.移除指定用户的权限：REVOKE INSERT,DELETE ON *.* FROM admin@192.168.254.49 IDENTIFIED BY '123';

4.查看指定用户的权限信息：SHOW GRANTS FOR zhangsan@localhost

5.重设密码：GRANT USAGE ON *.* TO 'zhangsan'@localhost IDENTIFIED BY PASSWORD('123456');     password()

6.删除整个用户的权限：DROP user zhangsan;
```
