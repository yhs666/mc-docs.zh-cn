---
title: "监视 Web 应用 | Azure"
description: "了解如何在 Web 应用中设置监视"
services: App-Service
keywords: 
author: btardif
ms.author: v-dazen
origin.date: 04/04/2017
ms.date: 07/03/2017
ms.topic: article
ms.service: app-service-web
ms.openlocfilehash: addddec44caf76ba02eb4f907afd3e4a78afea83
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 监视应用服务
<a id="monitor-app-service" class="xliff"></a>
本教程逐步讲解如何监视应用，以及在出现问题时如何使用内置的平台工具解决问题。

本文档的每个部分介绍一项具体功能。 结合使用这些功能可以：
- 识别应用中的问题。
- 确定该问题是由代码还是平台引起的。
- 在代码中缩小问题的起源范围。
- 调试并解决问题。

## 开始之前
<a id="before-you-begin" class="xliff"></a>
- 需要使用一个 Web 应用来监视活动和遵循所述的步骤。
    - 可以遵循 [Create an ASP.NET app in Azure with SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md)（在 Azure 中创建包含 SQL 数据库的 ASP.NET 应用）教程中所述的步骤创建一个应用程序。

- 如果想要对应用程序试用**远程调试**，需要安装 Visual Studio。
    - 如果尚未安装 Visual Studio 2017，可以下载并使用免费的 [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)。
    - 在安装 Visual Studio 的过程中，请确保启用“Azure 开发”。

## <a name="metrics"></a>步骤 1 - 查看指标
**指标**有助于了解以下信息：
- 应用程序运行状况
- 应用性能
- 资源消耗量

调查应用程序问题时，最好是先查看指标。 Azure 门户使用 **Azure Monitor** 能直观快速地检查应用的各指标。

指标提供应用的多个关键聚合数据的历史视图。 对于应用服务中托管的任何应用，应该同时监视 Web 应用和应用服务计划。

> [!NOTE]
> 应用服务计划表示用于托管应用的物理资源集合。 分配到应用服务计划的所有应用程序将共享该计划定义的资源，在托管多个应用时可以节省成本。
>
> 应用服务计划定义：
> * 区域：中国北部、中国东部、中国北部，等等。
> * 实例大小：小、中、大，等等。
> * 规模计数：一个、两个、三个实例，等等。
> * SKU：免费、共享、基本、标准、高级，等等。

若要查看 Web 应用的指标，请转到要监视的应用的“概述”边栏选项卡。 在此处，可以**监视磁贴**的形式查看应用的指标图表。 单击该磁贴可以编辑和配置要查看哪些指标，以及要显示哪个时间范围。

默认情况下，资源边栏选项卡提供过去一个小时的应用程序请求和错误视图。
![监视应用](media/app-service-web-tutorial-monitoring/app-service-monitor.png)

在示例中可以看到，某个应用程序正在生成大量的 **HTTP 服务器错误**。 出现大量错误，就是需要调查此应用程序的第一个迹象。

