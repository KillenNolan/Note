# C# .NET Core API 实体模型

​		在 ASP.Net FrameWork 中，我们可以通过**安装EntityFramework 实体模型**，来进行模型导入table的实体类。但在.NET Core中用不了，我在网上百度到项目可以通过NuGet命令来下载安装模型。

​		EntityFramework 是微软以 ADO.NET 为基础所发展出来的[物件关联对应](https://zh.wikipedia.org/wiki/對象關係映射) (O/R Mapping) 解決方案 。

## 准备工作

1. Windows 10
2. Visual Studio 2019(2017就有可以集中发布到publish目录的功能了吧)
3. C# 
4. NuGet
5. Oracle
6. .NET Core 3.1（我之前是2.1，居然不支持报错The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.14-servicing-32113'. Update the tools for the latest features and bug fixes.）



## Nuget控制台

![NuGet](D:\TEMP\PNG\NetCoreEntity1.PNG)

## 安装依赖命令

如果不指定**版本**，则安装最新版本，但有可能会与你的项目版本冲突。

>Install-Package Oracle.EntityFrameworkCore	 
>Install-Package Microsoft.EntityFrameworkCore  -version 3.1.8
>Install-Package Microsoft.EntityFrameworkCore.Tools -version 3.1.8
>Install-Package Pomelo.EntityFrameworkCore.MySql	-version 3.1.0
>Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -version 3.1.1

![NuGet控制台](D:\TEMP\PNG\NetCoreEntity2.PNG)

## Oracle模型命令

>Scaffold-DbContext -Connection "User Id=登录名;Password=密码;Data Source=(DESCRIPTION=ADDRESS=PROTOCOL=TCP)HOST=192.168.127.44)(PORT=1521)) (CONNECT_DATA=(SERVER=DEDICATED)SERVICE_NAME=orcl)));"Oracle.EntityFrameworkCore -Force -Project Model -OutputDir ExtModels -Tables hangfire.log

Scaffold-DbContext -Connection "User Id=rmstest;Password=rmstest;Data Source=(DESCRIPTION=ADDRESS=PROTOCOL=TCP)HOST=10.124.131.71)(PORT=1901)) (CONNECT_DATA=(SERVER=DEDICATED)SERVICE_NAME=HRDB)));"Oracle.EntityFrameworkCore -Force -Project Model -OutputDir ExtModels -Tables TB_TP_CONFIG

Scaffold-DbContext "<connection string>" Oracle.EntityFrameworkCore -Schemas <schema> -Tables <tables>

Build started...
Build succeeded.

, computed value: (null)

下载了schema下所有的表。。。



### 导入实体模型报错

Scaffold-DbContext "<connection string>" Oracle.EntityFrameworkCore  -Tables <tables>

#### Build started... Build succeeded.
Unable to find a schema in the database matching the selected schema mes1.
Unable to find a table in the database matching the selected table mes1.c_emp.

这可能是错误29443379/28963848。 Beta 4之后已修复此问题，这意味着它将在生产版本中发布。



#### EntityTypeBuilder<Course>”未包含“ToTable”的定义，并且找不到可接受第一个“EntityTypeBuilder<Course>”类型参数的可访问扩展方法“ToTabl

今天学习ASP.NET Core 3.1，碰到上述错误，是因为没有引用Microsoft.EntityFrameworkCore.Relational这个Nuget包，在Nuget中加入这个引用就可以解决

#### 将实体类JSON化

在实体类变量前加FromBody

[FromBody] ImAddModel imAdd

#### Entity导入注释



 ## 查看oracle数据库的版本

>select * from v$version;<pre name="code" class="plain">

 