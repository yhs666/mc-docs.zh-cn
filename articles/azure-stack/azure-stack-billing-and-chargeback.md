---
title: "Azure Stack 中的客户计费和退款 | Microsoft Docs"
description: "了解如何从 Azure Stack 中检索资源使用情况信息。"
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: a9afc7b6-43da-437b-a853-aab73ff14113
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/28/2017
ms.date: 03/01/2018
ms.author: v-junlch
ms.openlocfilehash: b43dcc6b1f05a9f11f6068aa9f296e38d7e90047
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="usage-and-billing-in-azure-stack"></a>Azure Stack 中的使用情况和计费

使用情况表示用户消耗的资源数量。 Azure Stack 收集每个用户的使用情况信息并使用该信息来为用户计费。 本文介绍了如何根据资源使用情况对 Azure Stack 用户进行计费，以及如何访问计费信息以进行分析、退款，等等。

Azure Stack 包含的基础结构可以收集和聚合所有资源的使用情况数据，并将此数据转发到 Azure Commerce。 可以使用计费适配器访问此数据并将其导出到计费系统，或将其导出到诸如 Power BI 的商业智能工具。 在导出后，可以使用此计费信息进行分析或者将其传输到退款系统。

![将 Azure Stack 连接到计费应用程序的计费适配器的概念模型](./media/azure-stack-billing-and-chargeback/image1.png)

## <a name="usage-pipeline"></a>使用情况管道

Azure Stack 中的每个资源提供程序都按照资源利用率来发布使用情况数据。 使用情况服务定期（每小时或每天）聚合此使用情况数据并将其存储在使用情况数据库中。 Azure Stack 操作员和用户可以使用“使用情况 API”在本地访问所存储的使用情况数据。 

如果已[向 Azure 注册了 Azure Stack 实例](azure-stack-register.md)，则使用情况桥配置为将使用情况数据发送到 Azure Commerce。 当数据在 Azure 中可用后，可以通过计费门户或使用 Azure 使用情况 API 访问该数据。 若要详细了解哪些使用情况数据会被报告到 Azure，请参阅[使用情况数据报告](azure-stack-usage-reporting.md)主题。 

下图显示了使用情况管道中的关键组件：

![使用情况管道](./media/azure-stack-billing-and-chargeback/usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>可以找到哪些使用情况信息，如何查找？

Azure Stack 资源提供程序（例如计算、存储和网络）每隔一小时为每个订阅生成使用情况数据。 使用情况数据包含有关所使用资源的信息，例如资源名称、所用订阅、所用数量，等等。若要了解计量 ID 资源，请参阅[使用情况 API 常见问题解答](azure-stack-usage-related-faq.md)一文。 

在收集使用情况数据后，它将[报告给 Azure](azure-stack-usage-reporting.md)来生成帐单，可以通过 Azure 计费门户查看账单。 

> [!NOTE]
> 对于 Azure Stack 开发工具包和在容量模型下许可的 Azure Stack 集成系统用户，不需要进行使用情况数据报告。 若要详细了解 Azure Stack 中的许可，请参阅[打包和定价](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf)数据表。

Azure 计费门户仅显示应计费资源的使用情况数据。 除了应计费资源之外，Azure Stack 还会捕获更广范围内资源的使用情况数据，可以通过 REST API 或 PowerShell 在 Azure Stack 环境中访问这些数据。 Azure Stack 操作员可以检索所有用户订阅的使用情况数据，而用户只能获取其自己的使用情况详细信息。

## <a name="retrieve-usage-information"></a>检索使用情况信息

若要生成使用情况数据，你应当具有正在运行且在主动使用系统的资源，例如，活动虚拟机或包含某些数据的存储帐户，等等。如果不确定你是否有任何资源在 Azure Stack Marketplace 中运行，请部署一个虚拟机 (VM)，并验证 VM 监视边栏选项卡以确保它正在运行。 使用以下 PowerShell cmdlet 来查看使用情况数据：

1. [安装适用于 Azure Stack 的 PowerShell。](azure-stack-powershell-install.md)
2. [配置 Azure Stack 用户的](user/azure-stack-powershell-configure-user.md)或 [Azure Stack 操作员的](azure-stack-powershell-configure-admin.md) PowerShell 环境 

3. 若要检索使用情况数据，请使用 [Get-UsageAggregates](https://docs.microsoft.com/powershell/module/azurerm.usageaggregates/get-usageaggregates) PowerShell cmdlet：

   ```powershell
   Get-UsageAggregates -ReportedStartTime "<Start time for usage reporting>" -ReportedEndTime "<end time for usage reporting>" -AggregationGranularity <Hourly or Daily>
   ```

## <a name="next-steps"></a>后续步骤

[向 Azure 报告 Azure Stack 使用情况数据](azure-stack-usage-reporting.md)

[提供程序资源使用情况 API](azure-stack-provider-resource-api.md)

[租户资源使用情况 API](azure-stack-tenant-resource-usage-api.md)

[有关使用情况的常见问题解答](azure-stack-usage-related-faq.md)


