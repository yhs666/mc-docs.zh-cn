---
title: 借助 Azure 扩展对 SQL Server 2008 和 SQL Server 2008 R2 的支持 | Azure
description: 了解如何通过将 SQL Server 实例迁移到 Azure，或者购买扩展支持来保留本地实例，扩展对 SQL Server 2008 和 SQL Server 2008 R2 的支持。
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
origin.date: 04/08/2019
ms.date: 07/01/2019
ms.author: v-yeche
ms.reviewer: jroth
ms.openlocfilehash: f6b2f520dceaa56f4e574a4677539d5123aa46cb
ms.sourcegitcommit: 5191c30e72cbbfc65a27af7b6251f7e076ba9c88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571471"
---
# <a name="extend-support-for-sql-server-2008-and-sql-server-2008-r2-with-azure"></a>借助 Azure 扩展对 SQL Server 2008 和 SQL Server 2008 R2 的支持

SQL Server 2008 和 SQL Server 2008 R2 的[支持生命周期都即将结束 (EOS)](https://www.microsoft.com/sql-server/sql-server-2008)。 由于我们的许多客户仍在使用这两个版本，我们提供了多个选项让这些客户继续获得支持。 你可以将本地 SQL Server 实例迁移到 Azure 虚拟机 (VM)、迁移到 Azure SQL 数据库，或者将其保留在本地，但这需要购买扩展的安全更新。

与托管实例不同，迁移到 Azure VM 不需要重新认证应用程序。 此外，与保留本地实例不同，迁移到 Azure VM 可以获得免费的扩展安全修补程序。 

本文的余下内容提供有关将 SQL Server 实例迁移到 Azure VM 时的注意事项。 

## <a name="provisioning"></a>设置 

Azure 市场中提供了一个标准预付费套餐 `SQL Server 2008 R2 on Windows Server 2008 R2` 映像。 

SQL Server 2008 客户需要自行安装或升级到 SQL Server 2008 R2。 同样，Windows Server 2008 客户需要从自定义 VHD 部署其 VM，或升级到 Windows Server 2008 R2。 

通过市场部署的映像已预装了 SQL IaaS 扩展。 若要进行灵活授权和自动修补，必须安装 SQL IaaS 扩展。 部署自行安装的 VM 的客户需要手动安装 SQL IaaS 扩展。 Windows 2008 不支持 SQL IaaS 扩展。 

  > [!NOTE]
  > 尽管可以在 Azure 门户中的 SQL Server `Create` 和 `Manage` 边栏选项卡上处理 SQL Server 2008R2 映像，但不支持以下功能：  自动备份、Azure Key Vault 集成、R Services 和存储配置。

## <a name="licensing"></a>许可
标准预付费套餐 SQL Server 2008R2 部署可以转换为 [Azure 混合权益 (AHB)](https://www.azure.cn/pricing/hybrid-use-benefit/)。

若要将基于软件保障 (SA) 的许可证转换为标准预付费套餐，客户应注册到 SQL VM 资源提供程序。 注册到 SQL VM 资源提供程序后，可以在 AHB 与标准预付费套餐之间互换 SQL 许可证类型。 

<!--Not Available on [resource provider](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider)-->

在 Azure VM 上自行安装的 SQL Server 2008 或 SQL Server 2008 R2 实例可以注册到 SQL 资源提供程序，并且可将其许可证类型转换为标准预付费套餐。

## <a name="migration"></a>迁移
可以使用手动备份/还原方法将 EOS SQL Server 实例迁移到 Azure VM；这是从本地迁移到 Azure VM 的最常用方法。

### <a name="azure-site-recovery"></a>Azure Site Recovery

对于批量迁移，我们建议使用 [Azure Site Recovery](/site-recovery/site-recovery-overview) 服务。 客户可以使用 Azure Site Recovery 将整个 VM（包括 SQL Server）从本地复制到 Azure VM。

SQL Server 需要使用应用一致的 Azure Site Recovery 快照来保证恢复成功；Azure Site Recovery 支持最小 1 小时间隔的应用一致性快照。 使用 Azure Site Recovery 迁移 SQL Server 可以实现的最小 RPO 为 1 小时，RTO 为 2 个小时加上 SQL Server 恢复时间。

<!--Not Available on ### Database Migration Service-->

<!--Not Available on [Database Migration Service](/dms/dms-overview)-->

## <a name="disaster-recovery"></a>灾难恢复

适用于 Azure VM 上的 EOS SQL Server 的灾难恢复解决方案如下：

- **SQL Server 备份**：发生区域性或局部性的故障时，可以使用 SQL Server 备份来恢复 SQL Server。 由于 EOS SQL Server 不支持托管备份功能，客户需要手动进行备份。
- **日志传送**：可以在提供连续还原功能的另一个局部区域或 Azure 区域中创建一个日志传送副本，以减小 RTO。 客户需要手动配置日志传送。
- **Azure Site Recovery**：可以通过 Azure Site Recovery 在局部区域与区域之间复制 VM。 SQL Server 需要使用应用一致的快照来保证在发生灾难时成功恢复。 对于 EOS SQL Server 的灾难恢复，Azure Site Recovery 可提供最小 1 小时的 RPO，以及 2 小时 + SQL Server 恢复时间的 RTO。

## <a name="security-patching"></a>安全修补
将 SQL Server VM 注册到 SQL 资源提供程序后，将通过 Microsoft 更新通道传送 SQL Server VM 的扩展安全更新。 可以手动或自动下载修补程序。 

<!--Not Available on [resource provider](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider)-->

**自动修补** ：默认处于启用状态。 Azure 可以通过自动修补来自动修补 SQL Server 和操作系统。 如果已安装 SQL IaaS 扩展，可为维护时段指定星期日期、时间和持续时间。 Azure 会在维护时段进行修补。 维护时段计划使用 VM 的时间区域设置。  有关详细信息，请参阅 [Azure 虚拟机中 SQL Server 的自动修补](virtual-machines-windows-sql-automated-patching.md)。

## <a name="next-steps"></a>后续步骤

将 SQL Server VM 迁移到 Azure

* [将 SQL Server 数据库迁移到 Azure VM 中的 SQL Server](virtual-machines-windows-migrate-sql.md)

Azure 虚拟机上的 SQL Server 入门：

* [在 Azure 门户中创建 SQL Server VM](quickstart-sql-vm-create-portal.md)

获取有关 SQL VM 的常见问题的解答：

* [Azure 虚拟机中的 SQL Server 常见问题](virtual-machines-windows-sql-server-iaas-faq.md)

<!-- Update_Description: new articles on virtual machines windows sql server 2008 extent support-->
<!--ms.date: 07/08/2019-->