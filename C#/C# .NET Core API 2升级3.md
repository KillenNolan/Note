# C# .NET Core API 2升级3

### 问题一：Swagger升级到5以上

- 报错信息：swagger Some services are not able to be constructed
- 安装package==》Swashbuckle.AspNetCore
- ConfigureServices方法中注入的services.AddSwaggerGen中的new Info改为 new Microsoft.OpenApi.Models.OpenApiInfo
- [參考文献](https://github.com/domaindrivendev/Swashbuckle.AspNetCore/issues/1259)
- **直接.NET Core 5新建专案吧，自带Swagger注入。**

### 问题二：Autofac注入方法改变

- 报错信息："ConfigureServices returning an System.IServiceProvider isn't supported."
- 在Program==》IHostBuilder中添加 .UseServiceProviderFactory(new AutofacServiceProviderFactory()) 
- 在Startup中新增ConfigureContainer
- [参考文献](https://www.cnblogs.com/jellydong/p/13331450.html)

```C#
//Program.cs
using Autofac.Extensions.DependencyInjection;
public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)  		
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            })
    		//为了Autofac
    		.UseServiceProviderFactory(new AutofacServiceProviderFactory()); 
```
```c#
//Startup.cs
using Autofac;
public ILifetimeScope AutofacContainer { get; private set; }
#region ConfigureContainer should be only one
    //// ConfigureContainer is where you can register things directly
    //// with Autofac. This runs after ConfigureServices so the things
    //// here will override registrations made in ConfigureServices.
    //// Don't build the container; that gets done for you by the factory.
    //for castle
    public void ConfigureContainer(ContainerBuilder builder)
    { // 直接用Autofac注册我们自定义的 AutofacModuleRegister类
        builder.RegisterModule(new AutofacModuleRegister());
    }
#endregion
```
### 问题三：调用报错，进不去方法

- 报错信息：Endpoint testMethod contains CORS metadata, but a middleware was not found that supports CORS.
  Configure your application startup by **adding app.UseCors()** inside the call to Configure(..) in the application startup code. The call to app.UseCors() must appear **between app.UseRouting() and app.UseEndpoints**(...).
- 添加app.UseCors();

```c#
//Startup.cs
app.UseRouting();

//i am here 
app.UseCors();

app.UseEndpoints(endpoints =>
{ 
   	endpoints.MapControllers(); 
});
```

### 问题四：返回object数据类型报错--需要用到MVC

- 报错信息：A possible object cycle was detected which is not supported. This can either be due to a cycle or if the object depth is larger than the maximum allowed depth of 32
- 安装package==》 [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/3.0.0) 
- 注入代码==》  services.AddControllers().AddNewtonsoftJson(options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore);
- [参考文献](https://stackoverflow.com/questions/59199593/net-core-3-0-possible-object-cycle-was-detected-which-is-not-supported)

```json
//返回语句，居然正常
public ActionResult<string> Test1(){
    ... 
    return "TEST";
}

//返回语句，datatable有值，但返回报标题的错
public ActionResult<ReturnMessageModel> Test(){
    ...
    returnMessageModel.data=datatable;
    return returnMessageModel;
}
//返回模型
{
  "status":0,
  "code": "string",
  "message": "string",
  "data": {} 
}
```

```c#
//Startup.cs
public void ConfigureServices(IServiceCollection services)
{
   services.AddControllers().AddNewtonsoftJson(options =>
       options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore);
   services.AddSwaggerGen(c =>
   {
       c.SwaggerDoc("v1", new OpenApiInfo { Title = "RMS3", Version = "v1" });
   });
}
```

### 问题五：Oracle连接报错

- 报错信息：Exception has been thrown by the target of an invocation(調用的目標已引發異常).
- 报错信息：cnn.ServerVersion = 'cnn.ServerVersion' threw an exception of type 'System.InvalidOperationException'===》在.NET Core 2.1中也有这个异常，但是可以打开数据库连接，但升级后就不行了。。。





