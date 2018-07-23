---
title: PowerShell 示例 - 活动异地复制 - 单个 Azure SQL 数据库 | Microsoft Docs
description: 为单个 Azure SQL 数据库设置活动异地复制并进行故障转移的 Azure PowerShell 示例脚本。
services: sql-database
documentationcenter: sql-database
author: forester123
manager: digimobile
editor: carlrab
tags: azure-service-management
ms.assetid: ''
ms.service: sql-database
ms.custom: business continuity
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
origin.date: 04/01/2018
ms.date: 04/17/2018
ms.author: v-johch
ms.openlocfilehash: 4dd54a8a6b2755c48329923873e29db7252bb6cc
ms.sourcegitcommit: 53972dcdef77da92529996667545d2e83716f7e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2018
ms.locfileid: "39143420"
---
# <a name="use-powershell-to-configure-active-geo-replication-for-a-single-azure-sql-database"></a>使用 PowerShell 为单一 Azure SQL 数据库配置活动异地复制

此 PowerShell 脚本示例为单个 Azure SQL 数据库配置活动异地复制，并将其故障转移到 Azure SQL 数据库的辅助副本。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a>示例脚本

```powershell
# Login-AzureRmAccount -EnvironmentName AzureChinaCloud
# Set the resource group name and location for your primary server
$primaryresourcegroupname = "myPrimaryResourceGroup-$(Get-Random)"
$primarylocation = "China North"
# Set the resource group name and location for your secondary server
$secondaryresourcegroupname = "mySecondaryResourceGroup-$(Get-Random)"
$secondarylocation = "China East"
# Set an admin login and password for your servers
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# Set server names - the logical server names have to be unique in the system
$primaryservername = "primary-server-$(Get-Random)"
$secondaryservername = "secondary-server-$(Get-Random)"
# The sample database name
$databasename = "mySampleDatabase"
# The ip address range that you want to allow to access your servers
$primarystartip = "0.0.0.0"
$primaryendip = "0.0.0.0"
$secondarystartip = "0.0.0.0"
$secondaryendip = "0.0.0.0"




# Create two new resource groups
$primaryresourcegroup = New-AzureRmResourceGroup -Name $primaryresourcegroupname -Location $primarylocation
$secondaryresourcegroup = New-AzureRmResourceGroup -Name $secondaryresourcegroupname -Location $secondarylocation

# Create two new logical servers with a system wide unique server name
$primaryserver = New-AzureRmSqlServer -ResourceGroupName $primaryresourcegroupname `
    -ServerName $primaryservername `
    -Location $primarylocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
$secondaryserver = New-AzureRmSqlServer -ResourceGroupName $secondaryresourcegroupname `
    -ServerName $secondaryservername `
    -Location $secondarylocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

# Create a blank database with S0 performance level on the primary server
$database = New-AzureRmSqlDatabase  -ResourceGroupName $primaryresourcegroupname `
    -ServerName $primaryservername `
    -DatabaseName $databasename -RequestedServiceObjectiveName "S0"

# Establish Active Geo-Replication
$database = Get-AzureRmSqlDatabase -DatabaseName $databasename -ResourceGroupName $primaryresourcegroupname -ServerName $primaryservername
$database | New-AzureRmSqlDatabaseSecondary -PartnerResourceGroupName $secondaryresourcegroupname -PartnerServerName $secondaryservername -AllowConnections "All"

# Initiate a planned failover
$database = Get-AzureRmSqlDatabase -DatabaseName $databasename -ResourceGroupName $secondaryresourcegroupname -ServerName $secondaryservername
$database | Set-AzureRmSqlDatabaseSecondary -PartnerResourceGroupName $primaryresourcegroupname -Failover

# Monitor Geo-Replication config and health after failover
$database = Get-AzureRmSqlDatabase -DatabaseName $databasename -ResourceGroupName $secondaryresourcegroupname -ServerName $secondaryservername
$database | Get-AzureRmSqlDatabaseReplicationLink -PartnerResourceGroupName $primaryresourcegroupname -PartnerServerName $primaryservername

# Remove the replication link after the failover
$database = Get-AzureRmSqlDatabase -DatabaseName $databasename -ResourceGroupName $secondaryresourcegroupname -ServerName $secondaryservername
$secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink -PartnerResourceGroupName $primaryresourcegroupname -PartnerServerName $primaryservername
$secondaryLink | Remove-AzureRmSqlDatabaseSecondary

# Clean up deployment 
#Remove-AzureRmResourceGroup -ResourceGroupName $primaryresourcegroupname
#Remove-AzureRmResourceGroup -ResourceGroupName $secondaryresourcegroupname
```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $primaryresourcegroupname
Remove-AzureRmResourceGroup -ResourceGroupName $secondaryresourcegroupname
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | 创建用于托管数据库或弹性池的逻辑服务器。 |
| [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | 在逻辑服务器中创建弹性池。 |
| [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) | 更新数据库属性，或者将数据库移入、移出弹性池或在弹性池之间移动。 |
| [New-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| 为现有数据库创建辅助数据库，并开始数据复制。 |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)| 获取一个或多个数据库。 |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| 将辅助数据库切换为主数据库，启动故障转移。|
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | 获取 Azure SQL 数据库和资源组或 SQL Server 之间的异地复制链路。 |
| [Remove-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | 终止 SQL 数据库和指定的辅助数据库之间的数据复制。 |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
