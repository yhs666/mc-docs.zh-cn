---
title: 在不使用 Visual Studio 的情况下启用适用于 ASP.NET Core 的 Azure Application Insights | Azure Docs
description: 监视 ASP.NET Core Web 应用程序的可用性、性能和使用情况。
services: application-insights
documentationcenter: .net
author: lingliw
manager: digimobile
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 6/4/2019
ms.author: v-lingwu
ms.openlocfilehash: f2d9124748ecaab4eeffc2b02bc1062a998b604f
ms.sourcegitcommit: f818003595bd7a6aa66b0d3e1e0e92e79b059868
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66732230"
---
# <a name="application-insights-for-aspnet-core-applications"></a>适用于 ASP.NET Core 应用程序的 Application Insights

本文介绍了在不使用 Visual Studio IDE 的情况下，为 [ASP.NET Core](https://docs.microsoft.com/aspnet/core/?view=aspnetcore-2.2) 应用程序启用 Application Insights 的步骤。 如果已安装 Visual Studio IDE，请参阅[此文档](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)中的具体 Visual Studio 操作说明。 完成本文中所述的步骤后，Application Insights 将开始从 ASP.NET Core 应用程序收集请求、依赖项、异常、性能计数器、检测信号和日志。 所用的示例应用程序是一个面向 `netcoreapp2.2` 的 [MVC 应用程序](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/?view=aspnetcore-2.2)，但本文中的说明适用于所有 ASP.NET Core 应用程序。 适用时，本文会将例外情况标注出来。

## <a name="supported-scenarios"></a>支持的方案

无论应用程序在何处以何种方式运行，适用于 ASP.NET Core 的 Application Insights SDK（软件开发工具包）可以对其进行监视。 如果应用程序正在运行，并且与 Application Insights 服务建立了网络连接，则预期会收集遥测数据。 此项支持包括但不限于任何操作系统（Windows、Linux、Mac）、托管方法（进程内或进程外）、部署方法（框架相关或独立）、Web 服务器（IIS、Kestrel）、平台（Azure Web 应用、Azure VM、Docker、AKS 等）或 IDE（Visual Studio、VS Code、命令行）。

## <a name="prerequisites"></a>先决条件

- 一个正常运行的 ASP.NET Core 应用程序。 如果需要，请遵循[此指南](https://docs.microsoft.com/aspnet/core/getting-started/)创建一个 ASP.NET Core 应用程序。
- 将任何遥测数据发送到 Application Insights 服务时所需的有效 Application Insights 检测密钥。 如果需要，请遵照[这些说明](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource)创建新的 Application Insights 资源并获取检测密钥。

## <a name="enabling-application-insights-server-side-telemetry"></a>启用 Application Insights 服务器端遥测

1. 安装适用于 ASP.NET Core 的 Application Insights SDK（常规的 NuGet 包）。 建议始终使用最新稳定版本。 本文所述的某些功能在早期版本中可能不可用。 在[此处](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases)可以找到 SDK 的完整发行说明。 

下面显示了要添加到项目的 .csproj 文件中的更改。

```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
    </ItemGroup>
```

2. 将 `services.AddApplicationInsightsTelemetry();` 添加到 `Startup` 类中的 `ConfigureServices()` 方法。 下面是完整示例。

```csharp
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        // The following line enables Application Insights telemetry collection.
        services.AddApplicationInsightsTelemetry();

        // code adding other services for your application
        services.AddMvc();
    }
```

3. 设置检测密钥。

   尽管可将检测密钥作为参数提供给 `services.AddApplicationInsightsTelemetry("putinstrumentationkeyhere");`，但我们建议在配置中指定检测密钥。 以下示例演示如何在 `appsettings.json` 中指定检测密钥。 发布时，请确保将 `appsettings.json` 复制到应用程序根文件夹。

```json
    {
      "ApplicationInsights": {
        "InstrumentationKey": "putinstrumentationkeyhere"
      },
      "Logging": {
        "LogLevel": {
          "Default": "Warning"
        }
      }
    }
```

    Alternately, the instrumentation key can also be specified in either of the following environment variables.
    APPINSIGHTS_INSTRUMENTATIONKEY
    ApplicationInsights:InstrumentationKey

    Example:
    `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

`APPINSIGHTS_INSTRUMENTATIONKEY` 通常用于指定要部署到 Azure Web 应用的应用程序的检测密钥。

> [!NOTE]
> 在代码中指定的检测密钥优先于环境变量 `APPINSIGHTS_INSTRUMENTATIONKEY`，而后者又优先于其他选项。

4. 运行应用程序并向其发出请求。 现在，遥测数据应开始流入 Application Insights。 Application Insights SDK 自动收集以下遥测数据。

    1. 请求 - 传入应用程序的 Web 请求。
    1. 依赖项
        1. 使用 `HttpClient` 发出的 Http/Https 调用
        1. 使用 `SqlClient` 发出的 SQL 调用
        1. 使用 [Azure 存储客户端](https://www.nuget.org/packages/WindowsAzure.Storage/)* 发出的 Azure 存储调用
        1. [事件中心客户端 SDK](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) 1.1.0 和更高版本
        1. [服务总线客户端 SDK](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) 3.0.0 和更高版本

             \* 仅当使用 HTTP/HTTPS 时，才会自动跟踪 Azure Cosmos DB。 Application Insights 不会捕获 TCP 模式。


    1. [性能计数器](https://www.azure.cn/documentation/articles/app-insights-web-monitor-performance/)
        1. 对 ASP.NET Core 中的性能计数器的支持限制如下：
            1. 如果应用程序在 Azure Web 应用 (Windows) 中运行，则 SDK 2.4.1 和更高版本将收集性能计数器。
            1. 如果应用程序在 Windows 中运行，并且面向 `NETSTANDARD2.0` 或更高版本，则 SDK 2.7.0-beta3 和更高版本将收集性能计数器。
            1. 对于面向完整 .NET Framework 的应用程序，所有版本的 SDK 都支持性能计数器。

            在 Linux 中添加性能计数器支持后，本文档将会更新。
    1. [实时指标](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream)
    1. 自动从 SDK 2.7.0-beta3 或更高版本中捕获 `Warning` 或更高严重性的 `ILogger` 日志。 在[此处](https://docs.microsoft.com/azure/azure-monitor/app/ilogger)了解详细信息。

可能需要在几分钟后，遥测数据才开始显示在门户中。 若要快速检查是否一切正常，最好是在向运行中的应用程序发出请求时使用[实时指标](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream)。

## <a name="send-ilogger-logs-to-application-insights"></a>将 ILogger 日志发送到 Application Insights

Application Insights 支持捕获通过 ILogger 发送的日志。 请阅读[此处](https://docs.microsoft.com/azure/azure-monitor/app/ilogger)的完整文档。

## <a name="enable-client-side-telemetry-for-web-applications"></a>为 Web 应用程序启用客户端遥测

上述步骤足以开始收集服务器端遥测数据。 如果应用程序包含客户端组件，请遵循以下步骤开始从这些组件收集[使用情况遥测数据](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview)。

1. 在 _ViewImports.cshtml 中添加注入代码：

```cshtml
    @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
```

2. 在 _Layout.cshtml 中，将 HtmlHelper 插入到 `<head>` 节的末尾、任何其他脚本的前面。 要从页面报告的任何自定义 JavaScript 遥测数据应注入到此片段的后面：

```cshtml
    @Html.Raw(JavaScriptSnippet.FullScript)
    </head>
```

根据实际的应用程序修改文件名。 上面使用的名称摘自默认的 MVC 应用程序模板。

## <a name="configuring-application-insights-sdk"></a>配置 Application Insights SDK

可以自定义适用于 ASP.NET Core 的 Application Insights SDK 以更改默认配置。 Application Insights SDK ASP.NET 的用户可以使用 `ApplicationInsights.config` 或通过修改 `TelemetryConfiguration.Active` 来熟悉配置。 对于 ASP.NET Core，需以不同的方式完成配置。 使用 ASP.NET Core 的内置 [DependencyInjection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) 机制将 ASP.NET Core SDK 添加到应用程序，配置 ASP.NET Core SDK 也会使用 DependencyInjection。 除非另有说明，否则几乎所有的配置更改都是在 `Startup.cs` 类的 `ConfigureServices()` 方法中完成的。 请阅读以下部分了解详细信息。

> [!NOTE]
> 必须注意，不建议在 ASP.NET Core 应用程序中通过修改 `TelemetryConfiguration.Active` 来修改配置。

### <a name="configuring-using-applicationinsightsserviceoptions"></a>使用 ApplicationInsightsServiceOptions 进行配置

可以通过向 `services.AddApplicationInsightsTelemetry();` 传递 `ApplicationInsightsServiceOptions` 来修改某些通用设置。 下面显示了一个示例。

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions aiOptions
                    = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
        // Disables adaptive sampling.
        aiOptions.EnableAdaptiveSampling = false;

        // Disables QuickPulse (Live Metrics stream).
        aiOptions.EnableQuickPulseMetricStream = false;
        services.AddApplicationInsightsTelemetry(aiOptions);
    }
```

在[此处](https://github.com/Microsoft/ApplicationInsights-aspnetcore/blob/f0b8631e482d25982eb52092103b34e3ff6e5fef/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs)可以找到 `ApplicationInsightsServiceOptions` 中可配置设置的确切列表。

### <a name="sampling"></a>采样

适用于 ASP.NET Core 的 Application Insights SDK 支持 FixedRate 和自适应采样。 自适应采样默认已启用。 请阅读[此文档](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications)了解如何配置 ASP.NET Core 应用程序的采样。

### <a name="adding-telemetryinitializers"></a>添加 TelemetryInitializer

若要添加新的 `TelemetryInitializer`，请将其添加到 DependencyInjection 容器，如下所示。 SDK 会自动拾取添加到 DependencyInjection 容器的 `TelemetryInitializer`。

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
    }
```

### <a name="removing-telemetryinitializers"></a>删除 TelemetryInitializer

若要删除默认存在的所有或特定 TelemetryInitializer，请在调用 `AddApplicationInsightsTelemetry()` **之后**使用以下示例代码。

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // Remove a specific built-in TelemetryInitializer
        var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                         (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
        if (tiToRemove != null)
        {
            services.Remove(tiToRemove);
        }

        // Remove all initializers
        // This requires importing namespace using Microsoft.Extensions.DependencyInjection.Extensions;
        services.RemoveAll(typeof(ITelemetryInitializer));
    }
```

### <a name="adding-telemetryprocessors"></a>添加 TelemetryProcessor

可以使用 `IServiceCollection` 中的扩展方法 `AddApplicationInsightsTelemetryProcessor` 将自定义遥测处理程序添加到 `TelemetryConfiguration`。 使用以下示例。

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddApplicationInsightsTelemetry();
        services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();

        // If you have more processors:
        services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
    }
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>配置或删除默认的 TelemetryModule

以下自动收集模块默认已启用，它们负责自动收集遥测数据。 可禁用和配置这些模块，以改变默认行为。

1. `RequestTrackingTelemetryModule`
1. `DependencyTrackingTelemetryModule`
1. `PerformanceCollectorModule`
1. `QuickPulseTelemetryModule`
1. `AppServicesHeartbeatTelemetryModule`
1. `AzureInstanceMetadataTelemetryModule`

若要配置任何默认的 `TelemetryModule`，请按以下示例中所示使用 `IServiceCollection` 中的扩展方法 `ConfigureTelemetryModule<T>`。

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following configures DependencyTrackingTelemetryModule.
        // Similarly, any other default modules can be configured.
        services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) =>
                        {
                            module.EnableW3CHeadersInjection = true;
                        });

        // The following removes PerformanceCollectorModule to disable perf-counter collection.
        // Similarly, any other default modules can be removed.
        var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>(t => t.ImplementationType == typeof(PerformanceCollectorModule));
        if (performanceCounterService != null)
        {
         services.Remove(performanceCounterService);
        }
    }
