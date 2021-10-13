# Power Design 导出模型

## 准备

1. Windows 10
2. Power Design下载安装
3. 安装 ODBC 驱动
4. [导出模型实例](https://developer.aliyun.com/article/332161)
5. [导出模型实例](https://blog.csdn.net/u011781521/article/details/78958529)

## ODBC

- 快捷键：Windows+R--》输入 control
- 控制台--》系统和安全--》管理工具--》ODBC 数据源
- [MySQL驱动下载文章](https://developer.aliyun.com/article/332161)
- [MySQL Connector/ODBC 5.1.13 下载地址](http://dev.mysql.com/downloads/connector/odbc/5.1.html[)
- ODBC 加载连接 

**双击ODBC Data Sources (32-bit)/ODBC Data Sources (64-bit)，看哪个资料来源有，就用哪个。**

![系統管理工具](D:\TEMP\PNG\DesignODBC1.PNG)

我上图的3选的是MySQL ODBC 5.3，比较方便。

![配置MySQL](D:\TEMP\PNG\DesignODBC2.PNG)

## 导出模型

### 新建一个物理数据模型（PhysicalDataModel）

- 快捷键：Ctrl+N 
- File--》New Model

![PhysicalDataModel](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209162240158.png)

### 连接数据库

​		我的DB是版本8的，虽然我配的是5的，可能是因为我电脑5和8都安装了ODBC吧！

![选择数据库TEST](D:\TEMP\PNG\DesignODBC3.PNG)

![编辑数据库TEST](D:\TEMP\PNG\DesignPhysical2.PNG)

![成功连接到表](D:\TEMP\PNG\DesignPhysical3.PNG)

![成功连接到表](D:\TEMP\PNG\DesignPhysical4.PNG)

## [导入常见问题](https://mulanos.oschina.net/question/565065_103069#10)

MySQL 5.0 ---> Oracle 11

### 问题一：建表语句

Database-->Generate Database

需要是自己数据库支持的建表语句

### 问题二：national问题

Database-->Edit Current DBMS-->Profile-->Column-->Extended Attributes(Forms-->MySQL)-->National-->勾选Computed

### 问题三：字段双引号问题 

Database-->Edit Current DBMS-->Script-->Format-->EnableDtbsPrefix-->No

### 问题四：datetime,text 

问题：資料類型無效

解决：手动替换为date,varchar(100)

### 问题五：遺漏右括弧(关键字问题)

报错：KEY    TPStatusName (TPStatusName),

解决：删除这一行 

### 问题六：Comment注释

[Comment1](https://codertw.com/%E8%B3%87%E6%96%99%E5%BA%AB/11252/)

[Comment2](https://www.itread01.com/content/1549612836.html)

### 问题七：Name和Code联动

问题：Name和Code联动

解决：在【Tools】下找到【General Options】【Dialog】勾选【Name to Code mirrroring】



