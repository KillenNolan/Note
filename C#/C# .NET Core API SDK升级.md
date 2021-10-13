# C# .NET Core API SDK升级

## 项目背景

​		创建.NET Core 3.1项目报错 ***"目前的 .NET SDK 不支援以 .NET Core 3.1 作為目標。請以 .NET Core 2.1 或更低版本作為目標，或是使用支援 .NET Core 3.1 的 .NET SDK 版本"*** ，后去[***官网***](https://dotnet.microsoft.com/download/dotnet-core)下载安装依旧报同样错误。于是使用3以上特有命令***dotnet new globaljson --sdk-version 3.1.404***生成global.json文档并指定.NET Core SDK 的版本（对当前解决方案有效），报错***"3.1.404 版的 .NET Core SDK 需要最低 16.7.0 版的 MSBuild。目前可用版本的 MSBuild 為 16.5.0.12403。請將 global.json 中指定的 .NET Core SDK 變更為需要目前可用 MSBuild 版本的較舊版本"***。

## 查看MSBuild版本

​		[***官网MSBuild版本变更***](https://docs.microsoft.com/zh-tw/visualstudio/msbuild/updating-an-existing-application?view=vs-2019)==>直接升级VS吧，更快点

​		说明(H)==>关于Microsoft Visual Studio

![MSBuild版本](D:\TEMP\PNG\NetCoreSDK3.PNG)

[***版本资讯***](https://docs.microsoft.com/zh-tw/visualstudio/releases/2019/release-notes#16.8.3)

[***百度升级文章***](https://jingyan.baidu.com/article/7908e85c516e74ee481ad28b.html)

说明(H)==>查看是否有更新(U)

![升级VS版本](D:\TEMP\PNG\NetCoreSDK5.PNG)



## 查看SDK版本

​		在我的电脑中本来有3.1.201是安装VS的时候自己装上的，所以新建项目的时候可以选择.NET Core 3.1，但是建完就会报***"目前的 .NET SDK 不支援以 .NET Core 3.1 作為目標。請以 .NET Core 2.1 或更低版本作為目標，或是使用支援 .NET Core 3.1 的 .NET SDK 版本"*** ，且项目属性中的应用程式也没有其他3的版本可以选。3.1.404是我去官网下载安装的，但是***3.1.404 版的 .NET Core SDK 需要最低 16.7.0 版的 MSBuild***，也就是说，你要升级你的VS的MSBuild。

![安装SDK版本](D:\TEMP\PNG\NetCoreSDK2.PNG)

![可选SDK版本](D:\TEMP\PNG\NetCoreSDK4.PNG)

```c#
//輸入 'get-help NuGet' 可查看所有可用的 NuGet 命令。

PM> dotnet --list-sdks
2.1.403 [C:\Program Files\dotnet\sdk]
3.1.404 [C:\Program Files\dotnet\sdk]
PM> dotnet --list-sdks
2.1.403 [C:\Program Files\dotnet\sdk]

//列出電腦上安裝的SDK 和執行階段版本
dotnet --list-sdks

//查看電腦上安裝的執行時間清單
dotnet --list-runtimes 
```

