---
title: Azure 顾问性能建议 | Microsoft Docs
description: 使用顾问优化 Azure 部署的性能。
services: advisor
documentationcenter: NA
author: forester123
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 11/16/2016
ms.date: 03/26/2018
ms.author: v-johch
ms.openlocfilehash: a68755a07268d465ec3bc6ab3e27ce257a029168
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52664116"
---
# <a name="advisor-performance-recommendations"></a>顾问性能建议

Azure 顾问性能建议有助于提高关键业务应用程序的速度和响应能力。 可通过顾问从顾问仪表板的“性能”选项卡获取性能建议。

## <a name="improve-database-performance-with-sql-db-advisor"></a>通过 SQL DB 顾问提高数据库性能

顾问针对所有 Azure 资源提供一个一致且统一的建议视图。 它与 SQL 数据库顾问集成，提供建议以改进 SQL Azure 数据库性能。 SQL 数据库顾问通过分析使用情况历史记录来评估 SQL Azure 数据库的性能。 提供的建议最适合运行数据库典型工作负荷。 

> [!NOTE]
> 若要获取建议，数据库必须具有一周左右的使用量，且该周内必须有一些一致的活动。 SQL 数据库顾问优化一致的查询模式比优化随机的突发活动更加轻松。

有关 SQL 数据库顾问的详细信息，请参阅 [SQL 数据库顾问](/sql-database/sql-database-advisor)。

## <a name="how-to-access-performance-recommendations-in-advisor"></a>如何访问顾问中的性能建议

1. 登录 [Azure 门户](https://portal.azure.cn)，并打开[顾问](https://portal.azure.cn/#blade/Microsoft_Azure_Expert/AdvisorBlade)。

2.  在顾问仪表板中，单击“性能”选项卡。

## <a name="next-steps"></a>后续步骤

若要了解有关顾问建议的详细信息，请参阅以下资源：

* [顾问简介](advisor-overview.md)
* [顾问入门](advisor-get-started.md)
* [顾问成本建议](advisor-cost-recommendations.md)
* [顾问高可用性建议](advisor-high-availability-recommendations.md)

