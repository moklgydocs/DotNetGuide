# .NET Core完整面试题合集
![.NET Core面试题合集](https://images.cnblogs.com/cnblogs_com/Can-daydayup/2145479/o_240328134732_%E9%9D%A2%E8%AF%95%E5%AE%9D%E5%85%B8.png)

> 🎯 **从初级到高级，从底层原理到应用实战的完整.NET Core面试题集合**
> 
> 本合集基于DotNetGuide仓库积累的丰富面试内容，按照难度层次和知识领域精心整理，旨在帮助.NET开发者系统性地准备面试，查漏补缺，提升技术能力。

## 📖 使用说明

- **🟢 初级（0-2年经验）**：基础语法、概念理解
- **🟡 中级（2-5年经验）**：核心技术、设计模式
- **🔴 高级（5年+经验）**：架构设计、性能优化
- **🟣 深入（资深级别）**：底层原理、源码分析
- **🟠 实战（应用级别）**：项目开发、问题解决

---

## 🎯 目录导航

### 基础篇（初级）
- [C#基础语法](#c基础语法-🟢)
- [.NET Core基础概念](#net-core基础概念-🟢)
- [面向对象编程](#面向对象编程-🟢)

### 核心篇（中级）
- [多线程与异步编程](#多线程与异步编程-🟡)
- [内存管理与垃圾回收](#内存管理与垃圾回收-🟡)
- [LINQ与集合操作](#linq与集合操作-🟡)
- [设计模式](#设计模式-🟡)

### 高级篇（高级）
- [微服务架构](#微服务架构-🔴)
- [性能优化](#性能优化-🔴)
- [安全性](#安全性-🔴)
- [分布式系统](#分布式系统-🔴)

### 深入篇（深入）
- [CLR运行时](#clr运行时-🟣)
- [JIT编译](#jit编译-🟣)
- [反射与元数据](#反射与元数据-🟣)

### 实战篇（应用）
- [Web API开发](#web-api开发-🟠)
- [数据库操作](#数据库操作-🟠)
- [部署与运维](#部署与运维-🟠)
- [算法与数据结构](#算法与数据结构-🟠)

---

## C#基础语法 🟢

### 1. 值类型与引用类型
**面试频率：⭐⭐⭐⭐⭐**

**问题：** 解释C#中值类型和引用类型的区别，并举例说明。

**答案：**
- **值类型**：直接存储数据，存储在栈中，包括基本数据类型、结构体、枚举
  - 示例：`int, double, bool, DateTime, 自定义struct`
  - 特点：赋值时复制值本身，修改副本不影响原值
- **引用类型**：存储对象引用，对象存储在堆中
  - 示例：`class, string, array, delegate`
  - 特点：赋值时复制引用，多个变量可指向同一对象

```csharp
// 值类型示例
int a = 10;
int b = a;  // 复制值
b = 20;     // a仍为10，b为20

// 引用类型示例
var list1 = new List<int> { 1, 2, 3 };
var list2 = list1;  // 复制引用
list2.Add(4);       // list1和list2都包含{1,2,3,4}
```

### 2. 装箱与拆箱
**面试频率：⭐⭐⭐⭐**

**问题：** 什么是装箱和拆箱？它们对性能有什么影响？

**答案：**
- **装箱（Boxing）**：将值类型转换为引用类型（object），需要在堆上分配内存
- **拆箱（Unboxing）**：将引用类型转换回值类型，需要类型检查和值复制

```csharp
// 装箱
int i = 123;
object o = i;  // 装箱，性能开销

// 拆箱
int j = (int)o;  // 拆箱，需要显式转换

// 避免装箱的方法
List<int> numbers = new List<int>();  // 泛型避免装箱
numbers.Add(123);  // 无装箱
```

**性能影响：**
- 装箱：堆内存分配、对象创建开销
- 拆箱：类型检查、值复制开销
- 频繁装拆箱会导致垃圾回收压力增大

### 3. var关键字和动态类型
**面试频率：⭐⭐⭐**

**问题：** var、dynamic和具体类型声明的区别？

**答案：**
```csharp
// var - 编译时类型推断
var name = "John";  // 编译器推断为string
var count = 10;     // 编译器推断为int

// dynamic - 运行时类型检查
dynamic obj = "Hello";
obj = 123;          // 运行时改变类型
var length = obj.Length;  // 运行时错误

// 显式类型
string title = "Manager";  // 明确类型
```

**使用建议：**
- var：当类型明显时使用，提高可读性
- dynamic：与COM、反射等场景，谨慎使用
- 显式类型：复杂逻辑或团队规范要求时使用

### 4. 字符串操作性能
**面试频率：⭐⭐⭐⭐**

**问题：** string、StringBuilder和string插值的性能对比？

**答案：**
```csharp
// 性能对比（按性能从低到高）
// 1. 字符串连接（最慢）- 每次创建新字符串
string result = "";
for(int i = 0; i < 1000; i++)
    result += i.ToString();

// 2. StringBuilder（适中）- 可变缓冲区
var sb = new StringBuilder();
for(int i = 0; i < 1000; i++)
    sb.Append(i);
string result2 = sb.ToString();

// 3. 字符串插值（推荐）- 编译器优化
string name = "John";
int age = 25;
string result3 = $"Name: {name}, Age: {age}";

// 4. string.Join（批量连接最快）
string[] items = {"a", "b", "c"};
string result4 = string.Join(",", items);
```

### 5. 泛型约束
**面试频率：⭐⭐⭐**

**问题：** C#泛型约束有哪些类型？请举例说明。

**答案：**
```csharp
// where T : class - 引用类型约束
public class Repository<T> where T : class
{
    public void Save(T entity) { }
}

// where T : struct - 值类型约束
public class Calculator<T> where T : struct
{
    public T Add(T a, T b) { return default(T); }
}

// where T : new() - 无参构造函数约束
public class Factory<T> where T : new()
{
    public T Create() => new T();
}

// where T : BaseClass - 基类约束
public class Service<T> where T : BaseService
{
    public void Process(T service) => service.Execute();
}

// where T : IInterface - 接口约束
public class Handler<T> where T : IHandler
{
    public void Handle(T handler) => handler.Process();
}

// 多重约束
public class Manager<T> where T : class, IDisposable, new()
{
    public void Create() 
    {
        using (var instance = new T())
        {
            // 使用实例
        }
    }
}
```

---

## .NET Core基础概念 🟢

### 6. .NET Core与.NET Framework区别
**面试频率：⭐⭐⭐⭐⭐**

**问题：** .NET Core相比.NET Framework有哪些优势？

**答案：**

| 特性 | .NET Framework | .NET Core/.NET 5+ |
|------|----------------|-------------------|
| 跨平台 | 仅Windows | Windows/Linux/macOS |
| 开源 | 部分开源 | 完全开源 |
| 性能 | 较低 | 显著提升 |
| 部署 | 依赖Framework | 自包含部署 |
| 包管理 | NuGet | NuGet + 更好的包管理 |
| CLI工具 | 有限 | 丰富的CLI命令 |
| 容器支持 | 有限 | 原生支持Docker |

**主要优势：**
1. **跨平台开发和部署**
2. **高性能**：启动更快，内存占用更少
3. **模块化**：只加载需要的程序集
4. **Side-by-side部署**：不同版本共存
5. **云原生支持**：容器化、微服务友好

### 7. .NET Core应用程序生命周期
**面试频率：⭐⭐⭐⭐**

**问题：** 描述.NET Core应用程序的启动过程。

**答案：**
```csharp
// Program.cs - 应用程序入口点
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}

// Startup.cs - 应用配置
public class Startup
{
    // 1. 配置服务（在容器中注册服务）
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers();
        services.AddDbContext<AppDbContext>();
        services.AddScoped<IUserService, UserService>();
    }

    // 2. 配置HTTP请求管道（中间件）
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        
        app.UseRouting();
        app.UseAuthentication();
        app.UseAuthorization();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapControllers();
        });
    }
}
```

**启动流程：**
1. Main方法执行
2. 创建并配置Host
3. 调用ConfigureServices注册服务
4. 调用Configure配置中间件管道
5. 启动Kestrel服务器
6. 开始监听HTTP请求

### 8. 依赖注入（DI）
**面试频率：⭐⭐⭐⭐⭐**

**问题：** .NET Core内置DI容器支持哪些生命周期？请举例说明。

**答案：**

**三种生命周期：**
1. **Transient（瞬态）**：每次请求都创建新实例
2. **Scoped（作用域）**：在同一个请求/作用域内是单例
3. **Singleton（单例）**：整个应用程序生命周期内只有一个实例

```csharp
// 服务注册
public void ConfigureServices(IServiceCollection services)
{
    // Transient - 每次注入都创建新实例
    services.AddTransient<IEmailService, EmailService>();
    
    // Scoped - 请求期间单例
    services.AddScoped<IUserService, UserService>();
    
    // Singleton - 应用程序单例
    services.AddSingleton<ICacheService, CacheService>();
    
    // 工厂模式
    services.AddSingleton<Func<string, INotificationService>>(provider =>
        key => key switch
        {
            "email" => provider.GetService<IEmailService>(),
            "sms" => provider.GetService<ISmsService>(),
            _ => throw new ArgumentException($"Unknown service: {key}")
        });
}

// 构造函数注入
public class UserController : ControllerBase
{
    private readonly IUserService _userService;
    private readonly ILogger<UserController> _logger;
    
    public UserController(IUserService userService, ILogger<UserController> logger)
    {
        _userService = userService;
        _logger = logger;
    }
}
```
```