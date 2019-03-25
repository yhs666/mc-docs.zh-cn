---
title: 将 Azure 流量定向到 Azure SQL 数据库和 SQL 数据仓库 | Microsoft Docs
description: 本文档从 Azure 内部或 Azure 外部说明 Azure SQL 数据库和 SQL 数据仓库连接体系结构。
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
manager: digimobile
origin.date: 02/25/2019
ms.date: 03/25/2019
ms.openlocfilehash: cbfafaeac8f5a8de7e44005aaa3b8d57dae496e7
ms.sourcegitcommit: 02c8419aea45ad075325f67ccc1ad0698a4878f4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318983"
---
# <a name="azure-sql-connectivity-architecture"></a>Azure SQL 连接体系结构

本文介绍 Azure SQL 数据库和 SQL 数据仓库连接体系结构，以及如何使用不同的组件将流量定向到 Azure SQL 实例。 借助这些连接组件，可以通过连接自 Azure 内部的客户端和连接自 Azure 外部的客户端将网络流量定向到 Azure SQL 数据库或 SQL 数据仓库。 本文还提供脚本示例（用于更改连接发生的方式）以及与更改默认连接设置相关的注意事项。

> [!IMPORTANT]
> **[即将发生的更改] 对于到 Azure SQL Server 的服务终结点连接，`Default` 连接行为会更改为`Redirect`。**
> 建议客户创建新的服务器，并将现有服务器的连接类型显式设置为“重定向”（首选）或“代理”，具体取决于服务器的连接体系结构。
>
> 为了防止在进行此更改时在现有环境中通过服务终结点进行的连接中断，我们使用遥测执行以下操作：
>
> - 对于在更改前检测到的通过服务终结点进行过访问的服务器，我们将连接类型切换为 `Proxy`。
> - 对于所有其他的服务器，我们会将连接类型切换为 `Redirect`。
>
> 在以下方案中，服务终结点用户可能仍会受影响：
>
> - 应用程序不常连接到现有的服务器，因此我们的遥测不捕获有关这些应用程序的信息。
> - 自动部署逻辑创建 SQL 数据库服务器（假设服务终结点连接的默认行为是 `Proxy`）
>
> 如果不能建立到 Azure SQL Server 的服务终结点连接，而你怀疑自己受此更改的影响，请验证是否已将连接类型显式设置为 `Redirect`。 如果是这种情况，则必须对区域中属于端口 11000-12000 的 Sql [服务标记](../virtual-network/security-overview.md#service-tags)的所有 Azure IP 地址启用 VM 防火墙规则和网络安全组 (NSG)。 如果这不是适合自己的选项，请将服务器显式切换为 `Proxy`。
> [!NOTE]
> 本主题适用于托管单一数据库和弹性池的 Azure SQL 数据库服务器以及 SQL 数据仓库数据库。 为简单起见，在提到 SQL 数据库和 SQL 数据仓库时，本文统称 SQL 数据库。

## <a name="connectivity-architecture"></a>连接体系结构

下图提供 Azure SQL 数据库连接体系结构的高级概述。

![体系结构概述](./media/sql-database-connectivity-architecture/connectivity-overview.png)

以下步骤介绍如何建立到 Azure SQL 数据库的连接。

- 客户端连接到网关，后者使用公共 IP 地址并侦听端口 1433。
- 该网关根据有效的连接策略将流量重定向或代理到适当的数据库群集。
- 在数据库群集中，流量转发到相应的 Azure SQL 数据库。

## <a name="connection-policy"></a>连接策略

Azure SQL 数据库支持 SQL 数据库服务器连接策略设置的以下三个选项：

- **重定向（建议）：** 客户端直接与托管数据库的节点建立连接。 若要启用连接，客户端必须允许对区域中端口 11000-12000 的所有 Azure IP 地址而不仅仅是端口 1433 上的 Azure SQL 数据库网关 IP 地址应用出站防火墙规则，并结合[服务标记](../virtual-network/security-overview.md#service-tags)使用网络安全组 (NSG)。 由于数据包会直接发往数据库，因此延迟、吞吐量和性能都会得到改善。
- **代理：** 在此模式下，所有连接都是通过 Azure SQL 数据库网关代理的。 若要启用连接，客户端必须包含只允许 Azure SQL 数据库网关 IP 地址（通常每个区域有两个 IP 地址）的出站防火墙规则。 选择此模式可能导致延迟增大、吞吐量降低，具体取决于工作负荷的性质。 我们强烈建议使用 `Redirect` 连接策略而不要使用 `Proxy` 连接策略，以最大程度地降低延迟和提高吞吐量。
- **默认：** 除非显式将连接策略更改为 `Proxy` 或 `Redirect`，否则，在创建后，此连接策略将在所有服务器上生效。 有效策略取决于连接是源自 Azure 内部（`Redirect`）还是外部（`Proxy`）。

## <a name="connectivity-from-within-azure"></a>从 Azure 内连接

如果从 Azure 内部连接，则连接默认具有 `Redirect` 连接策略。 `Redirect` 策略是指建立到 Azure SQL 数据库的 TCP 会话连接后，会将 Azure SQL 数据库网关的目标虚拟 IP 更改为群集的目标虚拟 IP，从而将客户端会话重定向到适当的数据库群集。 此后，所有后续数据包绕过 Azure SQL 数据库网关，直接传输到群集。 下图演示了此流量流。

![体系结构概述](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>从 Azure 外连接

如果从 Azure 外部连接，则连接默认具有 `Proxy` 连接策略。 `Proxy` 策略是指通过 Azure SQL 数据库网关建立 TCP 会话，并且所有后续数据包通过网关传输。 下图演示了此流量流。

![体系结构概述](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL 数据库网关 IP 地址

若要从本地资源连接到 Azure SQL 数据库，需要允许到你的 Azure 区域的 Azure SQL 数据库网关的出站网络流量。 在 `Proxy` 模式下连接时，连接仅通过网关建立，从本地资源进行连接时这是默认设置。

下表列出了所有数据区域的 Azure SQL 数据库网关的主 IP 和次要 IP。 某些区域中存在两个 IP 地址。 在这些区域中，主 IP 地址是网关的当前 IP 地址，第二个 IP 地址是故障转移 IP 地址。 故障转移地址是我们可能会将服务器移动到该位置以保持服务的高可用性的地址。 对于这些区域，我们建议允许出站到这两个 IP 地址。 第二个 IP 地址由 Microsoft 拥有，并且不侦听任何服务，除非 Azure SQL 数据库激活该地址以接受连接。 现在，中国区域只有主 IP 地址。

| 区域名称 | 主 IP 地址 | 次要 IP 地址 |
| --- | --- |--- |
| 中国东部 1 | 139.219.130.35 | |
| 中国东部 2 | 40.73.82.1 | |
| 中国北部 1 | 139.219.15.17 | |
| 中国北部 2 | 40.73.50.0 | |

## <a name="change-azure-sql-database-connection-policy"></a>更改 Azure SQL 数据库连接策略

若要更改 Azure SQL 数据库服务器的 Azure SQL 数据库连接策略，请使用 [conn-policy](https://docs.azure.cn/zh-cn/cli/sql/server/conn-policy) 命令。

- 如果将连接策略设置为 `Proxy`，则所有网络数据包均通过 Azure SQL 数据库网关进行传输。 对于此设置，需要只允许出站到 Azure SQL 数据库网关 IP。 使用 `Proxy` 设置比使用 `Redirect` 设置的延迟时间更长。
- 如果连接策略设置为 `Redirect`，则所有网络数据包直接向数据库群集传输。 对于此设置，需要允许出站到多个 IP。

## <a name="script-to-change-connection-settings-via-powershell"></a>通过 PowerShell 编写脚本以更改连接设置

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> 此脚本需要 [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。

以下 PowerShell 脚本演示如何更改连接策略。

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzResource -ResourceId $id).Properties.connectionType

# Update connection policy
Set-AzResource -ResourceId $id -Properties @{"connectionType" = "Proxy"} -f
```

## <a name="script-to-change-connection-settings-via-azure-cli"></a>通过 Azure CLI 编写脚本以更改连接设置

> [!IMPORTANT]
> 此脚本需要 [Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

以下 CLI 脚本演示如何更改连接策略。

```azurecli
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $id

# Update connection policy
az resource update --ids $id --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>后续步骤

- 有关如何更改 Azure SQL 数据库服务器的 Azure SQL 数据库连接策略的信息，请参阅 [conn-policy](https://docs.azure.cn/cli/sql/server/conn-policy)。
- 若要了解使用 ADO.NET 4.5 或更高版本的客户端的 Azure SQL 数据库连接行为，请参阅[用于 ADO.NET 4.5 的非 1433 端口](sql-database-develop-direct-route-ports-adonet-v12.md)。
- 若要了解常规应用程序开发的概述信息，请参阅[SQL 数据库应用程序开发概述](sql-database-develop-overview.md)。
<!--Update_Description: add "Script to change connection settings via Azure CLI 2.0 " section-->