---
title: Azure 维护计划（预览版）| Microsoft Docs
description: 维护计划使客户能够围绕 Azure SQL 数据仓库服务用于推出新功能、升级和修补程序的必要计划性维护事件进行规划。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
origin.date: 10/06/2018
ms.date: 11/12/2018
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: c540067e1ae6e3f79977b07f6917a9d0c3c0f761
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52673040"
---
# <a name="view-a-maintenance-schedule"></a>查看维护计划 

## <a name="portal"></a>门户

默认情况下，所有新创建的 Azure SQL 数据仓库实例在部署期间会有八小时的主维护时段和辅助维护时段。 部署完成后，你可以更改此时段。 在未事先通知的情况下，不会在指定的维护时段外进行维护。

若要查看已应用于你的数据仓库的维护计划，请完成以下步骤：

1.  登录到 [Azure 门户](https://portal.azure.cn/)。
2.  选择要查看的数据仓库。 
3.  所选的数据仓库将在“概述”边栏选项卡上打开。 应用于所选数据仓库的维护计划将显示在“维护计划(预览版)”下方。

![概述边栏选项卡](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="next-steps"></a>后续步骤
[详细了解](https://docs.microsoft.com/azure/service-health/service-health-overview) Azure 服务运行状况。


