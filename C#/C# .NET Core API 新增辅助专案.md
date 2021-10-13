# C# .NET Core API 新增辅助专案

## 准备工作

1. Windows 10
2. Visual Studio 2019(2017就有可以集中发布到publish目录的功能了吧)
3. C# 
4. 将方法封装(据说可以提高效率,就像是我们用的dll那种感觉
5. 新增专案作为我们API的辅助专案(作用类似dll，此处，你也可以在你自己的API专案里建文件夹，但这样据说没有效果，我也不知道是不是真的，只能麻烦点，再新增专案啰)

## 新增专案

​		不想太麻烦的，就自己新建文件夹，自己调用就好，就不用建这么多专案。

​		在新增专案前，先迁移一下之前的项目。之前是D:\WEB_CODE\RMS\，我在此再建了一层目录，此时，xml文件的路径也需要重新勾选。

![迁移专案](D:\TEMP\PNG\NetCoreCase2.PNG)

​		给大家看下完整建好后的文件夹

### 新增IServices专案

### 新增Services专案

### 新增Model专案

![新增Model专案](D:\TEMP\PNG\NetCoreCase1.PNG)

![专案取名](D:\TEMP\PNG\NetCoreCase3.PNG)

### 版本冲突

![.NetCore版本不一致](D:\TEMP\PNG\NetCoreCase4.PNG)

 ![修正版本](D:\TEMP\PNG\NetCoreCase5.PNG)

## 专案引用

​		**具体的引用要看你需要用到那个专案，这是你自己设定的联系。**

 ![引用步骤](D:\TEMP\PNG\NetCoreCase6.PNG)

 ![引用](D:\TEMP\PNG\NetCoreCase7.PNG) 

![继承](D:\TEMP\PNG\NetCoreCase8.PNG)

## 



```c#
using Autofac;
using Autofac.Extensions.DependencyInjection;

namespace RMS
{
    public class Startup
    {
        
        // 为ConfigureServices方法添加新的注入，且将返回类型void改为 IServiceProvider 
        // This method gets called by the runtime. Use this method to add services to the container.
        public IServiceProvider ConfigureServices(IServiceCollection services)
        { 
            //使用Autofac實現IOC
            var containerBuilder = new ContainerBuilder();
            //模塊化注入
            containerBuilder.RegisterModule<HelpTool.AutofacModuleRegister>();
            containerBuilder.Populate(services);
            var container = containerBuilder.Build();
            return new AutofacServiceProvider(container);
        }
    }
}
```

```c#
using Autofac;
using System.Linq;
using System.Reflection;

namespace HelpTool
{
    public class AutofacModuleRegister : Autofac.Module
    {
        protected override void Load(ContainerBuilder builder)
        { 
            //動態註入服務
            builder.RegisterAssemblyTypes(Assembly.Load("IServices"), Assembly.Load("Services"))
              .Where(t => t.Name.EndsWith("Service"))//注入cs文件以Service结尾的
              .AsImplementedInterfaces(); 
        }
    }
}
```

## 方法调用

```
https://localhost:44372/api/Talentpool/TestMethod?test=111
会失败，因为_iTalentpoolService是个null

//需要 動態註入服務 ==> 会用到Autofac组件 ===》 Startup.cs里写
builder.RegisterAssemblyTypes(Assembly.Load("IServices"), Assembly.Load("Services"))
    .Where(t => t.Name.EndsWith("Service"))
    .AsImplementedInterfaces();

```

![继承](D:\TEMP\PNG\NetCoreCase9.PNG)

- 这样的话，大概的框架就是这样了，但效果是不是会快，我就没实践过了
- 还有，之所以目录Services和Iservices是有用处的，为了動態註入服務，后面有时间就写。
- 其实就是想为构造函数传递值，会用到Autofac组件。