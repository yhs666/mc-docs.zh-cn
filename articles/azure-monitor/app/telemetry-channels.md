---
title: Azure Application Insights 中的遥测通道 | Microsoft Docs
description: 如何自定义适用于 .NET/.NET Core 的 Azure Application Insights SDK 中的遥测通道。
services: application-insights
documentationcenter: .net
author: cijothomas
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/14/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: 1f86405b8425fce06fa072e3ad7435533005eaa6
ms.sourcegitcommit: 461c7b2e798d0c6f1fe9c43043464080fb8e8246
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2019
ms.locfileid: "68818145"
---
# <a name="telemetrychannel-in-application-insights"></a>Application Insights 中的遥测通道

遥测通道是 [Azure Application Insights SDK](../../azure-monitor/app/app-insights-overview.md) 不可或缺的组成部分。 它可以管理缓冲以及将遥测数据传输到 Application Insights 服务的过程。 SDK 的 .NET 和 .NET Core 版本包含两个内置的遥测通道 - `InMemoryChannel` 和 `ServerTelemetryChannel`。 本文将详细介绍每个通道，包括用户如何自定义通道的行为。

## <a name="what-is-a-telemetrychannel"></a>什么是遥测通道？

`TelemetryChannel` 负责缓冲以及将遥测项发送到 Application Insights 服务，存储在该服务中的项可用于查询和分析。 它是实现 [`Microsoft.ApplicationInsights.ITelemetryChannel`](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.channel.itelemetrychannel?view=azure-dotnet) 接口的任何类

在调用所有 `TelemetryInitializer` 和 `TelemetryProcessor` 之后才调用遥测通道的 `Send(ITelemetry item)` 方法。 这意味着，`TelemetryProcessor` 删除的任何项不会进入通道。 通常，`Send()` 不会立即将项发送到后端。 这些项通常在内存中缓冲并分批发送，以提高传输效率。

[LiveMetrics](live-stream.md) 还包含一个自定义通道用于支持遥测数据的实时流式传输。 此通道独立于常规的遥测通道，本文档不适用于 `LiveMetrics` 使用的通道。

## <a name="built-in-telemetrychannels"></a>内置的遥测通道

Application Insights .NET/.NET Core SDK 随附了两个内置的通道：

* **InMemoryChannel**
`InMemoryChannel` 是一个轻型通道，它会在内存中将项缓冲到发送为止。 项在内存中缓冲，每隔 30 秒刷新一次，或者每当缓冲了 500 个项时刷新。 此通道提供最基本的可靠性保证，因为它在失败时不会重试发送遥测数据。 此通道不会在磁盘上保留项，因此，在（正常或非正常）关闭应用程序后，未发送的所有项都会永久丢失。 此通道实现 `Flush()` 方法，使用此方法可以强制同步刷新内存中的所有遥测项。 这非常适合用于短时间运行的、最好是进行同步刷新的应用程序。

    此通道随附在 `Microsoft.ApplicationInsights` NuGet 包本身中，是未配置任何其他通道时，SDK 使用的默认通道。

