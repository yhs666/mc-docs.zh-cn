---
title: Azure Functions 的缩放和托管 | Microsoft Docs
description: 选择 Azure Functions 应用服务计划。
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: Azure Functions, Functions, 应用服务计划, 事件处理, webhook, 动态计算, 无服务体系结构
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
origin.date: 06/05/2018
ms.date: 07/23/2018
ms.author: v-junlch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bdae2e7b03282863406cb9d2766030e845612915
ms.sourcegitcommit: ba07d76f8394b5dad782fd983718a8ba49a9deb2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2018
ms.locfileid: "39220192"
---
# <a name="azure-functions-scale-and-hosting"></a>Azure Functions 的缩放和托管

可以在 Azure 应用服务计划中运行 Azure Functions。 如需详细了解如何使用应用服务计划，请参阅 [Azure 应用服务计划深入概述](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)。 


如果不熟悉 Azure Functions，请参阅 [Azure Functions 概述](functions-overview.md)。

创建函数应用时，必须为应用中包含的函数配置托管计划。 在任一模式下，Azure Functions 主机实例均可执行函数。 计划类型可控制：

- 主机实例的扩展方式。
- 可供每个主机使用的资源。

目前，创建函数应用时必须选择宿主计划的类型。 之后不能再进行更改。 

在应用服务计划中，可以在不同的层之间进行缩放以分配不同数量的资源。 

## <a name="app-service-plan"></a>应用服务计划

在应用服务计划中，函数应用在基本、标准、高级或独立 SKU 中的专用 VM 上运行，类似于 Web 应用、API 应用和移动应用。 专用 VM 将分配到应用服务应用，这意味着函数主机始终会运行。 应用服务计划支持 Linux。

对于以下情况，可以考虑使用应用服务计划：
- 具有已运行其他应用服务实例的、未充分利用的现成 VM。
- 预期函数应用会持续或几乎持续运行。 在这种情况下，应用服务计划可能更经济高效。
- 需要仅对应用服务计划可用的功能，例如应用服务环境支持、VNET/VPN 连接性和更大的 VM。 
- 想要在 Linux 上运行函数应用，或者想要提供要在其上运行函数的自定义映像。

VM 使得成本不再取决于执行数量、执行时间和所用内存。 因此，支付的费用不会超过分配的 VM 实例的费用。 如需详细了解如何使用应用服务计划，请参阅 [Azure 应用服务计划深入概述](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)。 

借助应用服务计划，可通过添加更多 VM 实例手动进行扩展，也可启用自动缩放。 有关详细信息，请参阅[手动或自动缩放实例计数](../monitoring-and-diagnostics/insights-how-to-scale.md)。 还可以通过选择不同的应用服务计划来进行增加。 有关详细信息，请参阅[增加 Azure 中的应用](../app-service/web-sites-scale.md)。 

如果计划在应用服务计划上运行 JavaScript 函数，则应选择具有较少 vCPU 的计划。 有关详细信息，请参阅[选择单核应用服务计划](functions-reference-node.md#considerations-for-javascript-functions)。  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
### <a name="always-on"></a>Always On

如果在应用服务计划上运行，应该启用“Always On”设置，使函数应用能正常运行。 在应用服务计划中，若函数运行时处于不活动状态，几分钟后就会进入空闲状态，因此只有 HTTP 触发器才能“唤醒”函数。 必须为 Web 作业启用 Always On 的原因与此类似。 

## <a name="storage-account-requirements"></a>存储帐户要求

在应用服务计划中，函数应用都需要一个支持 Azure Blob、队列、文件和表存储的常规 Azure 存储帐户。 Azure Functions 在内部使用 Azure 存储，执行管理触发器和记录函数执行等操作。 某些存储帐户不支持队列和表，例如仅限 blob 的存储帐户（包括高级存储）和使用区域冗余存储空间复制的常规用途存储帐户。 创建函数应用时，将在“存储帐户”边栏选项卡中筛选这些帐户。

<!-- JH: Does using a PRemium Storage account improve perf? -->

若要了解有关存储帐户类型的详细信息，请参阅 [Azure 存储服务简介](../storage/common/storage-introduction.md#azure-storage-services)。

<!-- Update_Description: update metedata properties -->