---
title: "Azure 虚拟机中的 SQL Server 常见问题 | Azure"
description: "本文提供有关运行 Azure VM 中的 SQL Server 时遇到的常见问题的解答。"
services: virtual-machines-windows
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
origin.date: 06/23/2017
ms.date: 08/21/2017
ms.author: v-dazen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 70e335e9f16c08eb109b1114602b9d2a2cf7c049
ms.sourcegitcommit: 20d1c4603e06c8e8253855ba402b6885b468a08a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-azure-virtual-machines"></a>Azure 虚拟机上的 SQL Server 常见问题
本主题提供有关运行 [Azure 虚拟机中的 SQL Server](https://www.azure.cn/home/features/virtual-machines/#virtual-machine-SQLserver) 时出现的一些最常见问题的解答。

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>常见问题

1. **如何创建装有 SQL Server 的 Azure 虚拟机？**

    最简单的解决方法是创建包含 SQL Server 的虚拟机。 有关注册 Azure 并从门户创建 SQL VM 的教程，请参阅[在 Azure 门户中预配 SQL Server 虚拟机](virtual-machines-windows-portal-sql-server-provision.md)。 可以选择按分钟支付 SQL Server 许可费的虚拟机映像，或者使用允许自带 SQL Server 许可证的映像。 也可以选择手动在 VM 上安装 SQL Server，并重复使用本地许可证。 如果自带许可，必须[在 Azure 上通过软件保障实现许可证移动性](https://www.azure.cn/pricing/license-mobility/)。 有关详细信息，请参阅 [SQL Server Azure VM 定价指南](virtual-machines-windows-sql-server-pricing-guidance.md)。

1. **SQL VM 与 SQL 数据库服务之间的差别是什么？**

    从概念上讲，在 Azure 虚拟机上运行 SQL Server 与在远程数据中心运行 SQL Server 并没什么不同。 相比之下，[SQL 数据库](../../../sql-database/sql-database-technical-overview.md)可提供数据库即服务。 使用 SQL 数据库时，无法访问托管数据库的计算机。 有关完整比较，请参阅[选择云 SQL Server 选项：Azure SQL (PaaS) 数据库或 Azure VM 上的 SQL Server (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md)。

1. **如何将本地 SQL Server 数据库迁转到云中？**

    首先，请创建装有 SQL Server 实例的 Azure 虚拟机。 然后将本地数据库迁转到该实例。 有关数据迁移策略，请参阅[将 SQL Server 数据库迁移到 Azure VM 中的 SQL Server](virtual-machines-windows-migrate-sql.md)。

1. **是否可以在同一 VM 上安装另一个 SQL Server 实例？是否可以更改默认实例的已安装功能？**

    是的。 SQL Server 安装介质位于 **C** 驱动器上的某个文件夹中。 可从该位置运行 **Setup.exe** 以添加新的 SQL Server 实例，或更改计算机上 SQL Server 的其他已安装功能。 请注意，某些功能（例如自动备份、自动修补和 Azure Key Vault 集成）仅对默认实例起作用。

1. **是否可以卸载 SQL Server 的默认实例？**

    是的。 但有一些注意事项。 如前面的解答中所述，依赖于 [SQL Server IaaS 代理扩展](virtual-machines-windows-sql-server-agent-extension.md)的功能仅对默认实例起作用。 卸载默认实例后，该扩展会继续查找默认实例并可能生成事件日志错误。 这些错误来自以下两个来源：**Microsoft SQL Server 凭据管理**和 **Microsoft SQL Server IaaS 代理**。 其中一个错误可能类似于以下内容：

        A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. 

    如果决定卸载默认实例，还要卸载 [SQL Server IaaS 代理扩展](virtual-machines-windows-sql-server-agent-extension.md)。

1. **如何将 Azure VM 中的 SQL Server 升级到新版本？**

    目前，对于在 Azure VM 中运行的 SQL Server，不提供就地升级。 因此，请使用所需的 SQL Server 版本创建新的 Azure 虚拟机，然后使用标准[数据迁移技术](virtual-machines-windows-migrate-sql.md)，将数据库迁移到新的服务器。

1. **如何在 Azure VM 上安装 SQL Server 的许可版本？**

    将 SQL Server 安装介质复制到 Windows Server VM 上，并在 VM 上安装 SQL Server。 出于许可原因，必须提供 [Azure 上通过软件保障实现的许可移动性](https://www.azure.cn/pricing/license-mobility/)。 有关详细信息，请参阅 [SQL Server Azure VM 定价指南](virtual-machines-windows-sql-server-pricing-guidance.md)。

1. **如果 VM 是基于一个即用即付库映像创建的，是否可以将它更改为使用我自己的 SQL Server 许可证？**

    不可以。 无法从按分钟付费许可证改为使用自己的许可证。 请创建新的 Azure 虚拟机，然后使用标准的[数据迁移技术](virtual-machines-windows-migrate-sql.md)将数据库迁移到新服务器。

1. **Azure VM 是否支持 SQL Server 故障转移群集实例 (FCI)？**

   是的。 可在 [Windows Server 2016 上创建 Windows 故障转移群集 ](virtual-machines-windows-portal-sql-create-failover-cluster.md)，并将存储空间直通 (S2D) 用于群集存储。 或者，可使用第三方群集或存储解决方案，如 [Azure 虚拟机中 SQL Server 的高可用性和灾难恢复](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions)中所述。

1. **如果 Azure VM 仅供备用/故障转移，是否必须支持该 VM 上的 SQL Server 许可费？**

    对于用作 HA 部署中的被动辅助副本的 SQL Server，如果客户购买了软件保障并使用许可移动性，则不需要支付许可费。

1. **如何将更新和服务包应用于 SQL Server VM？**

    虚拟机允许控制主机，包括应用更新的时间与方法。 对于操作系统，可以手动应用 Windows 更新，或者启用名为[自动修补](virtual-machines-windows-sql-automated-patching.md)的计划服务。 自动修补将安装任何标记为重要的更新，包括该类别中的 SQL Server 更新。 必须手动安装其他可选的 SQL Server 更新。

1. **是否可以设置虚拟机库中未显示的配置（例如 Windows 2008 R2 + SQL Server 2012）？**

    不可以。 对于包含 SQL Server 的虚拟机库映像，必须选择提供的映像之一。

1. **如何在 Azure VM 上安装 SQL Data Tools？**

     从 [Microsoft SQL Server 数据工具 - Visual Studio 2013 商业智能](https://www.microsoft.com/download/details.aspx?id=42313)下载并安装 SQL 数据工具。

## <a name="resources"></a>资源

有关 Azure 虚拟机上 SQL Server 的概述，请观看视频 [Azure VM 是 SQL Server 2016 的最佳平台](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016)。 也可以在 [Azure 虚拟机中的 SQL Server 概述](virtual-machines-windows-sql-server-iaas-overview.md)主题中获取详细介绍。

其他资源包括：

* [在 Azure 门户中预配 SQL Server 虚拟机](virtual-machines-windows-portal-sql-server-provision.md)
* [将数据库迁移到 Azure VM 上的 SQL Server](virtual-machines-windows-migrate-sql.md)
* [Azure 虚拟机中 SQL Server 的高可用性和灾难恢复](virtual-machines-windows-sql-high-availability-dr.md)
* [Azure 虚拟机中 SQL Server 的性能最佳做法](virtual-machines-windows-sql-performance.md)
* [Azure 虚拟机中 SQL Server 的应用程序模式和开发策略](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

<!--Update_Description: wording update-->