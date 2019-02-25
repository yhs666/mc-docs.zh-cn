---
title: PowerShell 示例-审核-威胁检测-Azure SQL 数据库 | Microsoft Docs
description: 在 Azure SQL 数据库中配置审核和威胁检测的 Azure PowerShell 示例脚本
services: sql-database
ms.service: sql-database
ms.subservice: threat-detection
ms.custom: security
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
manager: craigg
origin.date: 01/25/2019
ms.date: 02/25/2019
ms.openlocfilehash: bc5c394d619a93fa2902505320a58a33fc363c7c
ms.sourcegitcommit: 5ea744a50dae041d862425d67548a288757e63d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56663428"
---
# <a name="use-powershell-to-configure-sql-database-auditing-and-threat-detection"></a>使用 PowerShell 配置 SQL 数据库审核和威胁检测

此 PowerShell 脚本示例配置 SQL 数据库审核和威胁检测。

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
# The logical server name has to be unique in the system
$servername = "server-$(Get-Random)"
# The sample database name
$databasename = "mySampleDatabase"
# The ip address range that you want to allow to access your server
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The storage account name has to be unique in the system
$storageaccountname = $("sql$(Get-Random)")
# Specify the email recipients for the threat detection alerts
$notificationemailreceipient = "changeto@your.email;changeto@your.email"

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

# Create a blank database with S0 performance level
$database = New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename -RequestedServiceObjectiveName "S0"
    
# Create a Storage Account 
$storageaccount = New-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -AccountName $storageaccountname `
    -Location $location `
    -Type "Standard_LRS"

# Set an auditing policy
Set-AzureRmSqlDatabaseAuditing -State Enabled `
    -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -StorageAccountName $storageaccountname 

# Set a threat detection policy
Set-AzureRmSqlDatabaseThreatDetectionPolicy -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -StorageAccountName $storageaccountname `
    -NotificationRecipientsEmails $notificationemailreceipient `
    -EmailAdmins $False

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
| [New-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) | 创建单一数据库或弹性池。 |
| [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/new-azurermstorageaccount) | 创建存储帐户。 |
| [Set-AzureRmSqlDatabaseAuditingPolicy](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabaseauditingpolicy) | 设置数据库的审核策略。 |
| [Set-AzureRmSqlDatabaseThreatDetectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasethreatdetectionpolicy) | 在数据库上设置威胁检测策略。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
<!--Update_Description: wording update-->