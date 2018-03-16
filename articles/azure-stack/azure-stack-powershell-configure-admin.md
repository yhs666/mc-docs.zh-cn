---
title: "配置 Azure Stack 操作员的 PowerShell 环境 | Microsoft Docs"
description: "了解如何配置 Azure Stack 操作员的 PowerShell 环境。"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 37D9CAC9-538B-4504-B51B-7336158D8A6B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/23/2017
ms.date: 03/02/2018
ms.author: v-junlch
ms.openlocfilehash: ca1ddd18aaeb9dee462fca93a8fd4ac72f7fc1c6
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="configure-the-azure-stack-operators-powershell-environment"></a>配置 Azure Stack 操作员的 PowerShell 环境

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

作为 Azure Stack 操作员，可以配置 Azure Stack 开发工具包的 PowerShell 环境。 配置之后，可以使用 PowerShell 管理 Azure Stack 资源，如创建产品/服务、计划、配额以及管理警报，等等。本主题的范围仅限于用于云操作员环境，如果想要为用户环境设置 PowerShell，请参阅[配置 Azure Stack 用户的 PowerShell 环境](user/azure-stack-powershell-configure-user.md)主题。 

## <a name="prerequisites"></a>先决条件

如果已[通过 VPN 建立连接](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn)，请通过[开发工具包](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop)或基于 Windows 的外部客户端运行以下先决条件操作： 

- 安装 [Azure Stack 兼容的 Azure PowerShell 模块](azure-stack-powershell-install.md)。  
- 下载[使用 Azure Stack 所需的工具](azure-stack-powershell-download.md)。  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>配置操作员环境并登录到 Azure Stack

根据部署类型（Azure AD 或 AD FS），使用 PowerShell 运行以下脚本之一来配置 Azure Stack 操作员环境（请确保根据自己的环境配置替换 AAD tenantName、GraphAudience 终结点和 ArmEndpoint 的值）：

### <a name="azure-active-directory-aad-based-deployments"></a>基于 Azure Active Directory (AAD) 的部署
       
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = “<Keyvault DNS suffix for your environment>”


  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint
    
  Set-AzureRmEnvironment `
    -Name "AzureStackAdmin" `
    -GraphAudience $GraphAudience 

  # Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName "<myDirectoryTenantName>.partner.onmschina.cn" `
    -EnvironmentName "AzureStackAdmin"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
  ```

### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>基于 Active Directory 联合身份验证服务 (AD FS) 的部署
         
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = “<Keyvault DNS suffix for your environment>”


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

完成所有设置后，让我们使用 PowerShell 在 Azure Stack 中创建资源。 例如，可以为应用程序创建资源组并添加虚拟机。 使用以下命令创建名为“MyResourceGroup”的资源组：

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>后续步骤
- [为 Azure Stack 开发模板](user/azure-stack-develop-templates.md)
- [通过 PowerShell 部署模板](user/azure-stack-deploy-template-powershell.md)