```

### <a name="configuring-telemetry-channel"></a>配置遥测通道

使用的默认通道是 `ServerTelemetryChannel`。 可以遵循以下示例将其重写。

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // use the following to replace the default channel with InMemoryChannel.
        // this can also be applied to ServerTelemetryChannel as well.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="frequently-asked-questions"></a>常见问题

*1.除了自动收集的遥测数据以外，我还想要跟踪其他一些遥测数据。如何做到这一点？*

* 使用构造函数注入获取 `TelemetryClient` 的实例，然后对其调用所需的 `TrackXXX()` 方法。 不建议在 ASP.NET Core 应用程序中创建新的 `TelemetryClient` 实例，因为 DI 容器中已注册了 `TelemetryClient` 的单一实例，该实例与剩余的遥测功能共享 `TelemetryConfiguration`。 仅当需要与剩余的遥测功能使用不同的配置时，才建议创建新的 `TelemetryClient` 实例。 以下示例演示如何从控制器跟踪其他遥测数据。

```csharp
public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // use constructor injection to get TelemetryClient instance
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // call required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

 有关 Application Insights 中自定义数据报告的说明，请参阅 [Application Insights 自定义指标 API 参考](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/)。

*2.某些 Visual Studio 模板使用 IWebHostBuilder 中的 UseApplicationInsights() 扩展方法来启用 Application Insights。这种用法是否仍然有效？*

