# ASP.Net Core Features

**Hosting** is a fundamental feature of ASP.NET Core and is at the heart of any server-based application. An application host acts as a container and is responsible for managing the lifetime of the application. The host also contains environment
configuration and servers for handling requests. From a REST perspective, hosts and servers satisfy the Client-Server constraint.

The **middleware** feature aligns beautifully with the Layered System constraint from REST. The overall request/response architecture is primarily driven by middleware, which are components that can intercept requests and perform specific logic before possibly invoking the next component in the pipeline or stopping the request entirely.

To have maintainable and extensible code, you need to have loosely coupled components that are easy to test. Using a pattern called **dependency injection (DI)**, one can achieve loose-coupling by automatically resolving code dependencies by “injecting”
them when needed. In ASP.NET Core, DI is baked right in and available from the start.

One of the unique features of ASP.NET Core is **configuration**, which allows for application settings to be read at runtime from many different sources, like files, from the command line, environment variables, in-memory, encrypted secret stores, or your own tailor-made providers, like an INI file provider.

ASP.NET Core provides an extensive **logging** infrastructure that works with many providers to send entries to many destinations. You can control the level of logging as well as the scope of log entries, which groups log data for similar operations.

MVC is an example of an **application framework** supported by ASP.NET Core. Given the nature of the open-ended architectural design,
it allows you also to create your own application framework that can run seamlessly on ASP.NET Core.

---

#### Program.cs file - default initial code

```csharp

using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
public class Program
{
  public static void Main(string[] args)
  {
    BuildWebHost(args).Run();
  }

  public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
      .UseStartup<Startup>()
      .Build();
}

```

The above is actually short of:

```csharp
public static IWebHost BuildWebHost(string[] args) =>
  new WebHostBuilder()
    .UseKestrel()
    .UseContentRoot(Directory.GetCurrentDirectory())
    .ConfigureAppConfiguration(config =>
      config.AddJsonFile("appSettings.json", true)
    )
    .ConfigureLogging(logging=>
      logging
        .AddConsole()
        .AddDebug()
    )
    .UseIISIntegration()
    .UseStartup<Startup>()
    .Build();

```

## Dependency injection

It is essential to understand the concept of dependency injection
(DI). Having dependencies between the components of an application is inevitable, and if the references to them are not correctly designed, it can have a negative impact on the maintainability of the code. DI is a design pattern to allow instances of objects to be passed to other objects that require them at runtime.

Let’s say we have a class called `ComponentA` that is using `ComponentB`. The following example shows a typical scenario where no DI is used, and as a result these components are tightly coupled together:

```csharp

public class ComponentA
{
  private readonly ComponentB _componentB;
  public ComponentA()
  {
    this._componentB = new ComponentB();
  }
}
public class ComponentB
{
  public string Name { get; set; }
}

```

Instead of directly referencing an instance of `ComponentB`, we can decouple it by introducing an `IComponent` interface to abstract away the implementation and expect an instance of type `IComponent` in the constructor of `ComponentA`. In the example that follows, the previous code is now refactored to use DI, having `ComponentB` implement `IComponent` so that there is no direct reference to an instance of `ComponentB` anymore:

```csharp

public interface IComponent
{
  string Name { get; }
}

public class ComponentA
{
  private readonly IComponent _componentB;

  public ComponentA(IComponent componentB)
  {
    this._componentB = componentB;
  }
}

public class ComponentB: IComponent
{
  public string Name { get; set; } = nameof(ComponentB)
}

// The `nameof` operator obtains the name of a variable, type, or member as the string constant.

// E.g: Console.WriteLine(nameof(System.Collections.Generic)); output: Generic

```

When we run this code as is, it will result in a `NullReferenceException` error because `ComponentA` is expecting an object of type `IComponent`, and although `ComponentB` implements the `IComponent` interface, there is nothing configured to pass in
the required instance of `IComponent` to the constructor of `ComponentA`.

For the code to run without this issue, we need a mechanism to pass the correct instance of a requested type during runtime. This can be achieved by making use of an ***Inversion of Control*** (IoC) container to register all the required dependencies and their instances. There are many frameworks available on NuGet that provide IoC containers
for dependency resolution, namely Unity, Castle Windsor, Autofac, and Ninject.

**Note**: As a general rule of thumb, avoid the explicit instantiation of classes, as doing this results in a tightly coupled system.

ASP.NET Core implements DI as a first-class citizen in its infrastructure and has an IoC container built into its core. Most of the moving parts of this framework are abstracted away from each other to promote extensibility and modularity. This means that if you choose to use your own favorite IoC container instead of the built-in one, you
absolutely can.

---

## IoC Container

IoC Container (a.k.a. DI Container) is a framework for implementing automatic Dependency Injection. It manages object creation and it's life-time, and also injects dependencies to the class.

