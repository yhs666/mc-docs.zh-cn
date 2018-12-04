---
title: Azure SQL 数据库连接体系结构 | Microsoft Docs
description: 本文档从 Azure 内部或 Azure 外部说明 Azure SQL 数据库连接体系结构。
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
manager: craigg
origin.date: 11/02/2018
ms.date: 11/14/2018
ms.openlocfilehash: 57a95206f1301c456e0e330b012e05f90f553ad8
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52653525"
---
# <a name="azure-sql-database-connectivity-architecture"></a>Azure SQL 数据库连接体系结构

本文介绍 Azure SQL Database 连接体系结构，并说明不同组件如何将流量定向到 Azure SQL 数据库的实例。 这些 Azure SQL 数据库连接组件通过从 Azure 内连接的客户端以及从 Azure 外连接的客户端将网络流量定向到 Azure 数据库。 本文还提供脚本示例（用于更改连接发生的方式）以及与更改默认连接设置相关的注意事项。

## <a name="connectivity-architecture"></a>连接体系结构

下图提供 Azure SQL 数据库连接体系结构的高级概述。

![体系结构概述](./media/sql-database-connectivity-architecture/architecture-overview.png)

以下步骤介绍如何通过 Azure SQL 数据库软件负载均衡器 (SLB) 和 Azure SQL 数据库网关建立到 Azure SQL 数据库的连接。

- 客户端连接到 SLB，后者使用公共 IP 地址并侦听端口 1433。
- SLB 将流量转发到 Azure SQL 数据库网关。
- 该网关根据有效的连接策略将流量重定向或代理到适当的代理中间件。
- 代理中间件将流量转发到相应的 Azure SQL 数据库。

> [!IMPORTANT]
> 其中每个组件都具有内置于网络和应用层的分布式拒绝服务 (DDoS) 保护。

## <a name="connection-policy"></a>连接策略

Azure SQL 数据库支持 SQL 数据库服务器连接策略设置的以下三个选项：

