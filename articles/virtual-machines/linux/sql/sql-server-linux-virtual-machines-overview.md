---
title: Azure Linux 虚拟机中的 SQL Server 概述 | Azure
description: 了解如何在 Azure Linux 虚拟机上运行完整的 SQL Server 版本。 获取到所有 Linux SQL Server VM 映像和相关内容的直接链接。
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.workload: iaas-sql-server
origin.date: 04/10/2018
ms.date: 05/21/2018
ms.author: v-yeche
ms.openlocfilehash: 964775b9a2a6f88a6b066aa886a6e242156ab68b
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52653289"
---
# <a name="overview-of-sql-server-on-azure-virtual-machines-linux"></a>Azure 虚拟机 (Linux) 上的 SQL Server 概述

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
> * [Linux](sql-server-linux-virtual-machines-overview.md)

Azure 虚拟机上的 SQL Server 允许你在云中使用完整版本的 SQL Server，而不需管理任何本地硬件。 使用即用即付时，SQL Server VM 还可以简化许可成本。

Azure 虚拟机在中国许多不同的[地理区域](https://www.azure.cn/support/service-dashboard/)运行， 并提供各种[虚拟机大小](../sizes.md)。 使用虚拟机映像库可以创建 SQL Server VM，而且版本和操作系统都很正确。 因此，虚拟机适用于许多不同的 SQL Server 工作负荷。
<!--Notice: Change around the world to China -->

<a name="create"></a>
##  <a name="get-started-with-sql-vms"></a>SQL VM 入门

若要开始，请选择一个 SQL Server 虚拟机映像，其中包含所需的版本和操作系统。 下面的各部分针对相关 SQL Server 虚拟机库映像提供了指向 Azure 门户的直接链接。

> [!TIP]
> 有关如何了解 SQL 映像定价的详细信息，请参阅 [Linux SQL Server VM 定价页](https://www.azure.cn/pricing/details/virtual-machines/)。

| 版本 | 操作系统 | 版本 |
| --- | --- | --- |
| **SQL Server 2017** | Ubuntu 16.04 LTS |[Enterprise](https://portal.azure.cn/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS)、[Standard](https://portal.azure.cn/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS)、[Web](https://portal.azure.cn/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS)、[Express](https://portal.azure.cn/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS)、[Developer](https://portal.azure.cn/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS) |
<!-- Not Avaiable on RHEL (Red Har Enterprise License) -->
<!-- Not Avaiable on SLES (SUSE Linux Enterprise Server) -->

> [!NOTE]
> 若要查看可用的 Windows SQL Server 虚拟机映像，请参阅 [Azure 虚拟机上的 SQL Server 概述 (Windows)](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)。

<a name="packages"></a>
##  <a name="installed-packages"></a>已安装程序包

在 Linux 上配置 SQL Server 时，请先安装数据库引擎包，然后根据需要安装多个可选包。 用于 SQL Server 的 Linux 虚拟机自动安装大多数包。 下表显示为每个分发安装了哪些包。

| 分发 | [数据库引擎](https://docs.microsoft.com/sql/linux/sql-server-linux-setup) | [工具](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools) | [SQL Server 代理](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-sql-agent) | [全文搜索](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-full-text-search) | [SSIS](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-ssis) | [HA 外接程序](https://docs.microsoft.com/sql/linux/sql-server-linux-business-continuity-dr) |
|---|---|---|---|---|---|---|
| Ubuntu | ![是](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![是](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![是](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![是](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![是](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![是](./media/sql-server-linux-virtual-machines-overview/yes.png) |
<!-- Not Avaiable on RHEL (Red Har Enterprise License) -->
<!-- Not Avaiable on SLES (SUSE Linux Enterprise Server) -->

## <a name="related-products-and-services"></a>相关产品和服务

### <a name="linux-virtual-machines"></a>Linux 虚拟机

* [虚拟机概述](../overview.md)

### <a name="storage"></a>存储

* [Azure 存储简介](../../../storage/common/storage-introduction.md)

### <a name="networking"></a>网络

* [虚拟网络概述](../../../virtual-network/virtual-networks-overview.md)
* [Azure 中的 IP 地址](../../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [在 Azure 门户中创建完全限定的域名](../portal-create-fqdn.md)

### <a name="sql"></a>SQL

* [“Linux 上的 SQL Server”文档](https://docs.microsoft.com/sql/linux)
* [Azure SQL 数据库比较](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md)

## <a name="next-steps"></a>后续步骤

Azure Linux 虚拟机上的 SQL Server 入门：

* [在 Azure 门户中创建 SQL Server VM](provision-sql-server-linux-virtual-machine.md)

<!-- Not Available on Get answers to commonly asked questions about SQL VMs on Linux: -->

<!-- Not Available on [SQL Server on Azure Linux Virtual Machines FAQ](sql-server-linux-faq.md)-->
<!-- Update_Description: update meta properties, wording update, update link -->