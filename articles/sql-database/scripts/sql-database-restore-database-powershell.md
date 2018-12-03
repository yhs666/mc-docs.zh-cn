---
title: PowerShell 示例 - 还原备份 - Azure SQL 数据库 | Microsoft Docs
description: 通过异地冗余备份还原 Azure SQL 数据库的 Azure PowerShell 示例脚本
services: sql-database
documentationcenter: sql-database
author: WenJason
manager: digimobile
editor: carlrab
tags: azure-service-management
ms.assetid: ''
ms.service: sql-database
ms.custom: business continuity, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
origin.date: 09/07/2018
ms.date: 10/15/2018
ms.author: v-jay
ms.openlocfilehash: 3894d81eb3f4a5c45855947cbdc4e70f20f98028
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52649205"
---
# <a name="use-powershell-to-restore-an-azure-sql-database-from-backups"></a>使用 PowerShell 从备份还原 Azure SQL 数据库

此 PowerShell 脚本示例从异地冗余备份还原 Azure SQL 数据库，根据最新备份还原已删除的 Azure SQL 数据库，并将 Azure SQL 数据库还原到特定的时间点。  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

本教程需要 Azure PowerShell 模块 5.7.0 或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 此外，还需要运行 `Connect-AzureRmAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

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

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 | [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlserver) | 创建用于托管数据库或弹性池的逻辑服务器。 | 
| [New-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) | 在逻辑服务器中创建数据库作为单一数据库或入池数据库。 |
| [Get-AzureRmSqlDatabaseGeoBackup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | 获取数据库的异地冗余备份。 |
| [Restore-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase) | 还原 SQL 数据库。 |
| [Remove-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqldatabase) | 删除 Azure SQL 数据库。 |
| [Get-AzureRmSqlDeletedDatabaseBackup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | 获取可以还原的已删除数据库。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
<!--Update_Description: update "Clean up deployment" script-->