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
origin.date: 03/07/2019
ms.date: 03/25/2019
ms.openlocfilehash: 27e16c262488c0137b631f8f2698d35440651e58
ms.sourcegitcommit: 02c8419aea45ad075325f67ccc1ad0698a4878f4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318986"
---
# <a name="use-powershell-to-configure-sql-database-auditing-and-threat-detection"></a>使用 PowerShell 配置 SQL 数据库审核和威胁检测

此 PowerShell 脚本示例配置 SQL 数据库审核和威胁检测。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

本教程需要 AZ PowerShell 1.4.0 或更高版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps)（安装 Azure PowerShell 模块）。 此外，还需要运行 `Connect-AzAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```powershell
# Connect-AzAccount -Environment AzureChinaCloud
# The SubscriptionId in which to create these objects
$SubscriptionId = ''
# Set the resource group name and location for your server
$resourceGroupName = "myResourceGroup-$(Get-Random)"
$location = "chinaeast"
# Set an admin login and password for your server
$adminSqlLogin = "SqlAdmin"
$password = "ChangeYourAdminPassword1"
# The logical server name has to be unique in the system
$serverName = "server-$(Get-Random)"
# The sample database name
$databaseName = "mySampleDatabase"
# The ip address range that you want to allow to access your server
$startIp = "0.0.0.0"
$endIp = "0.0.0.0"
# The storage account name has to be unique in the system
$storageAccountName = $("sql$(Get-Random)")
# Specify the email recipients for the threat detection alerts
$notificationEmailReceipient = "changeto@your.email;changeto@your.email"

# Set subscription 
Set-AzContext -SubscriptionId $subscriptionId 

# Create a new resource group
$resourceGroup = New-AzResourceGroup -Name $resourceGroupName -Location $location

# Create a new server with a system wide unique server name
$server = New-AzSqlServer -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminSqlLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

# Create a server firewall rule that allows access from the specified IP range
$serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp

# Create a blank database with S0 performance level
$database = New-AzSqlDatabase  -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $databaseName -RequestedServiceObjectiveName "S0"
    
# Create a Storage Account 
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroupName `
    -AccountName $storageAccountName `
    -Location $location `
    -Type "Standard_LRS"

# Set an auditing policy
Set-AzSqlDatabaseAuditing -State Enabled `
    -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $databaseName `
    -StorageAccountName $storageAccountName 

# Set a threat detection policy
Set-AzSqlDatabaseThreatDetectionPolicy -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $databaseName `
    -StorageAccountName $storageAccountName `
    -NotificationRecipientsEmails $notificationEmailReceipient `
    -EmailAdmins $False
```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserver) | 创建托管单一数据库或弹性池的 SQL 数据库服务器。 |
| [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase) | 创建单一数据库或弹性池。 |
| [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) | 创建存储帐户。 |
| [Set-AzSqlDatabaseAuditingPolicy](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabaseauditingpolicy) | 设置数据库的审核策略。 |
| [Set-AzSqlDatabaseThreatDetectionPolicy](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasethreatdetectionpolicy) | 在数据库上设置威胁检测策略。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
<!--Update_Description: wording update-->