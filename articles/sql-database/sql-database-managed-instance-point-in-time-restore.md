---
title: SQL 数据库托管实例 - 时间点还原 | Microsoft Docs
description: 如何将 SQL 托管实例中的数据库还原到以前的某个时间点。
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: sstein, carlrab, mathoma
origin.date: 08/25/2019
ms.date: 09/30/2019
ms.openlocfilehash: d45ea6c5000373f27e9b38cb83bafb0579ba2ecb
ms.sourcegitcommit: 5c3d7acb4bae02c370f6ba4d9096b68ecdd520dd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71263066"
---
# <a name="restore-a-sql-managed-instance-database-to-a-previous-point-in-time"></a>将 SQL 托管实例数据库还原到以前的某个时间点

使用时间点还原 (PITR)，可以创建一个数据库作为另一个数据库在过去某个时间点的副本。 本文介绍如何对托管实例中的数据库执行时间点还原。

可以在恢复方案中使用时间点还原，例如，解决错误导致的事件、错误加载了数据、删除了重要数据和其他问题，以及进行测试或审核。 备份文件将根据数据库设置保留 7 到 35 天。

时间点还原可用于：

- 从现有数据库还原某个数据库。
- 从已删除的数据库还原某个数据库。

此外，对于托管实例，时间点还原可用于： 

- 将数据库还原到同一个托管实例。
- 将数据库还原到另一个托管实例


> [!NOTE]
> 无法对整个托管实例执行时间点还原。 可以做到的并且在本文中所述的时间点还原适用于托管实例上托管的数据库。


## <a name="limitations"></a>限制

还原到另一个托管实例时，两个实例必须位于同一个订阅和区域中。 目前不支持跨区域和跨订阅的还原。

> [!WARNING]
> 请注意托管实例的存储大小 - 根据还原数据的大小，可能会耗尽实例存储。 如果没有足够的空间用于存储还原的数据，请使用替代方法。

下表显示了托管实例的时间点恢复方案：

|           |还原现有数据库| 还原现有数据库|还原已删除的数据库| 还原已删除的数据库|
|:----------|:----------|:----------|:----------|:----------|
|目标| 同一个托管实例|另一个托管实例 |同一个托管实例|另一个托管实例 |
|Azure 门户| 是|否 |否|否|
|Azure CLI|是 |是 |否|否|
|PowerShell| 是|是 |是|是|


## <a name="restore-existing-database"></a>还原现有数据库

使用 Azure 门户、Powershell 或 Azure CLI 将现有数据库还原到同一个实例。 使用 PowerShell 或 Azure CLI 并指定目标托管实例和资源组属性，将数据库还原到另一个实例。 如果未指定这些参数，则数据库默认将还原到现有实例。 目前不支持通过 Azure 门户还原到另一个实例。 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

