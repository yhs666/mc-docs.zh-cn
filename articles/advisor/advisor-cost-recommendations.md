---
title: 使用 Azure 顾问降低服务成本 | Microsoft Docs
description: 使用 Azure 顾问优化 Azure 部署的成本。
services: advisor
documentationcenter: NA
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 01/29/2019
ms.author: lingliw
ms.openlocfilehash: 3ff2dab907b6ebf4e0c043477b37e801ed69b0df
ms.sourcegitcommit: c5646ca7d1b4b19c2cb9136ce8c887e7fcf3a990
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2019
ms.locfileid: "57964456"
---
# <a name="reduce-service-costs-using-azure-advisor"></a>使用 Azure 顾问降低服务成本

通过识别闲置和未充分利用的资源，顾问有助于优化和降低 Azure 总支出。 可在顾问仪表板的“成本”选项卡获取成本建议。

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>通过调整或关闭未充分利用的实例来优化虚拟机花费 

虽然某些应用程序方案有意使虚拟机利用率较低，但通过管理虚拟机大小和数量通常可降低成本。 顾问可监视虚拟机 7 天的使用情况，并识别出利用率较低的虚拟机。 以下情况可以将虚拟机视为使用率低：其 CPU 使用率为 5% 或更低且其网络使用率低于 2%，或者更小的虚拟机大小可以容纳当前工作负荷。

顾问会显示继续运行虚拟机的预估成本，以便你选择关闭它还是对其进行调整。

若要加强对低使用率虚拟机的标识，可在每个订阅的基础上调整平均 CPU 使用率。

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>如何访问 Azure 顾问中的成本建议

1. 登录 [Azure 门户](https://portal.azure.cn)，并打开[顾问](https://portal.azure.cn/#blade/Microsoft_Azure_Expert/AdvisorBlade)。

2.  在顾问仪表板中，单击“成本”选项卡。

## <a name="next-steps"></a>后续步骤

若要了解有关顾问建议的详细信息，请参阅以下资源：
* [顾问简介](advisor-overview.md)
* [入门](advisor-get-started.md)
* [顾问性能建议](advisor-cost-recommendations.md)
* [顾问高可用性建议](advisor-cost-recommendations.md)
