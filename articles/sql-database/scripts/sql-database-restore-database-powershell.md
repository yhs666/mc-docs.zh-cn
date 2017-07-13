---
title: "Azure PowerShell 脚本 - 还原 SQL 数据库 | Azure"
description: "Azure PowerShell 脚本示例 - 使用 PowerShell 还原 SQL 数据库"
services: sql-database
documentationcenter: sql-database
author: Hayley244
manager: digimobile
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: sample
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
origin.date: 05/23/2017
ms.date: 07/03/2017
ms.author: v-johch
ms.openlocfilehash: 34f85324a37b1c7b58c0470a2139c69c61f7a7b1
ms.sourcegitcommit: bb82041119027be7a62fc96945d92a8a452e7849
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2017
---
# 使用 PowerShell 还原 SQL 数据库
<a id="restore-a-sql-database-using-powershell" class="xliff"></a>

此示例 PowerShell 脚本从异地冗余备份还原 Azure SQL 数据库，并将已删除的数据库还原到最新备份。  

在运行此脚本前，请确保已使用 `Add-AzureRmAccount -EnviroumentName AzureChina` cmdlet 创建与 Azure 的连接。

## 示例脚本
<a id="sample-script" class="xliff"></a>

```powershell
# Login-AzureRmAccount -EnvironmentName AzureChinaCloud
# Set the resource group name and location for your server
$resourcegroupname = "myResourceGroup-$(Get-Random)"
$location = "China North"
# Set an admin login and password for your server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# Set server name - the logical server name has to be unique in the system
$servername = "server-$(Get-Random)"
# The sample database name
$databasename = "mySampleDatabase"
# The restored database names
$georestoredatabasename = "MySampleDatabase_GeoRestore"
$pointintimerestoredatabasename = "MySampleDatabase_10MinutesAgo"
$deleteddatabaserestorename = "MySampleDatabase_DeletedRestore"
# The ip address range that you want to allow to access your server
$startip = "0.0.0.0"
$endip = "0.0.0.0"

# Create a resource group
$resourcegroup = New-AzureRmResourceGroup -Name $resourcegroupname -Location $location

# Create a server with a system wide unique server name
$server = New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

# Create a server firewall rule that allows access from the specified IP range
$firewallrule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startip -EndIpAddress $endip

# Create a blank database with an S0 performance level
$database = New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -RequestedServiceObjectiveName "S0" 

# Restore database from latest geo-redundant backup into existing server 
# Note: Check to see that backups are created and ready to restore from geo-redundant backup
# Important: If no backup exists, you will get an error indicating that no backups exist for the server specified
Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName $resourcegroupname -ServerName $servername 
Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName $resourcegroupname -ServerName $servername -DatabaseName $databasename
# Do not continue until a backup exists
Restore-AzureRmSqlDatabase `
    -FromGeoBackup `
    -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -TargetDatabaseName $georestoredatabasename `
    -ResourceId $database.ResourceID `
    -Edition "Standard" `
    -ServiceObjectiveName "S0"

# Restore database to its state 10 minutes ago
# Note: Point-in-time restore requires database to be at least 5 minutes old
Restore-AzureRmSqlDatabase `
      -FromPointInTimeBackup `
      -PointInTime (Get-Date).AddMinutes(-10) `
      -ResourceGroupName $resourcegroupname `
      -ServerName $servername `
      -TargetDatabaseName $pointintimerestoredatabasename `
      -ResourceId $database.ResourceID `
      -Edition "Standard" `
      -ServiceObjectiveName "S0"

# Delete original database
Remove-AzureRmSqlDatabase -ResourceGroupName $resourcegroupname -ServerName $servername -DatabaseName $databasename

# Restore deleted database 
# Note: Check to see that the Get-AzureRmSqlDeletedDatabaseBackup cmdlet returns a deletion date (may take a few minutes). 
# Important: If no backup exists, no value will be returned.
$deleteddatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourcegroupname -ServerName $servername -DatabaseName $databasename
$deleteddatabase
# Do not continue until the cmdlet returns information about the deleted database.
Restore-AzureRmSqlDatabase -FromDeletedDatabaseBackup `
    -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -TargetDatabaseName $deleteddatabaserestorename `
    -ResourceId $deleteddatabase.ResourceID `
    -DeletionDate $deleteddatabase.DeletionDate `
    -Edition "Standard" `
    -ServiceObjectiveName "S0"

# Clean up deployment 
# Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## 清理部署
<a id="clean-up-deployment" class="xliff"></a>

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## 脚本说明
<a id="script-explanation" class="xliff"></a>

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 | [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlserver) | 创建用于托管数据库或弹性池的逻辑服务器。 | 
| [New-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) | 在逻辑服务器中创建数据库作为单一数据库或入池数据库。 |
[Get-AzureRmSqlDatabaseGeoBackup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | 获取数据库的异地冗余备份。 |
| [Restore-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase) | 还原 SQL 数据库。 |
|[Remove-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqldatabase) | 删除 Azure SQL 数据库。 |
| [Get-AzureRmSqlDeletedDatabaseBackup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | 获取可以还原的已删除数据库。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |

## 后续步骤
<a id="next-steps" class="xliff"></a>

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
