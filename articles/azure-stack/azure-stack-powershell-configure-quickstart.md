---
title: "有关安装和配置适用于 Azure Stack 的 PowerShell 的快速入门 | Microsoft Docs"
description: "了解如何安装和配置适用于 Azure Stack 的 PowerShell。"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 6996DFC1-5E05-423A-968F-A9427C24317C
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/11/2018
ms.date: 03/02/2018
ms.author: v-junlch
ms.openlocfilehash: 9ecdbd71395d2edd8ed127e5fd69f6817ce8d00f
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="get-up-and-running-with-powershell-in-azure-stack"></a>在 Azure Stack 中使用 PowerShell 启动并运行

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

本快速入门帮助你使用 PowerShell 来安装和配置 Azure Stack 环境。 本文中提供的脚本仅面向 **Azure Stack 操作员**。

本文是[安装 PowerShell]( azure-stack-powershell-install.md)、[下载工具]( azure-stack-powershell-download.md)和[配置 Azure Stack 操作员的 PowerShell 环境]( azure-stack-powershell-configure-admin.md)文章中所述步骤的精简版本。 使用本主题中的脚本，可以设置连同 Azure Active Directory 或 Active Directory 联合身份验证服务 (AD FS) 一起部署的 Azure Stack PowerShell 环境。  


## <a name="set-up-powershell-for-azure-active-directory-based-deployments"></a>设置 PowerShell 进行基于 Azure Active Directory 的部署

登录到 Azure Stack 开发工具包；如果已通过 VPN 建立连接，请登录到基于 Windows 的外部客户端。 打开权限提升的 PowerShell ISE 会话，然后运行以下脚本。 确保根据需要更新环境配置的 **TenantName**、**ArmEndpoint** 和 **GraphAudience** 变量：

> [!IMPORTANT]
> AzureRM 1.2.11 PowerShell 模块版本随附了重大更改列表。 

```powershell
# Specify Azure Active Directory tenant name.
$TenantName = "<mydirectory>.partner.onmschina.cn"

# Set the module repository and the execution policy.
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions, and then run the following command:
Get-Module -ListAvailable -Name Azure* | `
  Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module.
cd \

invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = "<Keyvault DNS suffix for your environment>"


# Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName $TenantName `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
```

## <a name="set-up-powershell-for-ad-fs-based-deployments"></a>设置 PowerShell 进行基于 AD FS 的部署

如果在连接到 Internet 的情况下运行 Azure Stack，则可以使用以下脚本。 但是，如果在未建立 Internet 连接的情况下运行 Azure Stack，请使用[脱机安装 PowerShell](azure-stack-powershell-install.md#install-powershell-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) 的方法，用于配置 PowerShell 的 cmdlet 与此脚本中所示相同。 登录到 Azure Stack 开发工具包；如果已通过 VPN 建立连接，请登录到基于 Windows 的外部客户端。 打开权限提升的 PowerShell ISE 会话，然后运行以下脚本。 确保根据需要更新环境配置的 **ArmEndpoint** 和 **GraphAudience** 变量：

```powershell

# Set the module repository and the execution policy.
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions and run the following command:
Get-Module -ListAvailable -Name Azure* | `
  Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module.
cd \
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
$ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = "<Keyvault DNS suffix for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack     
$TenantID = Get-AzsDirectoryTenantId `
    -ADFS `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID
```

## <a name="test-the-connectivity"></a>测试连接

配置 PowerShell 后，可以通过创建资源组来测试配置：

```powershell
New-AzureRMResourceGroup -Name "ContosoVMRG" -Location Local
```

创建资源组后，“预配状态”属性会设置为“成功”。

## <a name="next-steps"></a>后续步骤

- [安装和配置 CLI](azure-stack-connect-cli.md)

- [开发模板](user/azure-stack-develop-templates.md)








