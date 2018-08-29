---
title: 配置 Azure Stack 用户的 PowerShell 环境 | Azure
description: 配置 Azure Stack 用户的 PowerShell 环境
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: F4ED2238-AAF2-4930-AA7F-7C140311E10F
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/17/2018
ms.date: 08/27/2018
ms.author: v-junlch
ms.reviewer: Balsu.G
ms.openlocfilehash: 11bde558fa9c5f4f9bbf9e5e0d473376920c4c79
ms.sourcegitcommit: 9dda276bc6675d7da3070ea6145079f1538588ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "42869551"
---
# <a name="configure-the-azure-stack-users-powershell-environment"></a>配置 Azure Stack 用户的 PowerShell 环境

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

本文提供了连接到 Azure Stack 实例的步骤。 必须使用 PowerShell 连接以管理 Azure Stack 资源。 例如，可以使用 PowerShell 来订阅套餐、创建虚拟机和部署 Azure 资源管理器模板。 以便执行 PowerShell cmdlet。

若要获取设置：
  - 确保满足要求。
  - 与 Azure Active Directory (Azure AD) 或 Active Directory 联合身份验证服务 (AD FS) 连接。 
  - 注册资源提供程序。
  - 测试连接。

## <a name="prerequisites"></a>先决条件

如果已[通过 VPN 建立连接](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn)，请通过[开发工具包](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop)或基于 Windows 的外部客户端配置以下先决条件：

* 安装 [Azure Stack 兼容的 Azure PowerShell 模块](azure-stack-powershell-install.md)。
* 下载[使用 Azure Stack 所需的工具](azure-stack-powershell-download.md)。

确保将以下脚本变量替换为 Azure Stack 配置中的值：

- **Azure AD 租户名称**  
  用于管理 Azure Stack 的 Azure AD 租户的名称，例如 yourdirectory.partner.onmschina.cn。
- **Azure 资源管理器终结点**  
  对于 Azure Stack 开发工具包，此值设置为 https://management.local.azurestack.external。 若要为 Azure Stack 集成系统获取此值，请与服务提供商联系。

## <a name="connect-with-azure-ad"></a>与 Azure AD 连接

  ```PowerShell
  $AADTenantName = "yourdirectory.partner.onmschina.cn"
  $ArmEndpoint = "https://management.local.azurestack.external"

  # Register an Azure Resource Manager environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

  $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackUser").ActiveDirectoryAuthority.TrimEnd('/')
  $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackUser" `
    -TenantId $TenantId
   ```

## <a name="connect-with-ad-fs"></a>与 AD FS 连接

  ```PowerShell  
  $ArmEndpoint = "https://management.local.azurestack.external"

  # Register an Azure Resource Manager environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

  $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackUser").ActiveDirectoryAuthority.TrimEnd('/')
  $tenantId = (invoke-restmethod "$($AuthEndpoint)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackUser" `
    -TenantId $tenantId
  ```

## <a name="register-resource-providers"></a>注册资源提供程序

不会自动为没有通过门户部署任何资源的新用户订阅自动注册资源提供程序。 可以通过运行以下脚本显式注册资源提供程序：

```PowerShell  
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider -Force
    }
```

## <a name="test-the-connectivity"></a>测试连接

完成所有设置后，请通过使用 PowerShell 在 Azure Stack 中创建资源来测试连接。 作为测试，为应用程序创建资源组并添加虚拟机。 运行以下命令创建名为“MyResourceGroup”的资源组：

```PowerShell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>后续步骤

- [为 Azure Stack 开发模板](azure-stack-develop-templates.md)
- [通过 PowerShell 部署模板](azure-stack-deploy-template-powershell.md)
- 若要为云操作员环境设置 PowerShell，请参阅[配置 Azure Stack 操作员的 PowerShell 环境](../azure-stack-powershell-configure-admin.md)一文。