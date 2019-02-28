---
title: 使用 Azure Application Insights 为 ASP.NET 设置 Web 应用分析 | Microsoft 文档
description: 为托管在本地或 Azure 中的 ASP.NET 网站配置性能、可用性和用户行为分析工具。
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: mbullwin
ms.openlocfilehash: 1560fddb535c07dbfd9703bd97409a94d36a6805
ms.sourcegitcommit: 7e25a709734f03f46418ebda2c22e029e22d2c64
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2019
ms.locfileid: "56440555"
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>为 ASP.NET 网站设置 Application Insights

它适用于托管在你自己的本地或云中 IIS 服务器上的 ASP.NET 应用。 你会获得图表和功能强大的查询语言，这有助于你了解应用的性能以及用户使用它的方式。另外，在出现故障或性能问题时还会自动发送警报。 许多开发人员发现这些功能很有用，不过你也可以根据需要扩展和自定义遥测。

在 Visual Studio 中单击几下即可完成安装。 可以选择对遥测量进行限制，从而避免付费。 可以通过此功能对用户不多的站点进行试验和调试，或者对其进行监视。 如果认为需要前进一步，对生产站点进行监视，则可轻松地在以后提高该限制。

## <a name="prerequisites"></a>先决条件
若要要将 Application Insights 添加到 ASP.NET 网站，需要：

- 使用以下工作负荷安装[用于 Windows 的 Visual Studio 2017](https://www.visualstudio.com/downloads/)：
    - ASP.NET 和 Web 开发
    - Azure 开发

如果没有 Azure 订阅，请在开始之前创建一个[免费](https://azure.microsoft.com/free/)帐户。

## <a name="ide"></a> 步骤 1：添加 Application Insights SDK

> [!IMPORTANT]
> 添加 Application Insights 的过程因 ASP.NET 模板类型而异。 若要使用“空”或“Azure 移动应用”模板，请选择“项目” > “添加 Application Insights 遥测”。 对于所有其他的 ASP.NET 模板，请参阅以下说明。 

在解决方案资源管理器中右键单击 Web 应用的名称，并选择“配置 Application Insights”

![解决方案资源管理器的屏幕截图，其中突出显示了“配置 Application Insights”](./media/asp-net/0001-configure-application-insights.png)

（根据所用的 Application Insights SDK 版本，系统可能会提示升级到最新的 SDK 版本。 如果出现提示，请选择“更新 SDK”。）

![屏幕截图：新版的 Microsoft Application Insights SDK 可用。 突出显示了“更新 SDK”](./media/asp-net/0002-update-sdk.png)

Application Insights 配置屏幕：

选择“免费开始”。

![使用 Application Insights 页注册应用的屏幕截图](./media/asp-net/0004-start-free.png)

如果想要设置用于存储数据的资源组或位置，请单击“配置设置”。 资源组用于控制对数据的访问。 例如，如果有多个应用构成了同一个系统的一部分，可在同一个资源组中放置这些应用的 Application Insights 数据。

 选择“注册”。 

![使用 Application Insights 页注册应用的屏幕截图](./media/asp-net/0005-register-ed.png)

 在调试期间以及发布应用后，遥测数据将发送到 [Azure 门户](https://portal.azure.com)。
> [!NOTE]
> 如果不希望在进行调试时向门户发送遥测，则请直接向应用添加 Application Insights SDK，但不要在门户中配置资源。 在调试时，可以在 Visual Studio 中查看遥测数据。 可以稍后返回到此配置页，也可以等待应用部署完成，然后在运行时打开遥测。

## <a name="run"></a> 步骤 2：运行应用程序
使用 F5 运行应用。 打开不同的页以生成一些遥测数据。

Visual Studio 中会显示已记录的事件数。

![Visual Studio 的屏幕截图。 调试期间会显示“Application Insights”按钮。](./media/asp-net/0006-Events.png)

## <a name="step-3-see-your-telemetry"></a>步骤 3：查看遥测
可在 Visual Studio 或 Application Insights Web 门户中查看遥测数据。 在 Visual Studio 中搜索遥测，以便对应用进行调试。 当系统运行时，在 Web 门户中监视性能和使用情况。 

### <a name="see-your-telemetry-in-visual-studio"></a>在 Visual Studio 中查看遥测

在 Visual Studio 中查看 Application Insights 数据。  选择“解决方案资源管理器” > “连接的服务”，右键单击“Application Insights”，然后单击“搜索实时遥测”。

在 Visual Studio Application Insights 搜索窗口中，可以看到应用程序的服务器端生成的遥测数据。 使用这些筛选器进行试验，并单击任何事件以查看更多详细信息。

![Application Insights 窗口中“来自调试会话的数据”视图的屏幕截图。](./media/asp-net/55.png)

> [!Tip]
> 如果看不到任何数据，请确保时间范围正确，并单击“搜索”图标。

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>在 Wb 门户中查看遥测

还可以在 Application Insights Web 门户中查看遥测（除非选择仅安装 SDK）。 该门户中的图表、分析工具和跨组件视图比 Visual Studio 中的多。 此门户还提供警报。

打开 Application Insights 资源。 登录到 [Azure 门户](https://portal.azure.com/)并在其中找到所需的资源，或者选择“解决方案资源管理器” > “连接的服务”，然后右键单击“Application Insights”并选择“打开 Application Insights 门户”，以转到相应的资源。 > 

门户将从应用打开遥测视图。

![Application Insights 概述页的屏幕截图](./media/asp-net/66.png)

在门户中，单击任何磁贴或图表以查看更多详细信息。

## <a name="step-4-publish-your-app"></a>步骤 4：发布应用
将应用发布到 IIS 服务器或 Azure。 监视实时指标流，确保一切平稳运行。

遥测数据会在 Application Insights 门户中累积，可在此监视指标、搜索遥测数据以及设置仪表板。 还可以使用功能强大的 [Kusto 查询语言](/azure/kusto/query/)来分析使用情况和性能，或查找特定的事件。

> [!NOTE]
> 如果应用发送的遥测足够达到限制，自动采样会打开。 采样可以减少从应用发送的遥测数量，同时为诊断保留相关数据。
>
>

## <a name="land"></a> 已经完成全部设置

祝贺！ 已在应用中安装 Application Insights 包，并将其配置为向 Azure 中的 Application Insights 服务发送遥测。

![遥测数据移动示意图](./media/asp-net/01-scheme.png)

接收应用遥测的 Azure 资源通过“检测密钥”进行标识。 可以在 ApplicationInsights.config 文件中找到该密钥。


## <a name="upgrade-to-future-sdk-versions"></a>升级到更高的 SDK 版本
若要升级到 [SDK 的新版本](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases)，请打开 **NuGet 包管理器**，并筛选已安装的包。 选择“Microsoft.ApplicationInsights.Web”，并选择“升级”。

如果对 ApplicationInsights.config 执行了任何自定义操作，请在升级前保存相关副本。 然后，将更改合并到新版本中。

