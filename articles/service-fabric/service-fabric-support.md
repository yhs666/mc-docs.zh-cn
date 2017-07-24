---
title: "了解 Azure Service Fabric 支持选项 | Azure"
description: "支持的 Azure Service Fabric 群集版本，以及文件支持票证的链接"
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 06/15/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: e23edd62cd564be0653eea72714598e169a80422
ms.sourcegitcommit: f2f4389152bed7e17371546ddbe1e52c21c0686a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# <a name="azure-service-fabric-support-options"></a>Azure Service Fabric 支持选项

我们为用户设置了各种选项，方便其为 Service Fabric 群集（在其上运行应用程序工作负荷）提供相应的支持。 用户需根据所需支持级别以及问题的严重性，选取适当的选项。 

<a id="getlivesitesupportonazure"></a>

## <a name="report-production-or-live-site-issues-or-request-paid-support-for-azure"></a>报告生产或实时站点问题，或者请求 Azure 付费支持

若要报告部署在 Azure 上的 Service Fabric 群集的实时站点问题，请通过 [Azure 门户](https://www.azure.cn/support/support-ticket-form)或 [Microsoft 支持门户](http://support.microsoft.com/oas/default.aspx?prid=16146)开具专业支持票证。

了解有关以下方面的详细信息：

- [Microsoft 提供的 Azure 专业支持](https://www.azure.cn/support/plans/?b=16.44)。
- [Microsoft 顶级支持](https://support.microsoft.com/premier)。

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-or-live-site-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>报告生产或实时站点问题，或者请求独立 Service Fabric 群集的付费支持

若要报告部署在本地或其他云上的 Service Fabric 群集的实时站点问题，请通过 [Microsoft 支持门户](http://support.microsoft.com/oas/default.aspx?prid=16146)开具专业支持票证。

了解有关以下方面的详细信息：

- [Microsoft 提供的本地专业支持](https://support.microsoft.com/gp/offerprophone?wa=wsignin1.0)。
- [Microsoft 顶级支持](https://support.microsoft.com/premier)。

<a id="getsupportonissues"></a>

## <a name="report-azure-service-fabric-issues"></a>报告 Azure Service Fabric 问题

我们已设置 GitHub 存储库，用于报告 Service Fabric 问题。  我们还积极监视以下论坛。

### <a name="github-repo"></a>GitHub 存储库

在 [Service-Fabric-issues git 存储库](https://github.com/Azure/service-fabric-issues)中报告 Azure Service Fabric 问题。 此存储库用于报告和跟踪 Azure Service Fabric 问题，以及进行小型功能请求。 **请勿使用此存储库报告实时站点问题**。

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow 和 MSDN 论坛

[StackOverflow 上的 Service Fabric 标记][stackoverflow]和 [MSDN 上的 Service Fabric 论坛][msdn-forum]最适合提问有关平台工作方式以及如何通过该平台完成某些任务的问题。

### <a name="azure-feedback-forum"></a>Azure 反馈论坛

[有关 Service Fabric 的 Azure 反馈论坛][uservoice-forum]最适合用户提交关于产品的大型功能创意，我们可以看到，大多数常见的请求都属于我们的中长期规划。 我们鼓励你在社区内争取大家对你的建议的支持。

<a id="releasesuport"></a>

## <a name="supported-service-fabric-versions"></a>支持的 Service Fabric 版本

请确保群集始终运行受支持的 Service Fabric 版本。 当我们宣布发行新版 Service Fabric 时，以前的版本标记为自发布日期起至少 60 天后结束支持。 新版本在 [Service Fabric 团队博客](https://blogs.msdn.microsoft.com/azureservicefabric/)中公布。

有关如何使群集保持运行受支持的 Service Fabric 版本的详细信息，请参阅以下文档。

- [在 Azure 群集上升级 Service Fabric 版本](service-fabric-cluster-upgrade.md)
- [在单独的 Windows Server 群集上升级 Service Fabric 版本](service-fabric-cluster-upgrade-windows-server.md)

下面是支持的 Service Fabric 版本列表和这些版本的支持结束日期。

| **Service Fabric 运行时群集** | **兼容的 SDK/NuGet 包版本** | **支持结束日期** |
| --- | --- | --- |
| 5.3.121 之前的所有群集版本 |低于或等于版本 2.3 |2017 年 1 月 20 日 |
| 5.3.* |低于或等于版本 2.3 |2017 年 2 月 24 日 |
| 5.4.* |低于或等于版本 2.4 |2017 年 5 月 10 日     |
| 5.5.* |低于或等于版本 2.5 |2017 年 8 月 10 日    |
| 5.6.* |低于或等于版本 2.6 |当前版本，因此无结束日期

<a id="previewversion"></a>

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Service Fabric 预览版本 - 不支持在生产环境中使用

我们会不时发布包含重要功能的版本，希望用户对这些功能提供反馈，这些版本将作为预览版发布。 这些预览版本应仅用于测试目的。 生产群集应始终运行支持的稳定 Service Fabric 版本。 预览版本始终以主版本号和次版本号 255 开头。 例如，如果看到 Service Fabric 版本 255.255.5703.949，则该版本应仅在测试群集中使用且处于预览状态。 这些预览版本也在 [Service Fabric 团队博客](https://blogs.msdn.microsoft.com/azureservicefabric)上公布，并将提供有关包含的功能的详细信息。

这些预览版本没有付费的支持选项。 使用[报告 Azure Service Fabric 问题](/service-fabric/service-fabric-support#report-azure-service-fabric-issues)下列出的选项之一提出问题或提供反馈。

## <a name="next-steps"></a>后续步骤

- [在 Azure 群集上升级 Service Fabric 版本](service-fabric-cluster-upgrade.md)
- [在单独的 Windows Server 群集上升级 Service Fabric 版本](service-fabric-cluster-upgrade-windows-server.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: http://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: http://aka.ms/servicefabricdocs
[sample-repos]: http://aka.ms/servicefabricsamples