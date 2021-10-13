

# Python 701 配置MySQL

## 安装MySQL

1. [下载压缩文件自己配置](https://blog.csdn.net/weixin_42869365/article/details/83472466)
2. [MSI安装](https://clay-atlas.com/blog/2019/11/16/mysql-mysqlworkbench-tutorial-download-install-steps/ )

```python
#方法一的话，要自己进行较多的配置（没弄过）
#方法二的话，其实是下载MSI的安装步骤，下载的话，你找下MSI，同一页面但不是圈红的下载，且安装需要联网
#同：都要进行环境变量的配置 C:\Program Files\MySQL\MySQL Server 8.0\bin
#注意：MySQL Server
```

## 安装MySQL Connector/Python

[下载地址](https://dev.mysql.com/downloads/)

![image-20201127092035301](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\PythonConntor.PNG)

![下载界面](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\pythondownl.PNG)

```python
#在解压的文件目录输入cmd，打开一个终端窗口,执行以下命令
#我不是用的命令，我是下载的Windows下的MSI进行安装的
py -3 setup.py install
```

## 配置MySQL数据库

​		之前我们已经安装好了MySQL，环境变量也进行了配置，那么我们就可以使用mysql命令啦！



1. 查看安装的Mys版本
2. 输入账号密码(在安装的时候有让你创建，有点忘了)
3. 创建database
4. 创建账号赋权限 [**高版本创建账号**](https://blog.csdn.net/shenhonglei1234/article/details/84786443?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromBaidu-1.control&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromBaidu-1.control)

注意：@后面

- **'%' ===> 所有IP可连接**
- **'10.157.11.1' ===> 指定IP可连接**

```mysql
-- 在sql客户端中输入select version();
select version();

-- 在dos下输入如下命令
mysql --version或者mysql -V(注意：大写的V)


mysql -u root -p
-- -u user ==> root

create database vsearchDB;


-- 创建账号一：高版本适用
-- 创建账户 killen 设定密码 killen520 
create user 'killen'@'10.157.11.1%' identified by  'killen520';
create user 'killen'@'%' identified with mysql_native_password by  'killen520'; -- 8.0以上用这个好一点，不然代码连接会出问题

-- 赋予权限，with grant option这个选项表示该用户可以将自己拥有的权限授权给别人
grant all privileges on *.* to 'killen'@'%' with grant option;
grant all privileges on *.* to 'killen'@'10.157.11.1' with grant option;

-- 改密码&授权超用户，flush privileges 命令本质上的作用是将当前user和privilige表中的用户信息/权限设置从mysql库(MySQL数据库的内置库)中提取到内存里
flush privileges;


-- 创建账号二：低版本适用,我是高版本（8.0以上）
grant all on vsearchDB.* to 'killen' identified by  'killen520';


```

![输入账号密码](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201127095353925.png)

## 创建MySQL的表

​		在前面我们已经创建了一个数据库（vsearchDB），我们还可以看到MySQL自己自带的database。

```sql
--使用 show databases; 可查看的DB
mysql>show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| vsearchdb          |
+--------------------+
5 rows in set (0.03 sec)

-- 创建我们需要的表(超简陋的创建) log
mysql> create table log(
    -> id int auto_increment primary key,
    -> ts timestamp default current_timestamp,
    ->  phrase varchar(128) not null,
    -> letters varchar(32) not null,
    -> ip varchar(16) not null,
    -> browser_string varchar(256) not null,
    -> results varchar(64) not null);
```

![image-20201119154121805](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201119154213129.png)

## 查询创建的表

```sql
--查看当前DB下的所有表
mysql> show databases;
-- 查看指定表（log）结构字段
mysql> describe log;
```

![查看当前DB下的所有表](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201207101501699.png)

![查看表log的结构字段](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201119154252054.png)

### 向表中插入数据

```sql
mysql> insert into log(phrase,letters,ip,browser_string,results) values('glaxy','xyz','127.0.0.1','Opera',"{'x','y','z'}");
```

![向表中插入数据](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201207101013883.png)

### 查询表中数据

![查询表中数据](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201207093345268.png)

### 删除表

```sql
--其实就是自己 拼接 删除语句，然后执行拼接出来的语句
--批量按前缀删除表===>获得删除语句
SELECT CONCAT( 'DROP TABLE ', GROUP_CONCAT(table_name) , ';' ) AS statement FROM information_schema.tables WHERE table_schema = 'vsearchDB' AND table_name LIKE 'hangfire%';


--批量按前缀删除表===>删除语句
DROP TABLE hangfire_aggregatedcounter,hangfire_counter,hangfire_distributedlock,hangfire_hash,hangfire_job,hangfire_jobparameter,hangfire_jobqueue,hangfire_jobstate,hangfire_list,hangfire_server,hangfire_set,hangfire_state;

```



