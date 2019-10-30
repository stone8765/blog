---
layout:	post
title:	dotnet core 中读取配置信息
author: StoneLi
description: dotnet core 中读取配置信息
catalog: true
tags: [dotnet, dotnetcore]
type: dotnet
---

# 1. 读取配置信息
通过nuget安装基础包 ```Microsoft.Extensions.Configuration``` 这个包提供内存配置.
还可以添加其他的配置管理方式, 如命令行、JSON配置文件，**环境变量**等配置管理的方式
```csharp
    Microsoft.Extensions.Configuration.CommandLine
    Microsoft.Extensions.Configuration.Json
    Microsoft.Extensions.Configuration.EnvironmentVariables
```

# 2. 简单读取
 * 如果添加多种配置方式，**后面的值会覆盖前面的**，
 * 我们可以按照这个优先级： **命令行配置** > **环境变量配置** > **JSON配置文件** > **变量配置**
 * 这样在环境切换时会非常方便
    * 在本机测试可以使用命令行配置或使用JSON配置信息启动程序，
    * 程序发布到Docker中我们可以在**启动Docker容器时通过指定环境变量来改变配置信息**, 也可以进入Docker使用命令行来启动程序。

```csharp
    IDictionary<string, string> defaultSettings = new Dictionary<string, string>()
    {
        { "path","value1"}
    };

    var builder = new ConfigurationBuilder()
        .AddInMemoryCollection(defaultSettings)
        .AddJsonFile("config.json", true, true)
        .AddEnvironmentVariables()
        .AddCommandLine(args);

    IConfiguration config = builder.Build();

    Console.WriteLine($"path: { config["path"] }");
    Console.WriteLine($"section: { config.GetSection("test:section").Value }");
```

# 3. 绑定到对象实例
* 首先创建一个我们的配置类
    ```csharp
        public class MyConfig
        {
            public string Path { get; set; }
        }
    ```
* 然后添加引用 
    ```csharp
    Microsoft.Extensions.DependencyInjection
    Microsoft.Extensions.Options
    Microsoft.Extensions.Options.ConfigurationExtensions
    ```
* 先调用```AddOptions()```将Options服务添加到```ServiceCollection```中，然后配置```MyConfig```绑定到```config```实例
* 然后调用```GetService<IOptions<MyConfig>>()```获取```IOptions<MyConfig>```服务, 再取得它的```Value```就是我们的配置对象了。
    ```csharp
        var builder = new ConfigurationBuilder()
            .AddInMemoryCollection(defaultSettings)
            .AddJsonFile("config.json", true, true)
            .AddEnvironmentVariables()
            .AddCommandLine(args);

        IConfiguration config = builder.Build();

        var serviceProvider = new ServiceCollection()
            .AddOptions().Configure<MyConfig>(config)
            .BuildServiceProvider();

        var myConfigOptions = serviceProvider.GetService<IOptions<MyConfig>>();
        var myConfig = myConfigOptions.Value;

        Console.WriteLine($"path: { myConfig.Path }");
    ```