# C# .NET Core API 创建

## 环境

1. Windows 10
2. Visual Studio 2019(2017就有可以集中发布到publish目录的功能了吧)
3. C#
4. .NET Core 可跨平台发布代码,超级奈斯
5. NuGet 套件管理dll 

## 新建空白API项目

### 选择ASP.NET Core Web

![ASP.NET Core Web](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209093229913.png)

### 设定自己的项目名称、路径

![设定自己的项目名称路径](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209093720203.png)

### 选择 .NET Core API

​		**默认的HTTPS最好别勾选，因为我用Postman去测试(因为如果参数是放在Header里的话,Swagger好像不行)我的接口的时候，不成功，所以，看你自己啦**

![.NET Core API](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209093839902.png)

### 专案新增成功的样子

![专案新增成功](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209094658752.png)

### 专案跑起来的样子

​		按F5，让专案跑起来（实际代码在Controller里面）。待会注入Swagger后，就会变好看哦

![跑起来的样子](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209094933048.png)

