---
title: "REST API：管理 Azure SQL 数据库审核 | Azure"
description: "使用 REST API 配置 Azure SQL 数据库审核，以跟踪数据库事件并将其写入 Azure 存储帐户中的审核日志。"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: giladm
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: secure and protect
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/05/2016
ms.date: 03/24/2017
ms.author: v-johch
ms.openlocfilehash: d32f7fc6ca6c9765a938e47d88461c099fa9f1e8
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="configure-and-manage-sql-database-auditing-using-the-rest-api"></a>使用 REST API 配置和管理 SQL 数据库审核

本主题介绍如何使用 REST API 配置和管理审核。 若要使用 Azure 门户配置和管理审核，请参阅[在 Azure 门户中配置审核](./sql-database-auditing-portal.md)。 若要使用 PowerShell 配置和管理审核，请参阅[在 PowerShell 中配置审核](./sql-database-auditing-powershell.md)。

有关审核的概述，请参阅 [SQL 数据库审核](./sql-database-auditing.md)。

## <a name="rest-api---blob-auditing"></a>REST API - Blob 审核

   * [创建或更新数据库 Blob 审核策略](https://msdn.microsoft.com/zh-cn/library/azure/mt695939.aspx)
   * [创建或更新服务器 Blob 审核策略](https://msdn.microsoft.com/zh-cn/library/azure/mt771861.aspx)
   * [获取数据库 Blob 审核策略](https://msdn.microsoft.com/zh-cn/library/azure/mt695938.aspx)
   * [获取服务器 Blob 审核策略](https://msdn.microsoft.com/zh-cn/library/azure/mt771860.aspx)
   * [获取服务器 Blob 审核操作结果](https://msdn.microsoft.com/zh-cn/library/azure/mt771862.aspx)

## <a name="rest-api---table-auditing"></a>REST API - 表审核

   * [创建或更新数据库审核策略](https://msdn.microsoft.com/zh-cn/library/azure/mt604471.aspx)
   * [创建或更新服务器审核策略](https://msdn.microsoft.com/zh-cn/library/azure/mt604383.aspx)
   * [获取数据库审核策略](https://msdn.microsoft.com/zh-cn/library/azure/mt604381.aspx)
   * [获取服务器审核策略](https://msdn.microsoft.com/zh-cn/library/azure/mt604382.aspx)

## <a name="next-steps"></a>后续步骤

* 若要使用 Azure 门户配置和管理审核，请参阅[在 Azure 门户中配置数据库审核](./sql-database-auditing-portal.md)。 
* 若要使用 PowerShell 配置和管理审核，请参阅[使用 PowerShell 配置数据库审核](./sql-database-auditing-powershell.md)。
* 有关审核的概述，请参阅[数据库审核](./sql-database-auditing.md)。