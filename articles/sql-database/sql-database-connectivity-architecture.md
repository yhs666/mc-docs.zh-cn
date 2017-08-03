---
title: "Azure SQL 数据库连接体系结构 | Azure"
description: "本文档从 Azure 内或 Azure 外介绍 Azure SQLDB 连接体系结构。"
services: sql-database
documentationcenter: 
author: Hayley244
manager: digimobile
editor: monicar
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
origin.date: 06/05/2017
ms.date: 07/31/2017
ms.author: v-haiqya
ms.openlocfilehash: 2735d2dc5adabce23472fd4a064a495663a4da49
ms.sourcegitcommit: 2e85ecef03893abe8d3536dc390b187ddf40421f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2017
---
# <a name="azure-sql-database-connectivity-architecture"></a>Azure SQL 数据库连接体系结构 

本文介绍 Azure SQL Database 连接体系结构，并说明不同组件如何将流量定向到 Azure SQL 数据库的实例。 这些 Azure SQL 数据库连接组件通过从 Azure 内连接的客户端以及从 Azure 外连接的客户端将网络流量定向到 Azure 数据库。 本文还提供脚本示例（用于更改连接发生的方式）以及与更改默认连接设置相关的注意事项。 如果阅读本文后有任何问题，请通过 dmalik@microsoft.com 与 Dhruv 联系。 

## <a name="connectivity-architecture"></a>连接体系结构

下图提供 Azure SQL 数据库连接体系结构的高级概述。 

![体系结构概述](./media/sql-database-connectivity-architecture/architecture-overview.png)

以下步骤介绍如何通过 Azure SQL 数据库软件负载均衡器 (SLB) 和 Azure SQL 数据库网关建立到 Azure SQL 数据库的连接。

- Azure 内部或 Azure 外部的客户端连接到 SLB，后者具有公共 IP 地址并侦听端口 1433。
- SLB 将流量定向到 Azure SQL 数据库网关。
- 网关将流量重定向到正确的代理中间件。
- 代理中间件再将流量重定向到相应的 Azure SQL 数据库。

> [!IMPORTANT]
> 所有这些组件都在网络和应用层内置了分布式拒绝服务 (DDoS) 保护。
>

## <a name="connectivity-from-within-azure"></a>从 Azure 内连接

如果从 Azure 内连接，连接默认具有“重定向”连接策略。 “重定向”策略表示 TCP 会话与 Azure SQL 数据库建立连接后，会通过将目标虚拟 IP 从 Azure SQL 数据库网关的 IP 更改为代理中间件的 IP，将客户端会话重定向到该代理中间件。 此后，所有后续数据包都直接通过该代理中间件流动，绕过 Azure SQL 数据库网关。 下图演示了此流量流。

![体系结构概述](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>从 Azure 外连接

如果从 Azure 外连接，连接默认具有“代理”连接策略。 “代理”策略通过 Azure SQL 数据库网关建立 TCP 会话，并且所有后续数据包都通过网关流动。 下图演示了此流量流。

![体系结构概述](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

## <a name="change-azure-sql-database-connection-policy"></a>更改 Azure SQL 数据库连接策略

若要为 Azure SQL 数据库服务器更改 Azure SQL 数据库连接策略，请使用 [REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx)。 

- 如果将连接策略设置为“代理”，所有网络数据包都将通过 Azure SQL 数据库网关流动。 对于此设置，需要只允许出站到 Azure SQL 数据库网关 IP。 使用“代理”设置比使用“重定向”设置的延迟时间更长。 
- 如果将连接策略设置为“重定向”，则所有网络数据包将直接流向中间件代理。 对于此设置，需要允许出站到多个 IP。 

## <a name="script-to-change-connection-settings"></a>用于更改连接设置的脚本

> [!IMPORTANT]
> 此脚本需要 [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。
>

以下 PowerShell 脚本演示如何更改连接策略。

```powershell
import-module azureRm
Login-AzureRmAccount -EnvironmentName AzureChinaCloud

$tenantId =  #your AAD tenant ID
$subscriptionId = #Azure SubscriptionID
$uri = #AAD uri
$authUrl = "https://login.chinacloudapi.cn/$tenantId"
$serverName = #sqldb server name 
$resourceGroupName=#sqldb resource group
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl

$result = $AuthContext.AcquireToken("https://management.core.chinacloudapi.cn/",
$clientId,
[Uri]$uri, 
[Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Auto)

$authHeader = @{
'Content-Type'='application\json; '
'Authorization'=$result.CreateAuthorizationHeader()
}

#getting the current connection property
Invoke-RestMethod -Uri "https://management.chinacloudapi.cn/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#setting the property to 'Proxy'
$connectionType="Proxy" <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

Invoke-RestMethod -Uri "https://management.chinacloudapi.cn/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="next-steps"></a>后续步骤

- 若要了解如何为 Azure SQL 数据库服务器更改 Azure SQL 数据库连接策略，请参阅[使用 REST API 创建或更新服务器连接策略](https://msdn.microsoft.com/library/azure/mt604439.aspx)。
- 若要了解使用 ADO.NET 4.5 或更高版本的客户端的 Azure SQL 数据库连接行为，请参阅[用于 ADO.NET 4.5 的非 1433 端口](sql-database-develop-direct-route-ports-adonet-v12.md)。
- 若要了解常规应用程序开发的概述信息，请参阅[SQL 数据库应用程序开发概述](sql-database-develop-overview.md)。

<!--Update_Description: wording update -->