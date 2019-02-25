---
title: PowerShell 示例 - 在 Azure SQL 数据库中创建单一数据库 | Microsoft Docs
description: 在 Azure SQL 数据库中创建单一数据库的 Azure PowerShell 示例脚本
services: sql-database
ms.service: sql-database
ms.subservice: standalone-database
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: craigg
origin.date: 01/25/2019
ms.date: 02/25/2019
ms.openlocfilehash: 5000e5237a54b1f2b01a55e363363a14fc89dc7a
ms.sourcegitcommit: 5ea744a50dae041d862425d67548a288757e63d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56663649"
---
# <a name="use-powershell-to-create-a-single-azure-sql-database-and-configure-a-sql-database-server-firewall-rule"></a>使用 PowerShell 创建单个 Azure SQL 数据库并配置 SQL 数据库服务器防火墙规则

此 PowerShell 脚本示例创建 Azure SQL 数据库，并配置服务器级防火墙规则。 成功运行该脚本后，可以通过所有 Azure 服务和配置的 IP 地址访问 SQL 数据库。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

本教程需要 Azure PowerShell 模块 5.7.0 或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)（安装 Azure PowerShell 模块）。 此外，还需要运行 `Connect-AzureRmAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```powershell
# Login-AzureRmAccount -EnvironmentName AzureChinaCloud
# Set the resource group name and location for your server
$resourcegroupname = "myResourceGroup-$(Get-Random)"
$location = "China East"
# Set an admin login and password for your server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# Set server name - the logical server name has to be unique in the system
$servername = "server-$(Get-Random)"
# The sample database name
$databasename = "mySampleDatabase"
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
$serverfirewallrule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startip -EndIpAddress $endip

# Create a blank database with an S0 performance level
$database = New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -RequestedServiceObjectiveName "S0" `
    -SampleName "AdventureWorksLT"

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
| [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlserver) | 创建托管单一数据库或弹性池的 SQL 数据库服务器。 |
| [New-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | 创建一个防火墙规则，以允许从输入的 IP 地址范围访问服务器上的所有 SQL 数据库。 |
| [New-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) | 创建单一数据库或弹性池。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