1. 登录到 [Azure 门户](https://portal.azure.cn)。 
1. 导航到你的托管实例，并选择要还原的数据库。 
1. 在数据库页上选择“还原”。  

    ![还原现有数据库](media/sql-database-managed-instance-point-in-time-restore/restore-database-to-mi.png)

1. 在“还原”页上，在历史记录中选择要将数据库还原到的日期时间点。 
1. 选择“确认”还原该数据库。  这会启动还原过程，期间会创建一个新数据库，并在其中填充原始数据库在所需时间点的数据。 有关恢复过程的详细信息，请参阅[恢复时间](sql-database-recovery-using-backups.md#recovery-time)。 

1. 查找托管实例
1. 选择要还原的数据库
1. 在“数据库”屏幕上，单击“还原”操作
1. 在“还原”屏幕上，在历史记录中选择要将数据库还原到的日期时间点
1. 确认后，还原过程将会启动，期间会根据数据库的大小创建新的数据库，并在其中填充原始数据库在所需时间点的数据。 有关还原过程的持续时间，请查看“使用备份进行恢复”一文。


# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

如果尚未安装 Azure PowerShell，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)。

若要使用 PowerShell 还原数据库，请使用自己的值更新参数，并运行以下命令：

```powershell
$subscriptionId = "<Subscription ID>"
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source-database>"
$pointInTime = "2018-06-27T08:51:39.3882806Z"
$targetDatabase = "<Name of new database to be created>"
 
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId
 
Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                               -InstanceName $managedInstanceName `
                               -Name $databaseName `
                               -PointInTime $pointInTime `
                               -TargetInstanceDatabaseName $targetDatabase `
```

若要将数据库还原到另一个托管实例，请设置目标资源组名称和目标托管实例名称。  

```powershell
$targetResourceGroupName = "<Resource group of target managed instance>"
$targetInstanceName = "<Target managed instance name>"

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
                              -TargetResourceGroupName $targetResourceGroupName `
                              -TargetInstanceName $targetInstanceName 
```

有关详细信息，请参阅 [Restore-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase)。


# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

如果尚未安装 Azure CLI，请参阅[安装 Azure CLI](/cli/install-azure-cli?view=azure-cli-latest)。

若要使用 Azure CLI 还原数据库，请使用自己的值更新参数，并运行以下命令：


```azurecli
az sql midb restore -g mygroupname --mi myinstancename |
-n mymanageddbname --dest-name targetmidbname --time "2018-05-20T05:34:22"
```


若要将数据库还原到另一个托管实例，请设置目标资源组名称和目标托管实例名称。  

```azurecli
az sql midb restore -g mygroupname --mi myinstancename -n mymanageddbname |
       --dest-name targetmidbname --time "2018-05-20T05:34:22" |
       --dest-resource-group mytargetinstancegroupname |
       --dest-mi mytargetinstancename
```

有关可用参数的详细说明，请参阅[托管实例 CLI](/cli/sql/midb?view=azure-cli-latest#az-sql-midb-restore)。 

---

## <a name="restore-a-deleted-database"></a>还原已删除的数据库 
 
只能使用 PowerShell 来还原已删除的数据库。 可将数据库还原到同一个实例或另一个实例。 

若要使用 PowerShell 还原已删除的数据库，请使用自己的值更新参数，并运行以下命令：

```powershell
$subscriptionId = "<Subscription ID>"
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$deletedDatabaseName = "<Source database name>"

$deleted_db = Get-AzSqlDeletedInstanceDatabaseBackup -ResourceGroupName $resourceGroupName `
            -InstanceName $managedInstanceName -DatabaseName $deletedDatabaseName 

$pointInTime = "2018-06-27T08:51:39.3882806Z"
$properties = New-Object System.Object
$properties | Add-Member -type NoteProperty -name CreateMode -Value "PointInTimeRestore"
$properties | Add-Member -type NoteProperty -name RestorePointInTime -Value $pointInTime
$properties | Add-Member -type NoteProperty -name RestorableDroppedDatabaseId -Value $deleted_db.Id
```

若要将已删除的数据库还原到另一个实例，请更改资源组名称和托管实例名称。

位置参数应与资源组和托管实例的位置相匹配。

```powershell
$resourceGroupName = "<Second resource group name>"
$managedInstanceName = "<Second managed instance name>"

$location = "China East 2"

$restoredDBName = "WorldWideImportersPITR"
$resource_id = "subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/managedInstances/$managedInstanceName/databases/$restoredDBName"

New-AzResource -Location $location -Properties $properties `
        -ResourceId $resource_id -ApiVersion "2017-03-01-preview" -Force
```

## <a name="overwrite-existing-database"></a>覆盖现有数据库 
 
若要覆盖现有数据库，还必须执行以下操作：

1. 删除要覆盖的现有数据库。
1. 将时间点还原的数据库重命名为已删除的数据库的名称。 


### <a name="drop-original-database"></a>删除原始数据库 
 
可以使用 Azure 门户、PowerShell 或 Azure CLI 删除数据库。 

还可以通过直接连接到托管实例，启动 SQL Server Management Studio (SSMS) 并运行以下 Transact-SQL (T-SQL) 命令，来删除数据库。

```sql
DROP DATABASE WorldWideImporters;
```

使用以下方法之一连接到托管实例数据库： 

- [SQL 虚拟机](/sql-database/sql-database-managed-instance-configure-vm)
- [点到站点](/sql-database/sql-database-managed-instance-configure-p2s)
- [公共终结点](/sql-database/sql-database-managed-instance-public-endpoint-configure)

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

在 Azure 门户上选择托管实例中的数据库，然后选择“删除”。 

   ![还原现有数据库](media/sql-database-managed-instance-point-in-time-restore/delete-database-from-mi.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

使用以下 PowerShell 命令删除托管实例中的现有数据库： 

```powershell
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source database>"

Remove-AzSqlInstanceDatabase -Name $databaseName -InstanceName $managedInstanceName -ResourceGroupName $resourceGroupName
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

使用以下 Azure CLI 命令删除托管实例中的现有数据库： 

```azurecli
az sql midb delete -g mygroupname --mi myinstancename -n mymanageddbname
```

---


### <a name="alter-new-database-name-to-original"></a>将新数据库名称更改为原始数据库名称

直接连接到托管实例，启动 SQL Server Management Studio，然后执行以下 Transact-SQL (T-SQL) 查询，将已还原数据库的名称更改为你要覆盖的已删除数据库的名称。 


```sql
ALTER WorldWideImportersPITR MODIFY NAME = WorldWideImporters;
```


使用以下方法之一连接到托管实例数据库： 

- [SQL 虚拟机](/sql-database/sql-database-managed-instance-configure-vm)
- [点到站点](/sql-database/sql-database-managed-instance-configure-p2s)
- [公共终结点](/sql-database/sql-database-managed-instance-public-endpoint-configure)

## <a name="next-steps"></a>后续步骤

了解[长期保留](sql-database-long-term-retention.md)和[自动备份](sql-database-automated-backups.md)。 