* 使用此方法启用 Application Insights 是有效的，Visual Studio 载入以及 Azure Web 应用扩展都使用此方法。 但是，我们建议使用 `services.AddApplicationInsightsTelemery()`，因为它提供重载来控制某些配置。 这两个方法在内部的作用相同，因此，如果不需要应用任何自定义配置，则调用其中任一个方法都是有效的。

*3.我正在将 ASP.NET Core 应用程序部署到 Azure Web 应用。是否仍要从 Web 应用启用 Application Insights 扩展？*

* 如果在生成时已按本文中所述安装了 SDK，则无需从 Web 应用门户启用 Application Insights 扩展。 即使安装了扩展，在检测到已将 SDK 添加到应用程序时，该扩展也仍会回退。 从扩展启用 Application Insights 就无需在应用程序中安装和更新 SDK。 但是，根据本文所述启用 Application Insights 会更灵活，原因如下。
    1. Application Insights 遥测功能将在以下位置或模式下继续运行
        1. 所有操作系统 - Windows、Linux、Mac。
        1. 所有发布模式 -“独立”或“框架相关”。
        1. 所有目标框架，包括完整的 .NET Framework。
        1. 所有托管选项 - Azure Web 应用、VM、Linux、容器、AKS、非 Azure。
    1. 从 Visual Studio 调试时，可在本地查看遥测数据。
    1. 允许使用 `TrackXXX()` API 跟踪其他自定义遥测数据。
    1. 可以完全控制配置。

