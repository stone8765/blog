---
layout:	post
title:	【ASP.NET Core 入门】二、依赖注入-内置依赖注入框架
author: StoneLi
description: ASP.NET Core 依赖注入
catalog: true
tags: [dotnet, dotnetcore]
type: dotnet
---

> 本片文章主要讲解ASP.NET Core中的内置依赖注入框架

# 为什么要使用依赖注入框架
1. 借助依赖注入框架，我们可以轻松管理类之间的依赖，帮助我们在构建应用时遵 循设计原则，确保代码的可维护性和可扩展性。
2. ASP.NET Core 的整个架构中，依赖注入框架提供了对象创建和生命周期管理的 核心能力，各个组件相互协作，也是由依赖注入框架的能力来实现的。

# 组件包
1. Microsoft.Extensions.DependencyInjection.Abstractions 
2. Microsoft.Extensions.DependencyInjection

# 核心接口与类
1. IServiceCollection 
2. ServiceDescriptor 
3. IServiceProvider 
4. IServiceScope

# 生命周期：ServiceLifetime
1. Singleton 单例模式，生命周期跟app一样从app创建后开始到app结束前销毁
2. Scoped 作用域模式，在某个作用域内同一个service为重复获取，获取到的是同一个实例
3. Transient 瞬时模式，获取到service之后用完立即销毁

# Service的释放
1. 实现IDisposable接口
2. DI 只负责释放由其创建的对象实例 
3. DI 在容器或子容器释放时，释放由其创建的对象实例

# 需要注意的点
1. 避免通过静态属性的方式访问容器对象 
2. 避免在服务内部使用GetService方式来获取实例 
3. 避免使用静态属性存储单例，应该使用容器管理单例对象 
4. 避免在服务中实例化依赖对象，应该使用依赖注入来获得依赖对象 
5. 避免Singleton的类型注入Scoped或Transient的类型
6. 避免在根容器获取实现了IDisposable接口的瞬时服务 
7. 避免手动创建实现了IDisposable对象，应该使用容器来管理其生命周期

# 相关代码操作
```csharp
public void ConfigureServices(IServiceCollection services)
{
    #region 注册服务不同生命周期的服务
    services.AddSingleton<IMySingletonService, MySingletonService>();
    services.AddScoped<IMyScopedService, MyScopedService>();
    services.AddTransient<IMyTransientService, MyTransientService>();
    #endregion

    #region 其他注册方法
    services.AddSingleton<IMySingletonService>(new MySingletonService());  //直接注入实例
    services.AddSingleton<IMySingletonService>(serviceProvider =>
    {
       return new MySingletonService();
    });
    #endregion

    #region 尝试注册
    services.TryAddSingleton<IMySingletonService, MySingletonService>();
    services.TryAddEnumerable(ServiceDescriptor.Singleton<IMySingletonService, MySingletonService>());
    services.TryAddEnumerable(ServiceDescriptor.Singleton<IMySingletonService>(new MySingletonService()));
    services.TryAddEnumerable(ServiceDescriptor.Singleton<IMySingletonService>(p =>
    {
       return new MySingletonService();
    }));
    #endregion

    #region 移除和替换注册
    services.Replace(ServiceDescriptor.Singleton<IOrderService, OrderServiceEx>());
    services.RemoveAll<IOrderService>();
    #endregion

    #region 注册泛型模板
    services.AddSingleton(typeof(IGenericService<>), typeof(GenericService<>));
    #endregion

    //...
}
```








