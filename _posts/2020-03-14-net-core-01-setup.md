---
layout:	post
title:	【ASP.NET Core 入门】一、启动过程
author: StoneLi
description: ASP.NET Core 启动过程
tags: [dotnet, dotnetcore]
type: dotnet
---

> 本片文章主要讲解ASP.NET Core的启动过程

新建一个asp.net core的项目，修改一下代码：

一、修改```Program.cs```的```CreateHostBuilder```方法
```Csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(configurationBuilder =>
        {
            Console.WriteLine("执行方法：ConfigureHostConfiguration");
        })
        .ConfigureServices(services =>
        {
            Console.WriteLine("执行方法：ConfigureServices");
        })
        .ConfigureLogging(loggingBuilder =>
        {
            Console.WriteLine("执行方法：ConfigureLogging");
        })
        .ConfigureAppConfiguration((hostBuilderContext, configurationBinder) =>
        {
            Console.WriteLine("执行方法：ConfigureAppConfiguration");
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            Console.WriteLine("执行方法：ConfigureWebHostDefaults");
            webBuilder.UseStartup<Startup>();
        });
```

二、修改```Startup.cs```
```Csharp
public Startup(IConfiguration configuration)
{
    Console.WriteLine("执行方法：Startup.Ctor");
    Configuration = configuration;
}

public void ConfigureServices(IServiceCollection services)
{
    Console.WriteLine("执行方法：Startup.ConfigureServices");
    //...
}

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    Console.WriteLine("执行方法：Startup.Configure");
    //...
}

```

执行得到结果
```
执行方法：ConfigureWebHostDefaults
执行方法：ConfigureHostConfiguration
执行方法：ConfigureAppConfiguration
执行方法：ConfigureServices
执行方法：ConfigureLogging
执行方法：Startup.Ctor
执行方法：Startup.ConfigureServices
执行方法：Startup.Configure
```


