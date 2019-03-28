---
title: Azure Functions 的缩放和托管 | Microsoft Docs
description: 了解如何在 Azure Functions 应用服务计划之间进行选择。
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure functions, functions, 应用服务计划, 事件处理, webhook, 动态计算, 无服务器体系结构
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
origin.date: 02/28/2019
ms.date: 03/20/2019
ms.author: v-junlch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9c9d2a48148e57625640810614cb5e1faf084df2
ms.sourcegitcommit: 5c73061b924d06efa98d562b5296c862ce737cc7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2019
ms.locfileid: "58256359"
---
# <a name="azure-functions-scale-and-hosting"></a>Azure Functions 的缩放和托管

可以在 Azure 应用服务计划中运行 Azure Functions。 如需详细了解如何使用应用服务计划，请参阅 [Azure 应用服务计划深入概述](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)。 


如果不熟悉 Azure Functions，请参阅 [Azure Functions 概述](functions-overview.md)。

创建函数应用时，要为应用中的函数选择托管计划。 Azure Functions 主机的实例在任一计划中均可执行函数。 计划类型可控制：

- 主机实例的扩展方式。
- 可供每个主机使用的资源。

> [!IMPORTANT]
> 创建函数应用时必须选择托管计划的类型。 之后不能再进行更改。

在应用服务计划中，可在不同的层之间进行缩放，从而分配不同数量的资源。
## <a name="app-service-plan"></a>应用服务计划

在专用的应用服务计划中，函数应用在基本、标准、高级或独立 SKU 中的专用 VM 上运行，这与其他应用服务应用相同。 会向函数应用分配专用 VM，这意味着函数主机可[始终运行](#always-on)。 应用服务计划支持 Linux。

对于以下情况，可以考虑使用应用服务计划：

- 具有已运行其他应用服务实例的、未充分利用的现成 VM。
- 函数应用持续或几乎持续运行。 在这种情况下，应用服务计划可能更经济高效。
- 需要仅对应用服务计划可用的功能，例如应用服务环境支持、VNET/VPN 连接性和更大的 VM。
- 想要在 Linux 上运行函数应用，或者想要提供要在其上运行函数的自定义映像。

VM 使得成本不再取决于执行数量、执行时间和所用内存。 因此，支付的费用不会超过分配的 VM 实例的费用。 如需详细了解如何使用应用服务计划，请参阅 [Azure 应用服务计划深入概述](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)。 

借助应用服务计划，可通过添加更多 VM 实例手动进行扩展，也可启用自动缩放。 有关详细信息，请参阅[手动或自动缩放实例计数](../monitoring-and-diagnostics/monitoring-autoscale-get-started.md)。 还可以通过选择不同的应用服务计划来进行增加。 有关详细信息，请参阅[增加 Azure 中的应用](../app-service/web-sites-scale.md)。 

在应用服务计划上运行 JavaScript 函数时，应选择具有较少 vCPU 的计划。 有关详细信息，请参阅[选择单核应用服务计划](functions-reference-node.md#choose-single-vcpu-app-service-plans)。  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 

###<a name="always-on"></a> Always On

如果在应用服务计划上运行，应启用 AlwaysOn 设置，使函数应用能正常运行。 在应用服务计划中，如果函数运行时处于不活动状态，几分钟后就会进入空闲状态，因此只有 HTTP 触发器才能“唤醒”函数。 只能对应用服务计划使用始终可用。 

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]

## <a name="what-is-my-hosting-plan"></a>我采用了哪种托管计划

要确定你的函数应用所使用的托管计划，请在 [Azure 门户](https://portal.azure.cn)中参阅函数应用的“概览”选项卡中的“应用服务计划/定价层”。 对于应用服务计划，还指明了定价层。 

![在门户中查看缩放计划](./media/functions-scale/function-app-overview-portal.png)

还可以使用 Azure CLI 来确定计划，如下所示：

```azurecli
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

即使启用了 AlwaysOn，各函数的执行超时也由 [host.json](functions-host-json.md#functiontimeout) 项目文件中的 `functionTimeout` 设置控制。

## <a name="storage-account-requirements"></a>存储帐户要求

在应用服务计划中，函数应用需要一个支持 Azure Blob、队列、文件和表存储的常规 Azure 存储帐户。 这是因为 Functions 依赖 Azure 存储来执行管理触发器和记录函数执行等操作，但某些存储帐户不支持队列和表。 这些帐户包括仅限 Blob 的存储帐户（包括高级存储），会在创建函数应用时从现有的“存储帐户”选项中过滤掉。

<!-- JH: Does using a Premium Storage account improve perf? -->

若要了解有关存储帐户类型的详细信息，请参阅 [Azure 存储服务简介](../storage/common/storage-introduction.md#azure-storage-services)。

<!-- Update_Description: wording update -->

