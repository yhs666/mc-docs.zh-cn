---
title: 还原 Azure SQL 数据仓库 (PowerShell) | Microsoft 文档
description: 用于还原 SQL 数据仓库的 PowerShell 任务。
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
origin.date: 04/17/2018
ms.date: 09/17/2018
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: 9e947b341e76fcd78dc20fb79d8c0ef7cb39acbb
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52654336"
---
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>还原 Azure SQL 数据仓库 (PowerShell)
> [!div class="op_single_selector"]
> * [概述][概述]
> * [门户][门户]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

本文会介绍如何使用 PowerShell 还原 Azure SQL 数据仓库。

## <a name="before-you-begin"></a>准备阶段
**验证 DTU 容量。** 每个 SQL 数据仓库都由一个具有默认 DTU 配额的 SQL 服务器（例如 myserver.database.chinacloudapi.cn）托管。  在还原 SQL 数据仓库之前，请确保 SQL Server 的剩余 DTU 配额足够进行数据库还原。 
<!-- Not Available [Request a DTU quota change][Request a DTU quota change] -->

### <a name="install-powershell"></a>安装 PowerShell
若要对 SQL 数据仓库使用 Azure PowerShell，需要安装 Azure PowerShell 1.0 或更高版本。  可以通过运行 **Get-Module -ListAvailable -Name AzureRM**来检查版本。  可通过 [Microsoft Web 平台安装程序][Microsoft Web Platform Installer]安装最新版本。  有关安装最新版本的详细信息，请参阅[如何安装和配置 Azure PowerShell][如何安装和配置 Azure PowerShell]。

## <a name="restore-an-active-or-paused-database"></a>还原活动或暂停的数据库
若要从快照还原数据库，请使用 [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] PowerShell cmdlet。

1. 打开 Windows PowerShell。
2. 连接到 Azure 帐户，并列出与帐户关联的所有订阅。
3. 选择包含要还原的数据库的订阅。
4. 列出数据库的还原点。
5. 使用 RestorePointCreationDate 选取所需的还原点。
6. 将数据库还原到所需的还原点。
7. 验证已还原的数据库是否处于联机状态。

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.chinacloudapi.cn
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzureRmAccount -EnvironmentName AzureChinaCloud
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName)).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase -FromPointInTimeBackup -PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName -ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> 完成还原后，即可按[在恢复后配置数据库][在恢复后配置数据库]中的说明配置恢复的数据库。
> 
> 

## <a name="restore-a-deleted-database"></a>还原已删除的数据库
若要还原已删除的数据库，请使用 [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet。

1. 打开 Windows PowerShell。
2. 连接到 Azure 帐户，并列出与帐户关联的所有订阅。
3. 选择包含要还原的已删除数据库的订阅。
4. 获取特定的已删除数据库。
5. 还原已删除的数据库。
6. 验证已还原的数据库是否处于联机状态。

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.chinacloudapi.cn
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzureRmAccount -EnvironmentName AzureChinaCloud
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase -FromDeletedDatabaseBackup -DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName -ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> 完成还原后，即可按[在恢复后配置数据库][在恢复后配置数据库]中的说明配置恢复的数据库。
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a>从 Azure 地理区域还原
若要恢复数据库，请使用 [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet。

> [!NOTE]
> 可以异地还原到“计算优化”性能层！ 若要执行此操作，请将一个“计算优化”ServiceObjectiveName 指定为可选参数。 
>
> 

1. 打开 Windows PowerShell。
2. 连接到 Azure 帐户，并列出与帐户关联的所有订阅。
3. 选择包含要还原的数据库的订阅。
4. 获取要恢复的数据库。
5. 创建对数据库的恢复请求。
6. 验证异地还原的数据库的状态。

```Powershell
Connect-AzureRmAccount -EnvironmentName AzureChinaCloud
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase -FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" -ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>"

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> 若要在完成还原后配置数据库，请参阅[在恢复后配置数据库][在恢复后配置数据库]。
> 
> 

如果源数据库启用了 TDE，则已恢复的数据库会启用 TDE。

## <a name="next-steps"></a>后续步骤
若要了解 Azure SQL 数据库版本的业务连续性功能，请阅读 [Azure SQL 数据库业务连续性概述][Azure SQL Database business continuity overview]。

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
<!-- Not available for support ticket[Request a DTU quota change]: sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change--> [在恢复后配置数据库]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery [如何安装和配置 Azure PowerShell]: https://docs.microsoft.com/powershell/azureps-cmdlets-docs [概述]: ./sql-data-warehouse-restore-database-overview.md [门户]: ./sql-data-warehouse-restore-database-portal.md [PowerShell]: ./sql-data-warehouse-restore-database-powershell.md [REST]: ./sql-data-warehouse-restore-database-rest-api.md [在恢复后配置数据库]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recoveryy

<!--MSDN references-->
[Restore-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase

<!--Other Web references-->
[Azure Portal]: https://portal.azure.cn/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