- **重定向（建议）：** 客户端直接与托管数据库的节点建立连接。 若要启用连接，客户端必须允许对区域中的所有 Azure IP 地址而不仅仅是 Azure SQL 数据库网关 IP 地址应用出站防火墙规则（尝试结合[服务标记](../virtual-network/security-overview.md#service-tags)使用网络安全组 (NSG) 来应用此规则）。 由于数据包会直接发往数据库，因此延迟、吞吐量和性能都会得到改善。
- **代理：** 在此模式下，所有连接都是通过 Azure SQL 数据库网关代理的。 若要启用连接，客户端必须包含只允许 Azure SQL 数据库网关 IP 地址（通常每个区域有两个 IP 地址）的出站防火墙规则。 选择此模式可能导致延迟增大、吞吐量降低，具体取决于工作负荷的性质。 我们强烈建议使用“重定向”连接策略而不要使用“代理”连接策略，以最大程度地降低延迟和提高吞吐量。
- **默认：** 除非显式将连接策略更改为“代理”或“重定向”，否则，在创建后，此连接策略将在所有服务器上生效。 有效策略取决于连接是源自 Azure 内部（重定向）还是外部（代理）。

## <a name="connectivity-from-within-azure"></a>从 Azure 内连接

如果在 2018 年 11 月 10 日后创建的服务器上从 Azure 内部进行连接，则连接默认采用“重定向”连接策略。 “重定向”策略表示 TCP 会话与 Azure SQL 数据库建立连接后，会通过将目标虚拟 IP 从 Azure SQL 数据库网关的 IP 更改为代理中间件的 IP，将客户端会话重定向到该代理中间件。 此后，所有后续数据包都直接通过该代理中间件流动，绕过 Azure SQL 数据库网关。 下图演示了此流量流。

![体系结构概述](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

> [!IMPORTANT]
> 如果 SQL 数据库服务器是在 2018 年 11 月 10 日之前创建的，则连接策略已显式设置为“代理”。 使用服务终结点时，我们强烈建议将连接策略更改为“重定向”，以实现更好的性能。 如果将连接策略更改为“重定向”，允许根据 NSG 出站到下面列出的 Azure SQL 数据库网关 IP 将还不够，必须允许出站到所有 Azure SQL 数据库 IP。 这可以借助 NSG（网络安全组）服务标记实现。 有关详细信息，请参阅[服务标记](../virtual-network/security-overview.md#service-tags)。

## <a name="connectivity-from-outside-of-azure"></a>从 Azure 外连接

如果从 Azure 外连接，连接默认具有“代理”连接策略。 “代理”策略通过 Azure SQL 数据库网关建立 TCP 会话，并且所有后续数据包都通过网关流动。 下图演示了此流量流。

![体系结构概述](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL 数据库网关 IP 地址

若要从本地资源连接到 Azure SQL 数据库，需要允许到你的 Azure 区域的 Azure SQL 数据库网关的出站网络流量。 在代理模式下连接时，连接仅通过网关建立，从本地资源进行连接时这是默认设置。

下表列出了所有数据区域的 Azure SQL 数据库网关的主 IP 和次要 IP。 某些区域中存在两个 IP 地址。 在这些区域中，主 IP 地址是网关的当前 IP 地址，第二个 IP 地址是故障转移 IP 地址。 故障转移地址是我们可能会将服务器移动到该位置以保持服务的高可用性的地址。 对于这些区域，我们建议允许出站到这两个 IP 地址。 第二个 IP 地址由 Microsoft 拥有，并且不侦听任何服务，除非 Azure SQL 数据库激活该地址以接受连接。

| 区域名称 | 主 IP 地址 | 次要 IP 地址 |
| --- | --- |--- |
| 中国东部 1 | 139.219.130.35 | |
| 中国东部 2 | 40.73.82.1 | |
| 中国北部 1 | 139.219.15.17 | |
| 中国北部 2 | 40.73.50.0 | |

## <a name="change-azure-sql-database-connection-policy"></a>更改 Azure SQL 数据库连接策略

若要更改 Azure SQL 数据库服务器的 Azure SQL 数据库连接策略，请使用 [conn-policy](https://docs.azure.cn/zh-cn/cli/sql/server/conn-policy) 命令。

- 如果将连接策略设置为“代理”，所有网络数据包都将通过 Azure SQL 数据库网关流动。 对于此设置，需要只允许出站到 Azure SQL 数据库网关 IP。 使用“代理”设置比使用“重定向”设置的延迟时间更长。
- 如果将连接策略设置为“重定向”，则所有网络数据包将直接流向中间件代理。 对于此设置，需要允许出站到多个 IP。

## <a name="script-to-change-connection-settings-via-powershell"></a>通过 PowerShell 编写脚本以更改连接设置

> [!IMPORTANT]
> 此脚本需要 [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。
>

以下 PowerShell 脚本演示如何更改连接策略。

```powershell
Connect-AzureRmAccount -EnvironmentName AzureChinaCloud
Select-AzureRmSubscription -SubscriptionName <Subscription Name>

# Azure Active Directory ID
$tenantId = "<Azure Active Directory GUID>"
$authUrl = "https://login.partner.microsoftonline.cn/$tenantId"

# Subscription ID
$subscriptionId = "<Subscription GUID>"

# Create an App Registration in Azure Active Directory.  Ensure the application type is set to NATIVE
# Under Required Permissions, add the API:  Windows Azure Service Management API

# Specify the redirect URL for the app registration
$uri = "<NATIVE APP - REDIRECT URI>"

# Specify the application id for the app registration
$clientId = "<NATIVE APP - APPLICATION ID>"

# Logical SQL Server Name
$serverName = "<LOGICAL DATABASE SERVER - NAME>"

# Resource Group where the SQL Server is located
$resourceGroupName= "<LOGICAL DATABASE SERVER - RESOURCE GROUP NAME>"


# Login and acquire a bearer token
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$result = $AuthContext.AcquireToken(
"https://management.core.chinacloudapi.cn/",
$clientId,
[Uri]$uri,
[Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Auto
)

$authHeader = @{
'Content-Type'='application\json; '
'Authorization'=$result.CreateAuthorizationHeader()
}

#Get current connection Policy
Invoke-RestMethod -Uri "https://management.chinacloudapi.cn/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#Set connection policy to Proxy
$connectionType="Proxy" <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

# Apply Changes
Invoke-RestMethod -Uri "https://management.chinacloudapi.cn/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="script-to-change-connection-settings-via-azure-cli"></a>通过 Azure CLI 编写脚本以更改连接设置

> [!IMPORTANT]
> 此脚本需要 [Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

以下 CLI 脚本演示如何更改连接策略。

```azurecli
<pre>
# Get SQL Server ID
sqlserverid=$(az sql server show -n <b>sql-server-name</b> -g <b>sql-server-group</b> --query 'id' -o tsv)

# Set URI
id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $id

# Update connection policy
az resource update --ids $id --set properties.connectionType=Proxy

</pre>
```

## <a name="next-steps"></a>后续步骤

- 有关如何更改 Azure SQL 数据库服务器的 Azure SQL 数据库连接策略的信息，请参阅 [conn-policy](https://docs.azure.cn/cli/sql/server/conn-policy)。
- 若要了解使用 ADO.NET 4.5 或更高版本的客户端的 Azure SQL 数据库连接行为，请参阅[用于 ADO.NET 4.5 的非 1433 端口](sql-database-develop-direct-route-ports-adonet-v12.md)。
- 若要了解常规应用程序开发的概述信息，请参阅[SQL 数据库应用程序开发概述](sql-database-develop-overview.md)。
<!--Update_Description: add "Script to change connection settings via Azure CLI 2.0 " section-->