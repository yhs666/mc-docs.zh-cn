---
title: PowerShell 示例 - 复制 - Azure SQL 数据库 - 新服务器 | Microsoft Docs
description: 将 SQL 数据库复制到新服务器的 Azure PowerShell 示例脚本
services: sql-database
documentationcenter: sql-database
author: WenJason
manager: digimobile
editor: carlrab
tags: azure-service-management
ms.assetid: ''
ms.service: sql-database
ms.custom: load & move data, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
origin.date: 09/07/2018
ms.date: 10/15/2018
ms.author: v-jay
ms.openlocfilehash: a86f15e24a28ff1c7eb6fa8b365fcef71d9c8cb4
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52653867"
---
# <a name="use-powershell-to-copy-a-sql-database-to-a-new-server"></a>使用 PowerShell 将 SQL 数据库复制到新服务器

此 PowerShell 脚本示例在新服务器中创建现有数据库的副本。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

本教程需要 Azure PowerShell 模块 5.7.0 或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 此外，还需要运行 `Connect-AzureRmAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="copy-a-database-to-a-new-server"></a>将数据库复制到新服务器

```powershell
# Login-AzureRmAccount -EnvironmentName AzureChinaCloud
# Set the resource group name and location for your source server
$sourceresourcegroupname = "mySourceResourceGroup-$(Get-Random)"
$sourcelocation = "China East"
# Set the resource group name and location for your target server
$targetresourcegroupname = "myTargetResourceGroup-$(Get-Random)"
$targetlocation = "China East"
# Set an admin login and password for your server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The logical server names have to be unique in the system
$sourceservername = "source-server-$(Get-Random)"
$targetservername = "target-server-$(Get-Random)"
# The sample database name
$sourcedatabasename = "mySampleDatabase"
$targetdatabasename = "CopyOfMySampleDatabase"
# The ip address range that you want to allow to access your servers
$sourcestartip = "0.0.0.0"
$sourceendip = "0.0.0.0"
$targetstartip = "0.0.0.0"
$targetendip = "0.0.0.0"

# Create two new resource groups
$sourceresourcegroup = New-AzureRmResourceGroup -Name $sourceresourcegroupname -Location $sourcelocation
$targetresourcegroup = New-AzureRmResourceGroup -Name $targetresourcegroupname -Location $targetlocation

# Create a server with a system wide unique server name
$sourceresourcegroup = New-AzureRmSqlServer -ResourceGroupName $sourceresourcegroupname `
    -ServerName $sourceservername `
    -Location $sourcelocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
$targetresourcegroup = New-AzureRmSqlServer -ResourceGroupName $targetresourcegroupname `
    -ServerName $targetservername `
    -Location $targetlocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

# Create a server firewall rule that allows access from the specified IP range
$sourceserverfirewallrule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $sourceresourcegroupname `
    -ServerName $sourceservername `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $sourcestartip -EndIpAddress $sourceendip
$targetserverfirewallrule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $targetresourcegroupname `
    -ServerName $targetservername `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $targetstartip -EndIpAddress $targetendip

# Create a blank database in the source-server with an S0 performance level
$sourcedatabase = New-AzureRmSqlDatabase  -ResourceGroupName $sourceresourcegroupname `
    -ServerName $sourceservername `
    -DatabaseName $sourcedatabasename -RequestedServiceObjectiveName "S0"

# Copy source database to the target server 
$databasecopy = New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceresourcegroupname `
    -ServerName $sourceservername `
    -DatabaseName $sourcedatabasename `
    -CopyResourceGroupName $targetresourcegroupname `
    -CopyServerName $targetservername `
    -CopyDatabaseName $targetdatabasename 

# Clean up deployment 
# Remove-AzureRmResourceGroup -ResourceGroupName $sourceresourcegroupname
# Remove-AzureRmResourceGroup -ResourceGroupName $targetresourcegroupname
```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $sourceresourcegroupname
Remove-AzureRmResourceGroup -ResourceGroupName $targetresourcegroupname
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlserver) | 创建用于托管数据库或弹性池的逻辑服务器。 |
| [New-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) | 在逻辑服务器中创建数据库作为单一数据库或入池数据库。 |
| [New-AzureRmSqlDatabaseCopy](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) | 创建当前使用快照的数据库副本。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
<!--Update_Description: update "Clean up deployment" script-->