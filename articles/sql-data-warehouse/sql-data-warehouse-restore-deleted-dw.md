---
title: 还原已删除的 Azure SQL 数据仓库 | Microsoft Docs
description: 用于还原已删除的 SQL 数据仓库的操作指南。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
origin.date: 08/29/2018
ms.date: 09/02/2019
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: 047cb814a4fd198296d8983ea329c6baae18ba46
ms.sourcegitcommit: 3f0c63a02fa72fd5610d34b48a92e280c2cbd24a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70132752"
---
# <a name="restore-a-deleted-azure-sql-data-warehouse"></a>还原已删除的 Azure SQL 数据仓库

本文介绍如何通过 Azure 门户和 PowerShell 还原已删除的 SQL 数据仓库：

## <a name="before-you-begin"></a>准备阶段

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**验证 DTU 容量。** 每个 SQL 数据仓库都由一个具有默认 DTU 配额的 SQL 服务器（例如 myserver.database.chinacloudapi.cn）托管。  验证 SQL Server 的剩余 DTU 配额是否足够进行数据库还原。

## <a name="restore-a-deleted-data-warehouse-through-powershell"></a>通过 PowerShell 还原已删除的数据仓库

若要还原已删除的 SQL 数据仓库，请使用 [Restore-AzSqlDatabase][Restore-AzSqlDatabase] cmdlet。 如果相应的逻辑服务器也被删除，则不能还原该数据仓库。

1. 开始之前，请确保[安装 Azure PowerShell][Install Azure PowerShell]。
2. 打开 PowerShell。
3. 连接到 Azure 帐户，并列出与帐户关联的所有订阅。
4. 选择包含要还原的已删除数据仓库的订阅。
5. 获取特定的已删除数据仓库。
6. 还原已删除的数据仓库
    1. 若要将已删除的 SQL 数据仓库还原到另一逻辑服务器，请确保指定另一逻辑服务器名称。  该逻辑服务器也可以位于另一资源组和区域中。
1. 验证已还原的数据仓库是否处于联机状态。
1. 完成还原后，可以按[在恢复后配置数据库][Configure your database after recovery]中的说明配置恢复后的数据仓库。

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.chinacloudapi.cn
#$TargetResourceGroupName="<YourTargetResourceGroupName>" # uncomment to restore to a different logical server.
#$TargetServerName="<YourtargetServerNameWithoutURLSuffixSeeNote>" 
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount -Environment AzureChinaCloud
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Use the following command to restore deleted data warehouse to a different logical server
#$RestoredDatabase = Restore-AzSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $TargetResourceGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

## <a name="restore-a-deleted-database-using-the-azure-portal"></a>通过 Azure 门户还原已删除的数据库

1. 登录到 [Azure 门户][Azure portal]。
2. 导航到承载着已删除数据仓库的 SQL Server。
3. 在目录中选择“已删除的数据库”图标。 

    ![已删除的数据库](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-01.png)

4. 选择要还原的已删除 SQL 数据仓库。

    ![选择“已删除的数据库”](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-11.png)

5. 指定新的**数据库名称**，并单击“确定” 

    ![指定数据库名称](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-21.png)

## <a name="next-steps"></a>后续步骤
- [还原现有数据仓库][Restore an existing data warehouse]
- [从异地备份数据仓库还原][Restore from a geo-backup data warehouse]

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Install Azure PowerShell]: https://docs.microsoft.com/powershell/azure/overview
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Restore an existing data warehouse]:./sql-data-warehouse-restore-active-paused-dw.md
[Restore a deleted data warehouse]:./sql-data-warehouse-restore-deleted-dw.md
[Restore from a geo-backup data warehouse]:./sql-data-warehouse-restore-from-geo-backup.md

<!--MSDN references-->
[Restore-AzSqlDatabase]: https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase

<!--Other Web references-->
[Azure Portal]: https://portal.azure.cn/