* **ServerTelemetryChannel**
`ServerTelemetryChannel` 是更高级的通道，它具有重试策略，并可以在本地磁盘上存储数据。 如果发生暂时性错误，此通道会重试发送遥测数据。 在网络中断或者遥测量较高时，此通道还会使用本地磁盘存储在磁盘上保留项。 由于这些重试机制和本地磁盘存储，我们认为此通道更可靠，建议在所有生产方案中使用。 此通道是根据链接的官方文档配置的 [ASP.NET](/azure-monitor/app/asp-net) 和 [ASP.NET Core](/azure-monitor/app/asp-net-core) 应用程序的默认通道。此通道已针对长时间运行的服务器方案进行优化。 此通道实现的 [`Flush()`](#which-channel-should-i-use) 方法不是同步的。

    此通道随附在 NuGet 包 `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel` 中，使用 NuGet 包 `Microsoft.ApplicationInsights.Web` 或 `Microsoft.ApplicationInsights.AspNetCore` 时会自动启动它。

## <a name="configuring-telemetrychannel"></a>配置遥测通道

可以通过在活动的 `TelemetryConfiguration` 上设置所需的 `TelemetryChannel` 来配置遥测通道。 对于 ASP.NET 应用程序，配置过程涉及到在 `TelemetryConfiguration.Active` 上设置 `TelemetryChannel`，或修改 `ApplicationInsights.config`。 对于 ASP.NET Core 应用程序，配置过程涉及到将所需的通道添加到依赖项注入容器。

以下示例演示了用户为通道配置 `StorageFolder` 的情况。 `StorageFolder` 只是可配置的设置之一。 [设置部分](telemetry-channels.md#configurable-settings-in-channel)描述了配置设置的完整列表。

## <a name="configuration-using-applicationinsightsconfig-for-aspnet-applications"></a>使用适用于 ASP.NET 应用程序的 ApplicationInsights.Config 进行配置

[ApplicationInsights.config](configuration-with-applicationinsights-config.md) 中的以下节显示了在将 `StorageFolder` 设置为自定义位置的情况下配置的 ServerTelemetryChannel。

```xml
    <TelemetrySinks>
        <Add Name="default">
            <TelemetryProcessors>
                <!--TelemetryProcessors omitted for brevity  -->
            </TelemetryProcessors>
            <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel">
                <StorageFolder>d:\temp\applicationinsights</StorageFolder>
            </TelemetryChannel>
        </Add>
    </TelemetrySinks>
```

## <a name="configuration-in-code-for-aspnet-applications"></a>ASP.NET 应用程序代码中的配置

以下代码在将 `StorageFolder` 设置为自定义位置的情况下设置 ServerTelemetryChannel。 应将此代码添加到应用程序的开头，通常是添加到 `Global.aspx.cs` 中的 Application_Start() 方法内

```csharp
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
protected void Application_Start()
{
    var serverTelemetryChannel = new ServerTelemetryChannel();
    serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
    serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
    TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
}
```

## <a name="configuration-in-code-for-aspnet-core-applications"></a>ASP.NET Core 应用程序代码中的配置

按如下所示修改 `Startup.cs` 类的 `ConfigureServices` 方法。

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

public void ConfigureServices(IServiceCollection services)
{
    // This sets up ServerTelemetryChannel with StorageFolder set to a custom location.
    services.AddSingleton(typeof(ITelemetryChannel), new ServerTelemetryChannel() {StorageFolder = @"d:\temp\applicationinsights" });

    services.AddApplicationInsightsTelemetry();
}

```

> [!NOTE]
> 必须注意，不建议使用 `TelemetryConfiguration.Active` 配置 ASP.NET Core 应用程序的通道。

## <a name="configuring-telemetrychannel-in-code-for-netnet-core-console-applications"></a>在 .NET/.NET Core 控制台应用程序代码中配置遥测通道

.NET 和 .NET Core 控制台应用的代码相同，如下所示。

```csharp
var serverTelemetryChannel = new ServerTelemetryChannel();
serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
```

## <a name="operational-details-of-servertelemetrychannel"></a>ServerTelemetryChannel 的操作详细信息

`ServerTelemetryChannel` 在内存中缓冲区内缓冲抵达的项。 每隔 30 秒或每当缓冲了 500 个项时，这些项将序列化、压缩并存储到 `Transmission` 实例中。 单个 `Transmission` 实例最多包含 500 个项，表示通过 Application Insights 服务的单个 https 调用发送的遥测数据批。 默认情况下，最多可以并行发送 10 个 `Transmission`。 如果遥测数据以更快的速度抵达，或者网络/Application Insights 后端速度缓慢，则 `Transmission` 将存储到内存中。 此内存中传输缓冲区的默认容量为 5 MB。 一旦超过内存中容量，`Transmission` 将存储在本地磁盘上（最多 50 MB）。 出现网络问题时，`Transmission` 也会存储在本地磁盘上。 当应用程序崩溃时，只有存储在本地磁盘中的项才能幸存。每当应用程序再次启动时，将发送这些项。

## <a name="configurable-settings-in-channel"></a>通道中的可配置设置

每个通道的可配置设置完整列表如下：

[InMemoryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/Channel/InMemoryChannel.cs)

[ServerTelemetryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs)

下面列出了 `ServerTelemetryChannel` 的最常用设置：

1. `MaxTransmissionBufferCapacity` - 用于在内存中缓冲传输内容的通道所用的内存量，以字节为单位。 一旦达到此容量，新项将直接存储到本地磁盘。 默认值为 5 MB。 设置更大的值可以减少磁盘用量，但务必注意，如果应用程序崩溃，内存中的项将会丢失。

2. `MaxTransmissionSenderCapacity` - 要同时发送到 Application Insights 的最大 `Transmission` 数量。 默认值为 10，但可配置为更大的数字。 如果要生成巨量的遥测数据（通常是在执行负载测试和/或关闭采样时），建议使用此设置。

3. `StorageFolder` - 磁盘上的文件夹，通道根据需要将项存储到其中。 在 Windows 中，如果未显式配置其他文件夹，则会使用 %LocalAppData% 或 %Temp%。 在非 Windows 环境中，用户**必须**配置有效的位置，否则遥测数据不会存储到本地磁盘。

## <a name="which-channel-should-i-use"></a>应使用哪个通道？

对于长时间运行的应用程序的大多数生产方案，建议使用 `ServerTelemetryChannel`。 `ServerTelemetryChannel` 的 `Flush()` 方法实现不是同步的，`Flush()` 不保证发送内存/磁盘中所有等待中的项。 如果在应用程序即将关闭时要使用此通道，则我们建议在针对此通道调用 `Flush()` 后留出一定的延迟。 所需的确切延迟时间不可预测，因为这取决于多种因素，例如，有多少个项或 `Transmissions` 位于内存中和磁盘中、其中有多少个正在传输到备份，以及该通道是否位于指数退让方案的中间部分。 如果需要执行同步刷新，则我们建议使用 `InMemoryChannel`。

## <a name="frequently-asked-questions"></a>常见问题解答

### <a name="does-applicationinsights-channel-offer-guaranteed-telemetry-delivery-or-what-are-the-scenarios-where-telemetry-can-be-lost"></a>Application Insights 通道是否提供有保证的遥测数据传送，或者，在哪些情况下遥测数据可能会丢失？ 

* 简单地讲，在向后端传送遥测数据方面，没有任何内置通道能够提供事务类型保证。 尽管在可靠传送遥测数据方面 `ServerTelemetryChannel` 比 `InMemoryChannel` 更先进，并且它会尽最大努力来尝试发送遥测数据，但在多种情况下，遥测数据仍可能会丢失。 丢失遥测数据的几种常见情况包括：

1. 每当应用程序崩溃时，内存中的项就会丢失。
1. 出现网络中断或者 Application Insights 后端问题时，遥测数据将存储到本地磁盘。 但是，超过 24 小时的项将被丢弃。 因此，如果网络问题的持续时间很长，则遥测数据将会丢失。
1. 在 Windows 中，用于存储遥测数据的默认磁盘位置是 %LocalAppData% 或 %Temp%。 这些位置通常位于计算机本地。 如果以物理方式将应用程序从一个位置迁移到另一个位置，在此位置存储的所有遥测数据将会丢失。
1. 在 Azure Web 应用 (Windows) 中，默认磁盘位置为“D:\local\LocalAppData”。 此位置并非永久性的，在应用重启、横向扩展等情况下将被擦除，导致这些位置存储的遥测数据丢失。 用户可将存储位置替代为某个永久性位置（例如“D:\home”），但这些永久性位置由远程存储提供服务，因此运行速度可能很缓慢。

### <a name="does-servertelemetrychannel-work-in-non-windows-systems"></a>ServerTelemetryChannel 是否可在非 Windows 系统中工作？ 

* 尽管包/命名空间的名称提到了 WindowsServer，但非 Windows 系统也支持此通道，不过存在以下差别。 在非 Windows 中，该通道默认不会创建本地存储文件夹。 用户必须创建本地存储文件夹，并将通道配置为使用该文件夹。 配置本地存储后，该通道在 Windows 和非 Windows 系统中的工作方式相同。

### <a name="does-the-sdk-create-temporary-local-storage-is-the-data-encrypted-at-storage"></a>SDK 是否创建临时本地存储？  存储中的数据是否会加密？

* 出现网络问题或限制时，SDK 会将遥测项存储在本地存储中。 此数据不会在本地加密。
对于 Windows 系统，SDK 会自动在 TEMP 或 APPDATA 目录中创建临时本地文件夹，并仅限管理员和当前用户访问该文件夹。
对于非 Windows，SDK 不会自动创建本地存储，因此默认不会在本地存储数据。 用户可以自行创建存储目录，并将通道配置为使用该目录。 在这种情况下，用户需负责确保此目录受到保护。
详细了解[数据保留和隐私](data-retention-privacy.md#does-the-sdk-create-temporary-local-storage)。

## <a name="open-source-sdk"></a>开源 SDK
与每个 Application Insights SDK 一样，通道也是开源的。 请在[官方 GitHub 存储库](https://github.com/Microsoft/ApplicationInsights-dotnet)中阅读和贡献代码，或者报告问题。

## <a name="next-steps"></a>后续步骤

* [采样](../../azure-monitor/app/sampling.md)
* [SDK 故障排除](../../azure-monitor/app/asp-net-troubleshoot-no-data.md)
