---
title: PowerShell 示例 - 监视 - 缩放 - 弹性池 - Azure SQL 数据库 | Microsoft Docs
description: 用于在 Azure SQL 数据库中监视和缩放弹性池的 Azure PowerShell 脚本示例
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
manager: digimobile
origin.date: 01/25/2019
ms.date: 02/25/2019
ms.openlocfilehash: 58deeafeea085605d93cfc0c7db07a18068b05a1
ms.sourcegitcommit: 5ea744a50dae041d862425d67548a288757e63d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56663627"
---
# <a name="use-powershell-to-monitor-and-scale-an-elastic-pool-in-azure-sql-database"></a>使用 PowerShell 在 Azure SQL 数据库中监视和缩放弹性池

此 PowerShell 脚本示例监视弹性池的性能指标，将其扩展到更高的计算大小，并基于性能指标之一创建警报规则。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

本教程需要 Azure PowerShell 模块 5.7.0 或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps)（安装 Azure PowerShell 模块）。 此外，还需要运行 `Connect-AzureRmAccount -EnvironmentName AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="sample-script"></a>示例脚本

```powershell
# Login-AzureRmAccount -EnvironmentName AzureChinaCloud
# Set the resource group name and location for your server
$resourcegroupname = "myResourceGroup-$(Get-Random)"
$location = "China East"
# Set elastic pool names
$poolname = "MySamplePool"
# Set an admin login and password for your database
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

# Create elastic database pool
$elasticpool = New-AzureRmSqlElasticPool -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -ElasticPoolName $poolname `
    -Edition "Standard" `
    -Dtu 50 `
    -DatabaseDtuMin 10 `
    -DatabaseDtuMax 50

# Create a server firewall rule that allows access from the specified IP range
$serverfirewallrule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startip -EndIpAddress $endip

# Create two blank database in the pool
$firstdatabase = New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $firstdatabasename `
    -ElasticPoolName $poolname
$seconddatabase = New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $seconddatabasename `
    -ElasticPoolName $poolname

# Monitor the pool
$monitorparameters = @{
  ResourceId = "/subscriptions/$($(Get-AzureRmContext).Subscription.Id)/resourceGroups/$resourcegroupname/providers/Microsoft.Sql/servers/$servername/elasticPools/$poolname"
  TimeGrain = [TimeSpan]::Parse("00:05:00")
  MetricNames = "dtu_consumption_percent"
}
(Get-AzureRmMetric @monitorparameters -DetailedOutput).MetricValues

# Scale the pool
$elasticpool = Set-AzureRmSqlElasticPool -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -ElasticPoolName $poolname `
    -Edition "Standard" `
    -Dtu 100 `
    -DatabaseDtuMin 20 `
    -DatabaseDtuMax 100

# Add an alert that fires when the pool utilization reaches 90%
Add-AzureRmMetricAlertRule -ResourceGroup $resourcegroupname `
    -Name "mySampleAlertRule" `
    -Location $location `
    -TargetResourceId "/subscriptions/$($(Get-AzureRmContext).Subscription.Id)/resourceGroups/$resourcegroupname/providers/Microsoft.Sql/servers/$servername/elasticPools/$poolname" `
    -MetricName "dtu_consumption_percent" `
    -Operator "GreaterThan" `
    -Threshold 90 `
    -WindowSize $([TimeSpan]::Parse("00:05:00")) `
    -TimeAggregationOperator "Average" `
    -Actions $(New-AzureRmAlertRuleEmail -SendToServiceOwners)

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
 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlserver) | 创建托管单一数据库或弹性池的 SQL 数据库服务器。 |
| [New-AzureRmSqlElasticPool](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | 在逻辑服务器中创建弹性池。 |
| [New-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabase) | 创建单一数据库或创建弹性池中的数据库。 |
| [Get-AzureRmMetric](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermmetric) | 显示数据库的大小使用情况信息。|
| [Add-AzureRMMetricAlertRule](https://docs.microsoft.com/powershell/module/azurerm.insights/add-azurermmetricalertrule) | 添加或更新基于指标的警报规则。 |
| [Set-AzureRmSqlElasticPool](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlelasticpool) | 更新弹性池属性 |
| [Add-AzureRMMetricAlertRule](https://docs.microsoft.com/powershell/module/azurerm.insights/add-azurermmetricalertrule) | 设置警报规则，以便在将来自动监视 DTU。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
<!--Update_Description: update "Clean up deployment" script-->