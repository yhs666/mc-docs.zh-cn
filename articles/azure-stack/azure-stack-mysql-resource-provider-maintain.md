---
title: 在 Azure Stack 上维护 MySQL 资源提供程序 | Microsoft Docs
description: 了解如何在 Azure Stack 上维护 MySQL 资源提供程序服务。
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/14/2018
ms.date: 06/26/2018
ms.author: v-junlch
ms.reviewer: jeffgo
ms.openlocfilehash: 19d43a268b7c908ceee6967572cff008d763df50
ms.sourcegitcommit: 8a17603589d38b4ae6254bb9fc125d668442ea1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2018
ms.locfileid: "37027292"
---
# <a name="maintenance-operations"></a>维护操作 
MySQL 资源提供程序是锁定的虚拟机。 可以通过 PowerShell Just Enough Administration (JEA) 终结点 _DBAdapterMaintenance_ 更新资源提供程序虚拟机的安全性。 RP 的安装包随附了一个方便执行这些操作的脚本。

## <a name="update-the-virtual-machine-operating-system"></a>更新虚拟机操作系统 
可以通过多种方式更新 Windows Server VM： 
- 使用当前进行了修补的 Windows Server 2016 Core 映像安装最新的资源提供程序包 
- 在安装或更新 RP 期间安装 Windows 更新包 

## <a name="update-the-virtual-machine-windows-defender-definitions"></a>更新虚拟机 Windows Defender 定义 
请按以下步骤更新 Defender 定义： 
1. 从 [Windows Defender 定义](https://www.microsoft.com/en-us/wdsi/definitions)下载 Windows Defender 定义更新

    在该页的“Manually download and install the definitions”（手动下载和安装定义）下，下载“适用于 Windows 10 和 Windows 8.1 的 Windows Defender 防病毒”64 位文件。
    
    直接链接：https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64。 

2. 创建连接到 MySQL RP 适配器虚拟机的维护终结点的 PowerShell 会话。 

3. 使用维护终结点会话将定义更新文件复制到 DB 适配器虚拟机。 

4. 在维护 PowerShell 会话中，调用 _Update-DBAdapterWindowsDefenderDefinitions_ 命令。 

5. 安装以后，建议删除使用过的定义更新文件。 可以在维护会话中使用 _Remove-ItemOnUserDrive)_ 命令将其删除。 

下面是一个用于更新 Defender 定义的示例脚本（请将虚拟机的地址或名称替换为实际值）： 

```powershell 
# Set credentials for the RP VM local admin user 
$vmLocalAdminPass = ConvertTo-SecureString "<local admin user password>" -AsPlainText -Force 
$vmLocalAdminUser = "<local admin user name>" 
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ` 
    ($vmLocalAdminUser, $vmLocalAdminPass) 

# Public IP Address of the DB adapter machine 
$databaseRPMachine  = "<RP VM IP address>" 
$localPathToDefenderUpdate = "C:\DefenderUpdates\mpam-fe.exe" 
 
# Download Windows Defender update definitions file from https://www.microsoft.com/en-us/wdsi/definitions.  
Invoke-WebRequest -Uri 'https://go.microsoft.com/fwlink/?LinkID=121721&arch=x64' ` 
    -Outfile $localPathToDefenderUpdate  

# Create session to the maintenance endpoint 
$session = New-PSSession -ComputerName $databaseRPMachine ` 
    -Credential $vmLocalAdminCreds -ConfigurationName DBAdapterMaintenance 

