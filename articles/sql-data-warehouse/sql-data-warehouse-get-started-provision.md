---
title: "在 Azure 门户中创建 SQL 数据仓库 | Azure"
description: "了解如何在 Azure 门户中创建 Azure SQL 数据仓库"
services: sql-data-warehouse
documentationcenter: NA
author: rockboyfor
manager: digimobile
editor: 
tags: azure-sql-data-warehouse
ms.assetid: 552e496e-d560-419c-9996-6bbc80c521cb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
origin.date: 10/31/2016
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: 3ed82cc84f3b41e3438d7adcd4b45ed3b1262421
ms.sourcegitcommit: 3727b139aef04c55efcccfa6a724978491b225a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2017
---
<a id="create-an-azure-sql-data-warehouse" class="xliff"></a>

# 创建 Azure SQL 数据仓库

> [!div class="op_single_selector"]
> * [Azure 门户](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

本教程使用 Azure 门户来创建包含 AdventureWorksDW 示例数据库的 SQL 数据仓库。

<a id="prerequisites" class="xliff"></a>

## 先决条件
若要开始，您需要：

* **Azure 帐户**：访问 [Azure 试用版][Azure Trial]，以创建帐户。
* **Azure SQL Server**：有关详细信息，请参阅[使用 Azure 门户创建 Azure SQL 数据库][Create an Azure SQL database in the Azure portal]。
<!-- [MSDN Azure Credits] not supported in ACN--> 

> [!NOTE]
> 创建 SQL 数据仓库可能会导致新的计费服务。  有关详细信息，请参阅 [SQL 数据仓库定价][SQL Data Warehouse pricing] 。
>
>

<a id="create-a-sql-data-warehouse" class="xliff"></a>

## 创建 SQL 数据仓库

1. 登录到 [Azure 门户](https://portal.azure.cn)。

2. 单击“+ 新建” > “数据库” > “SQL 数据仓库”。

    ![创建](./media/sql-data-warehouse-get-started-provision/create-sample.gif)
3. 在“SQL 数据仓库”  边栏选项卡中，填写所需的信息，然后按“创建”进行创建。

    ![创建数据库](./media/sql-data-warehouse-get-started-provision/create-database.png)

    * **服务器**：建议先选择服务器。  

    * **数据库名称**：用于引用 SQL 数据仓库的名称。  对服务器而言，它必须是唯一的。

    * **性能**：建议从 400 [DWU][DWU]开始。 可以将滑块向左或向右移动以调整数据仓库的性能，也可以在创建之后增加或减少。  若要了解有关 DWU 的详细信息，请参阅[缩放](sql-data-warehouse-manage-compute-overview.md)文档或[定价页][SQL Data Warehouse pricing]。

    * **订阅**：选择此 SQL 数据仓库将会计费的 [订阅] 。

    * **资源组**： [资源组][Resource group] 是旨在帮助管理 Azure 资源集合的容器。 了解有关 [资源组](../azure-resource-manager/resource-group-overview.md)的详细信息。

    * **选择源**：单击“选择源” > “示例”。 Azure 使用 AdventureWorksDW 自动填充“选择示例”  选项。

    > [!NOTE]
    > SQL 数据仓库的默认排序规则是 SQL_Latin1_General_CP1_CI_AS。 如果需要其他排序规则，可使用 [T-SQL][T-SQL] 创建具有不同排序规则的数据库。
    >
    >

4. 单击“创建”  以创建 SQL 数据仓库。

5. 请稍等几分钟。 数据仓库准备就绪后，应返回到 [Azure 门户](https://portal.azure.cn)。 你可以在仪表板上找到你的 SQL 数据仓库，它列于 SQL 数据库之下，或在用来创建它的资源组中。 

    ![门户视图](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

<a id="next-steps" class="xliff"></a>

## 后续步骤
创建 SQL 数据仓库后，便可以[连接](sql-data-warehouse-connect-overview.md)并开始查询。

若要将数据加载到 SQL 数据仓库中，请参阅[加载概述](sql-data-warehouse-overview-load.md)。

如果尝试将现有数据库迁移到 SQL 数据仓库，请参阅[迁移概述](sql-data-warehouse-overview-migrate.md)或使用[迁移实用工具](sql-data-warehouse-migrate-migration-utility.md)。

还可以使用 Transact-SQL 配置防火墙规则。 有关详细信息，请参阅 [sp_set_firewall_rule][sp_set_firewall_rule] 和 [sp_set_database_firewall_rule][sp_set_database_firewall_rule]。

另外，建议查看[最佳实践][Best practices]。

<!--Article references-->
[Create an Azure SQL database in the Azure portal]: ../sql-database/sql-database-get-started.md
<!--[Create an Azure SQL database with PowerShell] Not available in ACOM-->
[resource groups]: ../azure-resource-manager/resource-group-template-deploy-portal.md
[Best practices]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md
[订阅]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md

<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/zh-cn/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/zh-cn/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://www.azure.cn/pricing/details/sql-data-warehouse/
[Azure Trial]: https://www.azure.cn/pricing/1rmb-trial/
<!-- Not Available [MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F-->