The IoC container creates an object of the specified class and also injects all the dependency objects through a constructor, a property or a method at run time and disposes it at the appropriate time. This is done so that we don't have to create and manage objects manually.

All the containers must provide easy support for the following DI lifecycle.

**Register**: The container must know which dependency to instantiate when it encounters a particular type. This process is called registration. Basically, it must include some way to register type-mapping.

**Resolve**: When using the IoC container, we don't need to create objects manually. The container does it for us. This is called resolution. The container must include some methods to resolve the specified type; the container creates an object of the specified type, injects the required dependencies if any and returns the object.

**Dispose**: The container must manage the lifetime of the dependent objects. Most IoC containers include different lifetimemanagers to manage an object's lifecycle and dispose it.

---

## Design Principle vs Design Pattern

### Design Principle

Design principles provide high level guidelines to design better software applications. They do not provide implementation guidelines and are not bound to any programming language. The SOLID (SRP, OCP, LSP, ISP, DIP) principles are one of the most popular sets of design principles.

For example, the Single Responsibility Principle (SRP) suggests that a class should have only one reason to change. This is a high-level statement which we can keep in mind while designing or creating classes for our application. SRP does not provide specific implementation steps but it's up to you how you implement SRP in your application.

### Design Pattern

Design Pattern provides low-level solutions related to implementation, of commonly occurring object-oriented problems. In other words, design pattern suggests a specific implementation for the specific object-oriented programming problem. For example, if you want to create a class that can only have one object at a time, then you can use the Singleton design pattern which suggests the best way to create a class that can only have one object.

Design patterns are tested by others and are safe to follow, e.g. Gang of Four patterns: Abstract Factory, Factory, Singleton, Command, etc.

Principle --> Inversion of Control & Dependency Inversion Principle

Pattern   --> Dependency Injection

Framework --> IoC Container

read: https://www.tutorialsteacher.com/ioc/introduction

---

## Application Startup

The `UseStartup` method is one of the critical methods that extends an `IWebHostBuilder` and registers aclass that is responsible for configuring the application startup process.
The type specified in `UseStartup` needs to match a specific signature to have the host launch the application correctly. The runtime requires the specified startup class to contain two public functions, namely `ConfigureServices`, which is ***optional***, and `Configure`, which is ***compulsory***. For example, let’s say that the startup class is defined as `UseStartup<Foo>();` the structure of Foo should match the following:

```csharp

public class Foo
{
  //optional
  public void ConfigureServices(IServiceCollection services)
  {

  }

 //required
  public void Configure()
  {

  }
}

```

Across ASP.NET Core we will notice that dependencies and configurations conform to a certain "Add/Use" style by first defining what is required and then how it is used. By explicitly specifying components we need, it optimizes performance and thus increases the application’s performance, as we only pay for what we use, not the whole thing.

In the `ConfigureServices` method, all the application-level dependencies are registered inside the default IoC container by adding them to an `IServiceCollection`. Expanding on our previous example, we would map a singleton instance of `ComponentB` to an `IComponent` service as follows:

```csharp

public void ConfigureServices(IServiceCollection services)
{
  services.AddSingleton<IComponent, ComponentB>();
}

```

**Note**: It is also possible to register a dependency that binds to itself instead of using any interfaces by directly expecting the concrete type in the constructor and calling `services.AddSingleton<T>`, where `T` is the concrete type in this case.


In the startup class, the `Configure` method is responsible for the actual configuration of the application’s HTTP request pipeline and is required by the runtime. This method can contain many dependent parameters that are resolved from the IoC container.

Let’s build on the previous examples to have our application print out the name of an `IComponent` to the response when invoking it and show the Configure method in action:

```csharp

public void Configure(IApplicationBuilder app, IComponent component)
{
    app.Run(async (context) =>
      await context.Response.WriteAsync($"Name is {component.Name}")
    );
}

```

The two variables that are automatically resolved are an `IApplicationBuilder`, which is the mechanism to configure an application’s request, and an `IComponent`. The `IApplicationBuilder` extends with a `Run` function, which passes a `RequestDelegate` that writes out the Name property of the IComponent to the response. Running the application will result in the response being “Name is ComponentB.”

*Optional*

It is also possible to configure the application’s dependencies and HTTP request pipeline directly inline when defining the web host, without the use of a startup class.

Given the default web-host configuration, an inline startup definition could look like the following:

```csharp

  WebHost.CreateDefaultBuilder()
    .ConfigureServices(services =>
      services.AddSingleton<IComponent, ComponentB>()
    )
    .Configure(app =>
      {
        var component = app.ApplicationServices.GetRequiredService<IComponent>();
        app.Run(async (context) =>
          await context.Response.WriteAsync($"Name is {component.Name}")
        );
      })
    .Build();

```

One of the drawbacks of defining the bootstrapping configuration inline is that we can only pass in one parameter as IApplicationBuilder to the Configure extension method. This forces us to resolve any dependencies by calling `GetRequiredServices` manually.