> [!TIP]
> 使用以下链接了解有关 Azure Monitor 的详细信息：
> - [Azure Monitor 入门](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [Azure 仪表板](..\azure-portal\azure-portal-dashboards.md)

## <a name="alerts"></a>步骤 2 - 配置警报
可将**警报**配置为在应用出现特定的状态时触发。

在[步骤 1 - 查看指标](#metrics)中，我们看到该应用程序生成了大量错误。

让我们配置一个警报，以便在发生错误时自动接收通知。 在本例中，我们希望每当 HTTP 50X 错误数超过特定的阈值时，该警报就会发送电子邮件。

若要创建警报，请导航到“监视” > “警报”，然后单击“[+] 添加警报”。

![警报](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts.png)

为警报配置提供值：
- **资源：**要使用警报监视的站点。
- **名称：**警报的名称，在本例中为 *High HTTP 50X*。
- **说明：**描述此警报的监视内容的纯文本说明。
- **警报依据：**警报可以监视指标或事件，在本示例中，我们要监视指标。
- **指标：**要监视的指标，在本例中为“HTTP 服务器错误”。
- **条件：**何时触发警报，本例选择了“大于”选项。
- **阈值：**要监视的值，在本例中为 *400*。
- **时间段：**警报针对指标的平均值运行。 时间段越小，生成的警报越灵敏。 在本例中，监视的时间段为“5 分钟”。
- **电子邮件所有者和参与者：**在本例中设置为“已启用”。

创建警报后，每当应用超过配置的阈值时，系统会发送一封电子邮件。 还可以在 Azure 门户中查看活动的警报。

![触发的警报](media/app-service-web-tutorial-monitoring/app-service-monitor-alerts-triggered.png)

> [!TIP]
> 使用以下链接了解有关 Azure 警报的详细信息：
> - [对指标执行操作](..\monitoring-and-diagnostics\monitoring-overview.md)
> - [创建指标警报](..\monitoring-and-diagnostics\insights-alerts-portal.md)

## <a name="logging"></a>步骤 3 - 日志记录
将故障原因的范围缩小到应用程序问题后，可以查看应用程序和服务器日志来获取详细信息。

使用日志记录可以收集 Web 应用的**应用程序诊断**日志和 **Web 服务器诊断**日志。

### 应用程序诊断
<a id="application-diagnostics" class="xliff"></a>
使用应用程序诊断可以捕获应用程序在运行时生成的跟踪。

将跟踪添加到应用程序可以大大提高调试和查明问题的能力。

在 ASP.NET 中，可以使用 [System.Diagnostics.Trace 类](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx)记录应用程序跟踪，生成可由日志基础结构捕获的事件。 还可以指定跟踪的严重性以方便筛选。

```csharp
public ActionResult Delete(Guid? id)
{
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id);
    if (id == null)
    {
        System.Diagnostics.Trace.TraceError("/Todos/Delete/ failed, ID is null");
        return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
    }
    Todo todo = db.Todoes.Find(id);
    if (todo == null)
    {
        System.Diagnostics.Trace.TraceWarning("/Todos/Delete/ failed, ID: " + id + " could not be found");
        return HttpNotFound();
    }
    System.Diagnostics.Trace.TraceInformation("GET /Todos/Delete/" + id + "completed successfully");
    return View(todo);
}
```
若要启用应用程序日志记录，请转到“监视” > “诊断日志”，然后使用切换开关启用应用程序日志记录。

![监视应用](media/app-service-web-tutorial-monitoring/app-service-monitor-applogs.png)

可将应用程序日志存储到 Web 应用的文件系统，或推送到 Blob 存储。 对于生产方案，我们建议使用 Blob 存储。

> [!IMPORTANT]
> 启用日志记录会对应用程序的性能和资源利用率造成影响。 对于生产方案，建议使用错误日志。 仅当需要调查问题时，才启用更详细的日志记录。

### Web 服务器诊断
<a id="web-server-diagnostics" class="xliff"></a>
即使未检测应用，也会生成 Web 服务器日志。 应用服务可以收集三种不同类型的服务器日志：

- **Web 服务器日志记录**
    - 有关使用 [W3C 扩展日志文件格式](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx)的 HTTP 事务的信息。
    - 确定总体站点指标（例如处理的请求数或来自特定 IP 地址的请求数）时，这些信息非常有用。
- **详细错误日志记录**
    - 指示故障的 HTTP 状态代码（状态代码 400 或更高）的详细错误消息。
    - [了解详细错误日志记录](https://www.iis.net/learn/troubleshoot/diagnosing-http-errors/how-to-use-http-detailed-errors-in-iis)
- **失败请求跟踪**
    - 有关失败请求的详细信息，包括对用于处理请求的 IIS 组件和每个组件所用的时间的跟踪。
    - 在尝试查找导致特定 HTTP 错误的问题时，失败请求跟踪很有用。
    - [详细了解失败请求跟踪](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)

若要启用服务器日志记录，请执行以下操作：
- 转到“监视” > “诊断日志”。
- 使用切换开关启用不同类型的 Web 服务器诊断。

![监视应用](media/app-service-web-tutorial-monitoring/app-service-monitor-serverlogs.png)

> [!IMPORTANT]
> 启用日志记录会对应用程序的性能和资源利用率造成影响。 对于生产方案，建议使用错误日志；仅当需要调查问题时，才启用更详细的日志记录。

### 访问日志
<a id="accessing-logs" class="xliff"></a>
使用 Azure 存储资源管理器访问 Blob 存储中存储的日志。 通过 FTP 访问 Web 应用的文件系统中存储的日志，路径如下：

- **应用程序日志** - `%HOME%/LogFiles/Application/`。
    - 此文件夹包含一个或多个包含应用程序日志记录生成的信息的文本文件。
- **失败请求跟踪** - `%HOME%/LogFiles/W3SVC#########/`。
    - 此文件夹包含一个 XSL 文件和一个或多个 XML 文件。
- **详细错误日志** - `%HOME%/LogFiles/DetailedErrors/`。
    - 此文件夹包含一个或多个 .htm 文件，其中提供了有关应用生成的 HTTP 错误的广泛信息。
- **Web 服务器日志** - `%HOME%/LogFiles/http/RawLogs`。
    - 此文件夹包含使用 W3C 扩展日志文件格式进行格式化的一个或多个文本文件。

## <a name="streaming"></a>步骤 4 - 日志流式传输
使用流式处理日志调试应用程序十分便利，因为相较于通过 FTP [访问日志](#Accessing-Logs)，它更节省时间。

生成**应用程序日志**和 **Web 服务器日志**后，应用服务可以流式传输这些日志。

> [!TIP]
> 在流式传输日志之前，请务必根据[日志记录](#logging)部分中所述启用日志收集。

若要流式处理日志，请转到“监视”> “日志流”。 根据要查找的信息，选择“应用程序日志”或“Web 服务器日志”。 在此处还可以暂停、重新启动和清除缓冲区。

![流式传输日志](media/app-service-web-tutorial-monitoring/app-service-monitor-logstream.png)

> [!TIP]
> 仅当应用中发生流量时才会生成日志；你也可以增大日志的详细程度，以获取更多事件或信息。

## <a name="remote"></a>步骤 5 - 远程调试
查明应用程序问题的起源后，请使用**远程调试**逐行排查代码。

使用远程调试可将调试程序附加到云中运行的 Web 应用。 可以设置断点、直接操作内存、逐行执行代码，甚至更改代码路径，就像在本地运行应用一样。

若要将调试器附加到云中运行的应用，请执行以下操作：

- 使用 Visual Studio 2017 打开要调试的应用的解决方案
- 像本地开发时一样，设置一些断点。
- 打开**云资源管理器**（Ctrl + /、Ctrl + X）。
- 根据需要使用 Azure 凭据登录。
- 找到要调试的应用
- 在“操作”窗格中选择“附加调试器”。

![远程调试](media/app-service-web-tutorial-monitoring/app-service-monitor-vsdebug.png)

Visual Studio 将为应用程序配置远程调试，并启动一个浏览器窗口用于导航到你的应用。 请浏览你的应用以触发断点并逐行执行代码。

> [!WARNING]
> 不建议在生产环境中以调试模式运行。 如果生产应用未扩展，无法容纳多个服务器实例，则调试会阻止 Web 服务器响应其他请求。 对于如何解决生产问题，最佳资源是[配置日志记录](#logging)和 [Application Insights](#insights)。

## <a name="explorer"></a>步骤 6 - 进程资源管理器
将应用程序扩展到多个实例后，**进程资源管理器**可帮助你识别实例特定的问题。

使用**进程资源管理器**可以：

- 枚举应用服务计划的不同实例中的所有进程。
- 钻取和查看与每个进程关联的句柄与模块。
- 在进程级别查看 CPU、工作集和线程计数，帮助识别失控的进程
- 查找打开的文件句柄，甚至可以终止特定的进程实例。

可以在“监视” > “进程资源管理器”下面找到进程资源管理器。

![进程资源管理器](media/app-service-web-tutorial-monitoring/app-service-monitor-processexplorer.png)
