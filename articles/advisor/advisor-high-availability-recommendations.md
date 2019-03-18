---
title: 使用 Azure 顾问提高应用程序的可用性 | Microsoft Docs
description: 使用 Azure 顾问提高 Azure 部署的高可用性。
services: advisor
documentationcenter: NA
author: kasparks
ms.author: lingliw
ms.service: advisor
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: 33dc5415c02f8c52ab564ddfb59707a0eeb2ccb5
ms.sourcegitcommit: c5646ca7d1b4b19c2cb9136ce8c887e7fcf3a990
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2019
ms.locfileid: "57964461"
---
# <a name="improve-availability-of-your-application-with-azure-advisor"></a>使用 Azure 顾问提高应用程序的可用性

Azure 顾问可帮助确保并提高业务关键应用程序的连续性。 可以通过顾问仪表板的“高可用性”选项卡获取顾问的高可用性建议。

## <a name="ensure-virtual-machine-fault-tolerance"></a>确保虚拟机容错

要为应用程序提供冗余，建议你将两个或更多虚拟机组合到一个可用性集中。 顾问标识不属于可用性集的虚拟机，并建议将它们移动到可用性集中。 这种配置可以确保在计划内或计划外维护事件期间，至少有一个虚拟机可用，并满足 Azure 虚拟机 SLA 要求。 可以选择为虚拟机创建可用性集，或将虚拟机添加到现有可用性集。

> [!NOTE]
> 如果选择创建可用性集，则必须至少再向其中添加一台虚拟机。 建议在可用性集中对两个或更多虚拟机进行分组，确保其中一台虚拟机在出现故障期间可用。

## <a name="ensure-availability-set-fault-tolerance"></a>确保可用性集容错

要为应用程序提供冗余，建议你将两个或更多虚拟机组合到一个可用性集中。 顾问标识包含单个虚拟机的可用性集，并建议向其中添加一个或多个虚拟机。 这种配置可以确保在计划内或计划外维护事件期间，至少有一个虚拟机可用，并满足 Azure 虚拟机 SLA 要求。 可以选择创建虚拟机，或将现有的虚拟机添加到可用性集。  

## <a name="use-managed-disks-to-improve-data-reliability"></a>使用托管磁盘提高数据可靠性

具有共享存储帐户或存储缩放单元的磁盘的可用性集中的虚拟机在中断期间不可对单个存储规模单元故障进行复原。 顾问将确定这些可用性集，并建议迁移到 Azure 托管磁盘。 这将确保可用性集中的不同虚拟机的磁盘彼此完全独立，以避免单点故障。 

## <a name="ensure-application-gateway-fault-tolerance"></a>确保应用程序网关容错
为了确保由应用程序网关提供支持的任务关键型应用程序的业务连续性，顾问会标识没有针对容错进行配置的应用程序网关实例，并建议可以执行的修正操作。 顾问会标识中型或大型单实例应用程序网关，并建议至少再添加一个实例。 它还标识单实例或多实例小型应用程序网关，并建议迁移到中型或大型 SKU。 顾问建议执行这些操作以确保应用程序网关实例配置为满足这些资源的当前 SLA 要求。

此建议可确保由应用程序网关提供支持的任务关键型应用程序的业务连续性。 顾问会标识未针对容错进行配置的应用程序网关实例，并且会建议可以执行的修正操作。 顾问会标识中型或大型单实例应用程序网关，并建议至少再添加一个实例。 它还标识单实例或多实例小型应用程序网关，并建议迁移到中型或大型 SKU。 顾问建议执行这些操作以确保应用程序网关实例配置为满足这些资源的当前 SLA 要求。

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>防止意外删除虚拟机数据

设置虚拟机备份可确保业务关键型数据的可用性，并防止意外删除或损坏。 顾问标识其中未启用备份的虚拟机，并建议启用备份。 

## <a name="how-to-access-high-availability-recommendations-in-advisor"></a>如何访问顾问中的高可用性建议

1. 登录 [Azure 门户](https://portal.azure.cn)，并打开[顾问](https://portal.azure.cn/#blade/Microsoft_Azure_Expert/AdvisorBlade)。

2.  在顾问仪表板中，单击“高可用性”选项卡。

## <a name="next-steps"></a>后续步骤

有关顾问建议的详细信息，请参阅以下资源：
* [Azure 顾问简介](advisor-overview.md)
* [顾问入门](advisor-get-started.md)
* [顾问成本建议](advisor-cost-recommendations.md)
* [顾问性能建议](advisor-performance-recommendations.md)

