---
title: Azure PowerShell 脚本示例 - 将应用连接到 SQL 数据库 | Azure Docs
description: Azure PowerShell 脚本示例 - 将应用服务应用连接到 SQL 数据库
services: app-service\web
documentationcenter: ''
author: syntaxc4
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 055440a9-fff1-49b2-b964-9c95b364e533
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
origin.date: 03/20/2017
ms.date: 01/21/2019
ms.author: v-biyu
ms.custom: mvc
ms.openlocfilehash: 2327241a11619eaa6f9312e55a17eed584ee68d8
ms.sourcegitcommit: 90d5f59427ffa599e8ec005ef06e634e5e843d1e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083837"
---
# <a name="connect-an-app-service-app-to-a-sql-database"></a>将应用服务应用连接到 SQL 数据库

在此方案中，将了解如何创建 Azure SQL 数据库和应用服务应用。 然后，将使用应用设置将 SQL 数据库链接到应用。

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azure/overview)中的说明安装 Azure PowerShell，并运行 `Login-AzureRmAccount -EnvironmentName AzureChinaCloud` 创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```powershell

# Generates a Random Value
$Random=(New-Guid).ToString().Substring(0,8)

# Variables
$ResourceGroup="MyResourceGroup$Random"
$AppName="webappwithSQL$Random"
$Location="China North"
$ServerName="webappwithsql$Random"
$StartIP="0.0.0.0"
$EndIP="0.0.0.0"
$Username="ServerAdmin"
$Password="<provide-a-password>"
$SqlServerPassword=New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $Username,(ConvertTo-SecureString -String $Password -AsPlainText -Force)

# Create a Resource Group
New-AzureRMResourceGroup -Name $ResourceGroup -Location $Location

# Create an App Service Plan
New-AzureRMAppservicePlan -Name WebAppwithSQLPlan -ResourceGroupName $ResourceGroup -Location $Location -Tier Basic

# Create a Web App in the App Service Plan
New-AzureRMWebApp -Name $AppName -ResourceGroupName $ResourceGroup -Location $Location -AppServicePlan WebAppwithSQLPlan

# Create a SQL Database Server
New-AzureRMSQLServer -ServerName $ServerName -Location $Location -SqlAdministratorCredentials $SqlServerPassword -ResourceGroupName $ResourceGroup

# Create Firewall Rule for SQL Database Server
New-AzureRmSqlServerFirewallRule -FirewallRuleName "AllowYourIp" -StartIpAddress $StartIP -EndIPAddress $EndIP -ServerName $ServerName -ResourceGroupName $ResourceGroup

# Create SQL Database in SQL Database Server
New-AzureRMSQLDatabase -ServerName $ServerName -DatabaseName MySampleDatabase -ResourceGroupName $ResourceGroup

# Assign Connection String to Connection String 
Set-AzureRMWebApp -ConnectionStrings @{ MyConnectionString = @{ Type="SQLAzure"; Value ="Server=tcp:$ServerName.database.chinacloudapi.cn;Database=MySampleDatabase;User ID=$Username@$ServerName;Password=$password;Trusted_Connection=False;Encrypt=True;" } } -Name $AppName -ResourceGroupName $ResourceGroup
```

## <a name="clean-up-deployment"></a>清理部署 

运行脚本示例后，可以使用以下命令删除资源组、应用服务应用以及所有相关资源。

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmAppServicePlan](https://docs.microsoft.com/powershell/module/azurerm.websites/new-azurermappserviceplan) | 创建应用服务计划。 |
| [New-AzureRmWebApp](https://docs.microsoft.com/powershell/module/azurerm.websites/new-azurermwebapp) | 创建 Web 应用。 |
| [New-AzureRMSQLServer](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlserver) | 创建 SQL 数据库服务器。 |
| [New-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | 为 SQL 数据库服务器创建防火墙规则。 |
| [New-AzureRMSQLDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) | 创建数据库或弹性数据库。 |
| [Set-AzureRmWebApp](https://docs.microsoft.com/powershell/module/azurerm.websites/set-azurermwebapp) | 修改 Web 应用的配置。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../samples-powershell.md)中找到 Azure 应用服务的其他 Azure Powershell 示例。
