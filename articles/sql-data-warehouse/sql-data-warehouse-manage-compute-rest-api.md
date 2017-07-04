---
title: "在 Azure SQL 数据仓库中使用 REST 进行暂停、恢复和缩放 | Azure"
description: "用于管理计算能力的 PowerShell 任务。 通过调整 DWU 缩放计算资源。 或者，暂停和恢复计算资源来节省成本。"
services: sql-data-warehouse
documentationcenter: NA
author: rockboyfor
manager: barbkess
editor: 
ms.assetid: 21de7337-9356-49bb-a6eb-06c1beeba2c4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
origin.date: 10/31/2016
ms.date: 05/08/2017
ms.author: v-yeche
ms.openlocfilehash: ae017d0bf3e174abc44d9f0906c7b52209abd2ac
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>管理 Azure SQL 数据仓库中的计算能力 (REST)

> [!div class="op_single_selector"]
> * [概述](sql-data-warehouse-manage-compute-overview.md)
> * [门户](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

## <a name="scale-performance-bk"></a> <a name="scale-compute-bk"></a>缩放计算能力

[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

若要更改 DWU，请使用 [创建或更新数据库][Create or Update Database] REST API。 以下示例将托管在服务器 MyServer 上的数据库 MySQLDW 的服务级别目标设置为 DW1000。 该服务器位于名为 ResourceGroup1 的 Azure 资源组中。

```
PUT https://management.chinacloudapi.cn/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

## <a name="pause-compute-bk"></a>暂停计算

[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

若要暂停数据库，请使用 [暂停数据库][Pause Database] REST API。 以下示例将暂停 Server01 服务器上托管的 Database02 数据库。 该服务器位于名为 ResourceGroup1 的 Azure 资源组中。

```
POST https://management.chinacloudapi.cn/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="resume-compute-bk"></a>恢复计算

[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

若要启动数据库，请使用 [恢复数据库][Resume Database] REST API。 以下示例将启动 Server01 服务器上托管的 Database02 数据库。 该服务器位于名为 ResourceGroup1 的 Azure 资源组中。 

```
POST https://management.chinacloudapi.cn/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>检查数据库状态

```json
GET https://management.chinacloudapi.cn/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Sql/servers/{serverName}/databases/{databaseName}?api-version=2014-04-01 HTTP/1.1
```

## <a name="next-steps-bk"></a>后续步骤
有关其他管理任务，请参阅 [管理概述][Management overview]。

<!--Image references-->

<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Pause Database]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Resume Database]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Create or Update Database]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.cn/