# Copy defender update file to the db adapter machine 
Copy-Item -ToSession $session -Path $localPathToDefenderUpdate ` 
     -Destination "User:\" 

# Install the update file 
Invoke-Command -Session $session -ScriptBlock ` 
    {Update-AzSDBAdapterWindowsDefenderDefinition -DefinitionsUpdatePackageFile "User:\"} 

# Cleanup the definitions package file and session 
Invoke-Command -Session $session -ScriptBlock ` 
    {Remove-AzSItemOnUserDrive -ItemPath "User:\"} 
$session | Remove-PSSession  
``` 
## <a name="secrets-rotation"></a>机密轮换  
这些说明仅适用于 Azure Stack 集成系统 1804 和更高版本。请勿在低于 1804 的 Azure Stack 版本上尝试使用机密轮换。 
 
将 SQL 和 MySQL 资源提供程序与 Azure Stack 集成系统配合使用时，可以轮换以下基础结构（部署）机密： 
- [部署期间提供的](azure-stack-pki-certs.md)外部 SSL 证书。 
- 部署期间提供的资源提供程序 VM 本地管理员帐户密码。 
- 资源提供程序诊断用户 (dbadapterdiag) 密码。 

#### <a name="powershell-examples-for-rotating-secrets"></a>用于轮换机密的 PowerShell 示例 
 
**同时更改所有机密** 
```powershell 
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DiagnosticsUserPassword $passwd `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd `  
    -VMLocalCredential $localCreds 
``` 
**仅更改诊断用户密码** 
```powershell 
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DiagnosticsUserPassword  $passwd  
``` 

**更改 VM 本地管理员帐户密码** 
```powershell 
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -VMLocalCredential $localCreds 
``` 
**更改 SSL 证书** 
```powershell 
.\SecretRotationMySQLProvider.ps1 `
    -Privilegedendpoint $Privilegedendpoint `
    -CloudAdminCredential $cloudCreds `
    -AzCredential $adminCreds `
    -DependencyFilesLocalPath $certPath `
    -DefaultSSLCertificatePassword $certPasswd  
``` 

### <a name="secretrotationmysqlproviderps1-parameters"></a>SecretRotationMySQLProvider.ps1 参数 
|参数|说明| 
|-----|-----| 
|AzCredential|Azure Stack 服务管理员帐户凭据。| 
|CloudAdminCredential|Azure Stack 云管理域帐户凭据。| 
|PrivilegedEndpoint|用于访问 Get-AzureStackStampInformation 的特权终结点。| 
|DiagnosticsUserPassword|诊断用户密码。| 
|VMLocalCredential|MySQLAdapter VM 的本地管理员帐户。| 
|DefaultSSLCertificatePassword|默认 SSL 证书 (*pfx) 密码。| 
|DependencyFilesLocalPath|依赖项文件本地路径。| 
|     |     | 

### <a name="known-issues"></a>已知问题 
问题：如果脚本在运行时失败，则不会自动收集机密轮换的日志。 
 
解决方法：使用 Get-AzsDBAdapterLogs cmdlet 收集所有资源提供程序日志，包括 C:\Logs 下面的 AzureStack.DatabaseAdapter.SecretRotation.ps1_*.log。 

## <a name="collect-diagnostic-logs"></a>收集诊断日志 
MySQL 资源提供程序是锁定的虚拟机。 如果必须从虚拟机收集日志，则会相应地提供 PowerShell Just Enough Administration (JEA) 终结点 _DBAdapterDiagnostics_。 可以通过此终结点使用两个命令： 

- **Get-AzsDBAdapterLog**。 准备包含 RP 诊断日志的 zip 包并将其置于会话用户驱动器上。 此命令可以在不使用参数的情况下调用，将会收集过去四小时的日志。 

- **Remove-AzsDBAdapterLog**。 清理资源提供程序 VM 上现有的日志包 

在 RP 部署或更新期间会创建名为 _dbadapterdiag_ 的用户帐户，用于连接到诊断终结点以提取 RP 日志。 此帐户的密码就是在部署/更新期间为本地管理员帐户提供的密码。 

若要使用这些命令，需创建一个连接到资源提供程序虚拟机的远程 PowerShell 会话，然后调用命令。 可以选择提供 FromDate 和 ToDate 参数。 如果不指定这其中的一个参数，或者两个参数都不指定，则 FromDate 为当前时间之前的四小时，ToDate 为当前时间。 

以下示例脚本演示如何使用这些命令： 

```powershell 
# Create a new diagnostics endpoint session. 
$databaseRPMachineIP = '<RP VM IP address>' 
$diagnosticsUserName = 'dbadapterdiag' 
$diagnosticsUserPassword = '<Enter Diagnostic password>' 
$diagCreds = New-Object System.Management.Automation.PSCredential ` 
        ($diagnosticsUserName, (ConvertTo-SecureString -String $diagnosticsUserPassword -AsPlainText -Force)) 
$session = New-PSSession -ComputerName $databaseRPMachineIP -Credential $diagCreds 
        -ConfigurationName DBAdapterDiagnostics 

# Sample captures logs from the previous one hour 
$fromDate = (Get-Date).AddHours(-1) 
$dateNow = Get-Date 
$sb = {param($d1,$d2) Get-AzSDBAdapterLog -FromDate $d1 -ToDate $d2} 
$logs = Invoke-Command -Session $session -ScriptBlock $sb -ArgumentList $fromDate,$dateNow 

# Copy the logs 
$sourcePath = "User:\{0}" -f $logs 
$destinationPackage = Join-Path -Path (Convert-Path '.') -ChildPath $logs 
Copy-Item -FromSession $session -Path $sourcePath -Destination $destinationPackage 

# Cleanup logs 
$cleanup = Invoke-Command -Session $session -ScriptBlock {Remove- AzsDBAdapterLog } 
# Close the session 
$session | Remove-PSSession 
``` 


## <a name="next-steps"></a>后续步骤
[删除 MySQL 资源提供程序](azure-stack-mysql-resource-provider-remove.md)

