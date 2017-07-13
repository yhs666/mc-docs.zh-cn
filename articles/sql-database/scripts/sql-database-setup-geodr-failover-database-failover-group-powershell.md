---
title: "Azure PowerShell 脚本 - 设置异地复制故障转移组 - 单个 SQL 数据库 | Azure"
description: "Azure PowerShell 脚本示例 - 使用 PowerShell 为单个 Azure SQL 数据库设置活动异地复制"
services: sql-database
documentationcenter: sql-database
author: Hayley244
manager: digimobile
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
origin.date: 05/26/2017
ms.date: 07/03/2017
ms.author: v-johch
ms.openlocfilehash: 4d35a9a96589b3b5d8c366d20847c0023f2d9523
ms.sourcegitcommit: 73b1d0f7686dea85647ef194111528c83dbec03b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2017
---
# 使用 PowerShell 为单个 Azure SQL 数据库配置活动异地复制故障转移组
<a id="configure-an-active-geo-replication-failover-group-for-a-single-azure-sql-database-using-powershell" class="xliff"></a>

此示例 PowerShell 脚本为单一数据库配置活动异地复制故障转移组，并将其故障转移到次要副本。

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## 示例脚本
<a id="sample-scripts" class="xliff"></a>

```powershell
# Login-AzureRmAccount
# Set the resource group name and location for your primary server
$primaryresourcegroupname = "myPrimaryResourceGroup-$(Get-Random)"
$primarylocation = "China East"
# Set the resource group name and location for your secondary server
$secondarylocation = "China North"
# Set an admin login and password for your servers
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# Set failover group name - the failover group name has to be unique in the system
$failovergroupname = "failovergroupname-$(Get-Random)"
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

# Create new resource group
$primaryresourcegroup = New-AzureRmResourceGroup -Name $primaryresourcegroupname -Location $primarylocation
$primaryresourcegroup
# Create two new logical servers with a system wide unique server name
$primaryserver = New-AzureRmSqlServer -ResourceGroupName $primaryresourcegroupname `
    -ServerName $primaryservername `
    -Location $primarylocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
$primaryserver
$secondaryserver = New-AzureRmSqlServer -ResourceGroupName $primaryresourcegroupname `
    -ServerName $secondaryservername `
    -Location $secondarylocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
$secondaryserver
# Create a server firewall rule that allows access from the specified IP range
$primaryserverfirewallrule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $primaryresourcegroupname `
    -ServerName $primaryservername `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $primarystartip -EndIpAddress $primaryendip
$primaryserverfirewallrule
$secondaryserverfirewallrule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $primaryresourcegroupname `
    -ServerName $secondaryservername `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $secondarystartip -EndIpAddress $secondaryendip
$secondaryserverfirewallrule
# Create a blank database with S0 performance level on the primary server
$database = New-AzureRmSqlDatabase  -ResourceGroupName $primaryresourcegroupname `
    -ServerName $primaryservername `
    -DatabaseName $databasename -RequestedServiceObjectiveName "S0"
$database
# Create failover group
$fileovergroup = New-AzureRMSqlDatabaseFailoverGroup `
      -ResourceGroupName $primaryresourcegroupname `
      -ServerName $primaryservername `
      -PartnerServerName $secondaryservername  `
      -FailoverGroupName $failovergroupname `
      -FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
$fileovergroup
# Add database to failover group
$fileovergroup = Get-AzureRmSqlDatabase `
   -ResourceGroupName $primaryresourcegroupname `
   -ServerName $primaryservername `
   -DatabaseName $databasename | `
   Add-AzureRmSqlDatabaseToFailoverGroup `
   -ResourceGroupName $primaryresourcegroupname ` `
   -ServerName $primaryservername `
   -FailoverGroupName $failovergroupname
$fileovergroup
# Initiate a planned failover
Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $primaryresourcegroupname  `
   -ServerName $secondaryservername `
   -FailoverGroupName $failovergroupname 

# Monitor Geo-Replication config and health after failover
Get-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $primaryresourcegroupname  `
   -ServerName $primaryservername 
Get-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $primaryresourcegroupname  `
   -ServerName $secondaryservername 

# Remove the replication link after the failover
Remove-AzureRmSqlDatabaseFailoverGroup `
   -ResourceGroupName $primaryresourcegroupname  `
   -ServerName $secondaryservername `
   -FailoverGroupName $failovergroupname

# Clean up deployment 
# Remove-AzureRmResourceGroup -ResourceGroupName $primaryresourcegroupname

```

## 清理部署
<a id="clean-up-deployment" class="xliff"></a>

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myPrimaryResourceGroup"
Remove-AzureRmResourceGroup -ResourceGroupName "mySecondaryResourceGroup"
```

## 脚本说明
<a id="script-explanation" class="xliff"></a>

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlserver) | 创建用于托管数据库或弹性池的逻辑服务器。 |
| [New-AzureRmSqlElasticPool](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | 在逻辑服务器中创建弹性池。 |
| [Set-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase) | 更新数据库属性，或者将数据库移入、移出弹性池或在弹性池之间移动。 |
| [New-AzureRmSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| 为现有数据库创建辅助数据库，并开始数据复制。 |
| [Get-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabase)| 获取一个或多个数据库。 |
| [Set-AzureRmSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| 将辅助数据库切换为主数据库，启动故障转移。|
| [Get-AzureRmSqlDatabaseReplicationLink](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | 获取 Azure SQL 数据库和资源组或 SQL Server 之间的异地复制链路。 |
| [Remove-AzureRmSqlDatabaseSecondary](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | 终止 SQL 数据库和指定的辅助数据库之间的数据复制。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## 后续步骤
<a id="next-steps" class="xliff"></a>

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。