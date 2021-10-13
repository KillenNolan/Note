# C# .NET Core API 注入Swagger

## 环境

1. Windows 10
2. Visual Studio 2019(2017就有可以集中发布到publish目录的功能了吧)
3. C#
4. .NET Core 可跨平台发布代码,超级奈斯
5. NuGet 套件管理dll 
6. 将方法封装(据说可以提高效率,就像是我们用的dll那种感觉)
7. Swagger 让接口可视化

## 注入Swagger

​		在我们的专案新增成功后，看下专案的目录，**Program.cs**是这个项目的入口，看到**Main函数**了吗？它就是入口，百分之九十的开发语言应该都是由Main函数作为入口的。(至于它为何是入口，这个没探索过，自己琢磨)。

​		在Program中，最终会使用我们的**Startup.cs**，而我们的主角**Swagger**就是在这里注入的哦！

### 原始的Program和Startup

```c#
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace RMS
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>();
    }
}
```

```c#
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;

namespace RMS
{
    public class Startup
    {
        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        public IConfiguration Configuration { get; }

        // This method gets called by the runtime. Use this method to add services to the container.
        // 注入服务：我只是一个翻译的，我什么都不知道
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        // 配置管道：我只是一个翻译的，我什么都不知道
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseHsts();
            }

            app.UseHttpsRedirection();
            app.UseMvc();
        }
    }
}

```

### 先使用NuGet管理套件下载Swagger需要的依赖dll

​		**项目--》右击--》管理NuGet套件**

![NuGet套件管理](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209100439842.png)

#### 现有的套件，是安装.NET Core时已有的

![现有套件](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209101515253.png)

简单来说就是我 目前的dll 跟 安装的dll需要的dll冲突了，一般是版本冲突，说白了，就是一个靠一个，但我有的跟它要靠的冲突了。他需要我升级我的dll到至少5.2.6。

![安装失败](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209101845695.png)

下载套件的时候，看看它的**描述、相依性**之类的，可以知道是否是自己需要的。

![相依性](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209102615422.png)

### 安装成功

![安装成功](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209104100559.png)

![安装成功提示](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209104134346.png)

### 在Startup类中注入服务

```c#
// ConfigureServices 方法

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);

    // 添加
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v4",
            new Info
            {
                Version = "v4",
                Title = "RMS",
                Description = "ASP.NET Core Web API",
            });
        var basePath = AppContext.BaseDirectory;
        var xmlPath = Path.Combine(basePath, "RMS.xml");
        c.IncludeXmlComments(xmlPath, true);

    });
}
```

```c#
// Configure方法 

app.UseSwagger();

// loggerFactory.AddNLog();
// env.ConfigureNLog("NLog.config");
// Enable middleware to serve swagger-ui (HTML, JS, CSS, etc.), 
// specifying the Swagger JSON endpoint.
// 图形化
app.UseSwaggerUI(c =>
{
    c.SwaggerEndpoint("/swagger/v4/swagger.json", "RMSApi V4");
});
```



### F5跑起来

#### 问题一：没找到RMS.xml

![没找到RMS.xml](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209104650550.png)

​		**项目--》右击--》属性--》建置--》输出--》勾选XML文件档案** ===>勾选后自动生成路径

![勾选](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209105506059.png)

#### 问题二：咋还是原来的丑界面呢

​		将url **https://localhost:44372/api/values** 换成 https://localhost:44372/swagger，这个应该是可以在自己的设定文件里设定，看后面能不能找到，能找到就设定下。

![Swagger界面](C:\Users\F1331020\AppData\Roaming\Typora\typora-user-images\image-20201209104832610.png)

项目下的文件launchSettings.json，将launchUrl改为swagger，之前是默认的 api/values，修改后按F5就不用修改url了。

```c#
{
  "$schema": "http://json.schemastore.org/launchsettings.json",
  "iisSettings": {
    "windowsAuthentication": false, 
    "anonymousAuthentication": true, 
    "iisExpress": {
      "applicationUrl": "http://localhost:51816",
      "sslPort": 44372
    }
  },
  "profiles": { 
    "IIS Express": {//本地跑的时候读这个 IIS Express
      "commandName": "IISExpress",
      "launchBrowser": true,
      "launchUrl": "swagger",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "RMS": {//这个没试过，猜测可能是发布后访问的
      "commandName": "Project",
      "launchBrowser": true,
      "launchUrl": "swagger",
      "applicationUrl": "https://localhost:5001;http://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