*4.是否可以使用状态监视器之类的工具来启用 Application Insights 监视？*

* 否。 [状态监视器](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now)及其即将推出的替代服务 [IISConfigurator](https://github.com/Microsoft/ApplicationInsights-Announcements/issues/21) 目前仅支持 ASP.NET。 推出对 ASP.NET Core 应用程序的支持后，本文档将会更新。

*5.我有一个 ASP.NET Core 2.0 应用程序。系统是否会自动为该应用程序启用 Application Insights，而我不需要执行任何操作？*

* `Microsoft.AspNetCore.All` 2.0 元包包含 Application Insights SDK（版本 2.1.0），如果在 Visual Studio 调试器中运行该应用程序，则 Visual Studio 会启用 Application Insights，并在 IDE 本身中显示遥测数据。 除非显式指定检测密钥，否则遥测数据不会发送到 Application Insights 服务。 我们建议遵照本文中的说明启用 Application Insights，即使是对于 2.0 应用。

*6.我在 Linux 中运行应用程序。Linux 是否也支持所有上述功能？*

* 是的。 SDK 的功能支持在所有平台中是相同的，不过存在以下例外情况：
    1. 非 Windows 操作系统目前不支持性能计数器。 添加 Linux 支持后，本文档将会更新。
    1. 尽管默认已启用 `ServerTelemetryChannel`，但如果应用程序在非 Windows 操作系统中运行，出现网络问题时，通道不会自动创建本地存储文件夹来暂时保留遥测数据。 如果出现暂时性的网络或服务器问题，此限制会导致遥测数据丢失。 此问题的解决方法是让用户配置通道的本地文件夹，如下所示。

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure channel to use the given folder to temporarily
        // store telemetry items during network or application insights server issues.
        // User should ensure that the given folder already exists,
        // and that application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```




