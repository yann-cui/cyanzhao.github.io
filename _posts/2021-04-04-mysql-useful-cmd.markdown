---
layout: post
title:  "Mysql 常用命令"
date:   2021-04-04 18:01:50 +0800
categories: 数据库
---

# 1、连接Mysql

 *    格式：

```
mysql -h主机地址 -u用户名 -p用户密码
```

 *    本地

```
  mysql  -utaoyali -ptaoyali
```

 *    远程

```
 mysql  -h192.168.110.110 -utaoyali -ptaoyali
```

> 注意用户名前可以有空格也可以没有空格，但是密码前必须没有空格，否则让你重新输入密码。

# 2、修改密码

 *    格式：

```
mysqladmin -u用户名 -p旧密码 password 新密码
```

 *    给root加个密码ab12

```
mysqladmin -u root -password ab12
```

```
  注：因为开始时root没有密码，所以-p旧密码一项就可以省略了。 如果进入了mysql后想修改密码，就直接用mysql语句就好了：
```

```
  set PASSWORD=PASSWORD("123");注意mysql语句以分号结束
```

 *    再将root的密码改为djg345

```
mysqladmin -u root -p ab12 password djg345
```

# 3.增加新用户

> 注意：和上面不同，下面的因为是MYSQL环境中的命令，所以后面都带一个分号作为命令结束符

格式：

```
grant select on 数据库.* to 用户名@登录主机 identified by “密码”
```

 *    增加一个用户test1密码为abc，让他可以在任何主机上登录，并对所有数据库有查询、插入、修改、删除的权限。首先用root用户连入MYSQL，然后键入以下命令：

```
grant select,insert,update,delete on *.* to [email=test1@”%]test1@”%[/email]” Identified by “abc”;
```

> 但增加的用户是十分危险的，你想如某个人知道test1的密码，那么他就可以在internet上的任何一台电脑上登录你的mysql数据库并对你的数据可以为所欲为了，解决办法见👇。

 *    增加一个用户test2密码为abc,让他只可以在localhost上登录，并可以对数据库mydb进行查询、插入、修改、删除的操作\(localhost指本地主机，即MYSQL数据库所在的那台主机\),这样用户即使用知道test2的密码，他也无法从internet上直接访问数据库，只能通过MYSQL主机上的web页来访问了。

```
grant select,insert,update,delete on mydb.* to [email=test2@localhost]test2@localhost[/email] identified by “”;
```

# 4.数据库操作

 *    创建数据库

```
create database database_name;
```

 *    修改指定数据库中所有varchar类型的表字段的字符集为UTF8，并将排序规则修改为utf8\_general\_ci

```
SELECT CONCAT('ALTER TABLE `', table_name, '` MODIFY `', column_name, '` ', DATA_TYPE, '(', CHARACTER_MAXIMUM_LENGTH, ') CHARACTER SET UTF8 COLLATE utf8_general_ci', (CASE WHEN IS_NULLABLE = 'NO' THEN ' NOT NULL' ELSE '' END), ';')
FROM information_schema.COLUMNS
WHERE TABLE_SCHEMA = 'databaseName'
AND DATA_TYPE = 'varchar'
AND
(
    CHARACTER_SET_NAME != 'utf8'
    OR
    COLLATION_NAME != 'utf8_general_ci'
);
```

 *    修改指定数据库中所有数据表的字符集为UTF8，并将排序规则修改为utf8\_general\_ci

```
SELECT CONCAT('ALTER TABLE ', table_name, ' CONVERT TO CHARACTER SET  utf8 COLLATE utf8_unicode_ci;')
FROM information_schema.TABLES
WHERE TABLE_SCHEMA = 'databaseName'
```

# 5.数据表数据操作

 *    显示所有的数据库

```
show databases;
```

 *    进入dataBase\_name数据库

```
use dataBase_name;
```

 *    显示dataBase\_name数据库的所有表

```
show tables;
```

 *    显示table\_name表的字段信息

```
desc  table_name;
```

 *    创建表

```
create table  MIFit_Image (name char(100), path char(100), count int(10), firstName char(100), firstMD5 char(100), secondName char(100), secondMD5 char(100), thirdName char(100), thirdMD5 char(100));
```

 *    修改表名

```
rename table MIFit_Image to MIFit_Image_New
```

 *    删除表

```
drop table tableName;
```

 *    插入数据

```
insert into MIFit_Image (name, path, count, firstName, firstMD5, secondName, secondMD5, thirdName, thirdMD5) VALUES ('test', 'test', 1, 'name1', 'md1', 'name2', 'md2', 'name3', 'md3');
```

 *    查询表中的数据

```
select * from MIFit_Image;

select * from MIFit_Image where name = 'test';

select * from MyClass order by id limit 0,2;
```

 *    查询数据库中的重复数据 （ | b71edca50989258e68fadcc3cb9bc689 | 2 | ）

```
select firstMD5, count(*) as count from MIFit_Image group by firstMD5 having count > 1;
```

 *    查询重复数据的其它字段的数据

```
select folderName, path, firstMD5 from MIFit_Image where firstMD5 in (select firstMD5 from MIFit_Image group by firstMD5 having count(firstMD5) > 1);
```

 *    更新数据

```
update MIFit_Image set folderName ='Mary' where id=1;
```

 *    删除数据

```
delete from MIFit_Image where folderName = 'test';
```

# 6.数据表字段操作

 *    添加字段 （int 自增 不为null 主键）

```
alter table MIFit_Image add id int auto_increment not null primary key;
```

 *    修改字段的顺序 \(id 放置最前\)

```

ALTER TABLE 表名 MODIFY 字段名1 数据类型 FIRST ｜ AFTER 字段名2;
其中：

字段名1：表示需要修改位置的字段的名称。
数据类型：表示“字段名1”的数据类型。
FIRST：指定位置为表的第一个位置。
AFTER 字段名2：指定“字段名1”插入在“字段名2”之后。

alter table MIFit_Image modify id int first;

alter table MIFit_Image modify id int after name;
```

 *    移除id主键标志

```
alter table MIFit_Image modify id int, drop primary key;
```

 *    修改字段名name为folderName

```
alter table MIFit_Image change name folderName char(100);
```

 *    删除字段

```
alter table testTable drop folderName;
```

 *    加索引

```
 alter table MIFit_Image add index indexName (folderName);
```

 *    删除索引

```
alter table MIFit_Image drop index indexName;
```
