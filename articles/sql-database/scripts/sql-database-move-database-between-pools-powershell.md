---
title: PowerShell 示例 - 移动 Azure SQL 数据库 - 弹性池 | Microsoft Docs
description: 使用 PowerShell 在弹性池之间移动 SQL 数据库的 Azure PowerShell 示例脚本
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.reviewer: ''
ms.author: v-jay
manager: digimobile
origin.date: 02/08/2019
ms.date: 03/11/2019
ms.openlocfilehash: 74d50238d159b08236ee6b7efbe630e6e844ebf9
ms.sourcegitcommit: 0ccbf718e90bc4e374df83b1460585d3b17239ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2019
ms.locfileid: "57347180"
---
# <a name="use-powershell-to-create-elastic-pools-and-move-databases-between-elastic-pools"></a>使用 PowerShell 创建弹性池，并在弹性池之间移动数据库

此 PowerShell 脚本示例创建两个弹性池，将数据库从一个弹性池移到另一个弹性池中，再将数据库移出弹性池，以实现单一数据库计算大小。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

本教程需要 Azure PowerShell 模块 5.7.0 或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps)（安装 Azure PowerShell 模块）。 此外，还需要运行 `Connect-AzureRmAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```powershell
# Login-AzureRmAccount -EnvironmentName AzureChinaCloud
# Set the resource group name and location for your server
$resourcegroupname = "myResourceGroup-$(Get-Random)"
$location = "China East"
# Set elastic poool names
$firstpoolname = "MyFirstPool"
$secondpoolname = "MySecondPool"
# Set an admin login and password for your server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The logical server name has to be unique in the system
$servername = "server-$(Get-Random)"
# The sample database names
$firstdatabasename = "myFirstSampleDatabase"
$seconddatabasename = "mySecondSampleDatabase"
# The ip address range that you want to allow to access your server
$startip = "0.0.0.0"
$endip = "0.0.0.0"

# Create a new resource group
$resourcegroup = New-AzureRmResourceGroup -Name $resourcegroupname -Location $location

# Create a new server with a system wide unique server name
$server = New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

# Create a server firewall rule that allows access from the specified IP range
$serverfirewallrule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startip -EndIpAddress $endip

# Create two elastic database pools
$firstpool = New-AzureRmSqlElasticPool -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -ElasticPoolName $firstpoolname `
    -Edition "Standard" `
    -Dtu 50 `
    -DatabaseDtuMin 10 `
    -DatabaseDtuMax 20
$secondpool = New-AzureRmSqlElasticPool -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -ElasticPoolName $secondpoolname `
    -Edition "Standard" `
    -Dtu 50 `
    -DatabaseDtuMin 10 `
    -DatabaseDtuMax 50

# Create two blank databases in the first pool
$firstdatabase = New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $firstdatabasename `
    -ElasticPoolName $firstpoolname
$seconddatabase = New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName "MySecondSampleDatabase" `
    -ElasticPoolName "MySecondPool"

# Move the database to the second pool
$firstdatabase = Set-AzureRmSqlDatabase -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $firstdatabasename `
    -ElasticPoolName $secondpoolname

# Move the database into a standalone performance level
$firstdatabase = Set-AzureRmSqlDatabase -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $firstdatabasename `
    -RequestedServiceObjectiveName "S0"

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
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlserver) | 创建用于承载单一数据库或弹性池的 SQL 数据库服务器。 |
| [New-AzureRmSqlElasticPool](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | 创建弹性池。 |
| [New-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) | 在 SQL 数据库服务器中创建数据库作为单一数据库或入池数据库。 |
| [Set-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase) | 更新数据库属性，或者将数据库移入、移出弹性池或在弹性池之间移动。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
<!--Update_Description: update "Clean up deployment" script-->