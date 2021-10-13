# DOS 之MySQL

## 建立连接

```sql
#创建账户 killen 设定密码 killen520
create user 'killen'@'%' identified by  'killen520';
create user 'killen'@'%' identified with mysql_native_password by  'killen520';

#赋予权限，with grant option这个选项表示该用户可以将自己拥有的权限授权给别人
grant all privileges on *.* to 'killen'@'%' with grant option;
grant all privileges on *.* to 'killen'@'10.157.147.134' with grant option;

#改密码&授权超用户，flush privileges 命令本质上的作用是将当前user和privilige表中的用户信息/权限设置从mysql库(MySQL数据库的内置库)中提取到内存里
flush privileges;
```



## 数据库信息

```sql
select version();
--在sql客户端中输入select version();
--在dos下输入如下命令

mysql --version或者mysql -V(注意：大写的V)
```





## 权限

```sql
 SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;
 
 
 
```









## 查询端口

```sql
--使用这个语句进入MySQL操作界面
mysql -u root -p

--使用这个语句进入MySQL操作界面===>可指定进入的数据库
--vsearchDB是我已经创建的DB,若首次进入MySQL可不用写，进入默认的就好
mysql -u root -p vsearchDB

--输入账号root的密码即可进入DB


show databases;
--使用 show databases; 可查看默认的DB 
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+

--显示MySQL使用的端口
show global variables like 'port';
```



## 连接远端的MySQL

```sql
mysql -h 192.168.1.139 -u root -p hangfire
```



## 创建DataBase

```sql
create database hangfire;
```



## 查询有哪些账号

```sql
--查看MYSQL数据库中所有用户 及 有权访问的host

SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;

--查看数据库中具体某个用户的权限

show grants for 'cactiuser'@'%';   
select * from mysql.user where user='cactiuser' \G 


--查看user表结构　需要具体的项可结合表结构来查询
desc mysql.user;


--删除账号
drop user 'killen';
```



## 查询表

```sql
--查看多少个数据库,database后面带"s"
show databases;

--使用该数据库
use "database", 

--查看当前库下有哪些表
show tables ; 

--查看表下面那些列
show columns from city; 

更便捷方式 是 describe city; 
```



## 删除表

```sql
--批量按前缀删除表===>或得删除语句
SELECT CONCAT( 'DROP TABLE ', GROUP_CONCAT(table_name) , ';' ) AS statement FROM information_schema.tables WHERE table_schema = 'vsearchDB' AND table_name LIKE 'hangfire%';


--批量按前缀删除表===>删除语句
DROP TABLE hangfire_aggregatedcounter,hangfire_counter,hangfire_distributedlock,hangfire_hash,hangfire_job,hangfire_jobparameter,hangfire_jobqueue,hangfire_jobstate,hangfire_list,hangfire_server,hangfire_set,hangfire_state;



--Oracle

 SELECT 'drop table '||TABLE_NAME ||';' FROM USER_TABLES WHERE TABLE_NAME LIKE 'HF%';


drop table HF_AGGREGATED_COUNTER;
drop table HF_COUNTER;
drop table HF_DISTRIBUTED_LOCK;
drop table HF_HASH;
drop table HF_JOB_QUEUE;
drop table HF_SET;

 SELECT     TO_CHAR (wm_concat (table_name))  FROM USER_TABLES WHERE TABLE_NAME LIKE 'HF%' ;

```



