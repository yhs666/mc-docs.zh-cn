---
title: 将 BACPAC 文件导入 Azure SQL 数据库的PowerShell 示例 | Microsoft Docs
description: 将 BACPAC 文件导入 SQL 数据库的 Azure PowerShell 示例脚本
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: load & move data
ms.devlang: PowerShell
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: carlrab
manager: digimobile
origin.date: 01/25/2019
ms.date: 02/25/2019
ms.openlocfilehash: b88c3e8ad82c4a59c0a87fe992891ef1f4f0e270
ms.sourcegitcommit: 5ea744a50dae041d862425d67548a288757e63d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56663439"
---
# <a name="use-powershell-to-import-a-bacpac-file-into-an-azure-sql-database"></a>使用 PowerShell 将 BACPAC 文件导入 Azure SQL 数据库

以下 PowerShell 脚本示例将数据库从 BACPAC 文件导入 Azure SQL 数据库。  

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
$databasename = "myImportedDatabase"
# The storage account name and storage container name
$storageaccountname = "sqlimport$(Get-Random)"
$storagecontainername = "importcontainer$(Get-Random)"
# BACPAC file name
$bacpacfilename = "sample.bacpac"
# The ip address range that you want to allow to access your server
$startip = "0.0.0.0"
$endip = "0.0.0.0"

# Create a resource group
$resourcegroup = New-AzureRmResourceGroup -Name $resourcegroupname -Location $location

# Create a storage account 
$storageaccount = New-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -AccountName $storageaccountname `
    -Location $location `
    -Type "Standard_LRS"

# Create a storage container 
$storagecontainer = New-AzureStorageContainer -Name $storagecontainername `
    -Context $(New-AzureStorageContext -StorageAccountName $storageaccountname `
        -StorageAccountKey $(Get-AzureRmStorageAccountKey -ResourceGroupName $resourcegroupname -StorageAccountName $storageaccountname).Value[0])

# Download sample database from Github
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 #required by Github
Invoke-WebRequest -Uri "https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bacpac" -OutFile $bacpacfilename

# Upload sample database into storage container
Set-AzureStorageBlobContent -Container $storagecontainername `
    -File $bacpacfilename `
    -Context $(New-AzureStorageContext -StorageAccountName $storageaccountname `
        -StorageAccountKey $(Get-AzureRmStorageAccountKey -ResourceGroupName $resourcegroupname -StorageAccountName $storageaccountname).Value[0])

# Create a new server with a system wide unique server name
$server = New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

# Create a server firewall rule that allows access from the specified IP range
$serverfirewallrule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startip -EndIpAddress $endip

# Import bacpac to database with an S3 performance level
$importRequest = New-AzureRmSqlDatabaseImport -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -DatabaseMaxSizeBytes "262144000" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName $resourcegroupname -StorageAccountName $storageaccountname).Value[0] `
    -StorageUri "http://$storageaccountname.blob.core.chinacloudapi.cn/$storagecontainername/$bacpacfilename" `
    -Edition "Standard" `
    -ServiceObjectiveName "S3" `
    -AdministratorLogin "$adminlogin" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String $password -AsPlainText -Force)

# Check import status and wait for the import to complete
$importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$importStatus

# Scale down to S0 after import is complete
Set-AzureRmSqlDatabase -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename  `
    -Edition "Standard" `
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
| [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlserver) | 创建托管单一数据库和弹性池的 SQL 数据库服务器。 |
| [New-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | 创建一个 SQL 数据库服务器防火墙规则，允许从输入的 IP 地址范围访问 SQL 数据库服务器上的所有单一数据库和入池数据库。 |
| [New-AzureRmSqlDatabaseImport](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) | 导入 BACPAC 文件，并在服务器上创建一个新数据库。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。
<!--Update_Description: wording update-->