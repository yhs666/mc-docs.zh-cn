---
title: Azure Stack 中的多租户
description: 了解如何在 Azure Stack 中支持多个 Azure Active Directory 目录
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 11/06/2018
ms.date: 12/17/2018
ms.author: v-jay
ms.reviewer: bryanr
ms.openlocfilehash: 15c9947da56ee7aa813036735d6b7af5a976247d
ms.sourcegitcommit: 98142af6eb83f036d72e26ebcea00e2fceb673af
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2018
ms.locfileid: "53396077"
---
# <a name="multi-tenancy-in-azure-stack"></a>Azure Stack 中的多租户

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

可以配置 Azure Stack，以支持多个 Azure Active Directory (Azure AD) 租户中的用户使用 Azure Stack 中的服务。 例如，考虑以下方案：

 - 你是安装了 Azure Stack 的 contoso.partner.onmschina.cn 的服务管理员。
 - Mary 是来宾用户所在的 fabrikam.partner.onmschina.cn 的目录管理员。 
 - Mary 的公司收到你的公司提供的 IaaS 和 PaaS 服务，并需要允许来宾目录 (fabrikam.partner.onmschina.cn) 中的用户登录 contoso.partner.onmschina.cn 并使用其中的 Azure Stack 资源。

本指南提供了此方案上下文中所需的步骤，用于在 Azure Stack 中配置多租户。 在此方案中，你和 Mary 必须完成相关步骤以使 Fabrikam 中的用户能够登录并使用 Contoso 中部署的 Azure Stack 提供的服务。  

## <a name="enable-multi-tenancy"></a>启用多租户

在 Azure Stack 中配置多租户之前，需要考虑几个先决条件：
  
 - 你和 Mary 必须在安装 Azure Stack 的目录 (Contoso) 和来宾目录 (Fabrikam) 之间协调管理步骤。  
 - 确保已[安装](azure-stack-powershell-install.md)并[配置](azure-stack-powershell-configure-admin.md)适用于 Azure Stack 的 PowerShell。
 - [下载 Azure Stack Tools](azure-stack-powershell-download.md)，并导入“连接和标识”模块：

    ````PowerShell  
    Import-Module .\Connect\AzureStack.Connect.psm1
    Import-Module .\Identity\AzureStack.Identity.psm1
    ````

### <a name="configure-azure-stack-directory"></a>配置 Azure Stack 目录

在本部分中，将配置 Azure Stack 以允许从 Fabrikam Azure AD 目录租户登录。

通过将 Azure 资源管理器配置为接受来自来宾目录租户的用户和服务主体，将来宾目录租户 (Fabrikam) 加入到 Azure Stack。

contoso.partner.onmschina.cn 的服务管理员运行以下命令。

````PowerShell  
## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
$adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

## Replace the value below with the Azure Stack directory
$azureStackDirectoryTenant = "contoso.partner.onmschina.cn"

## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantToBeOnboarded = "fabrikam.partner.onmschina.cn"

## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
$ResourceGroupName = "system.local"

## Replace the value below with the region location of the resource group. 
$location = "local"

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location $location `
 -ResourceGroupName $ResourceGroupName
````

### <a name="configure-guest-directory"></a>配置来宾目录

在 Azure Stack 管理员 / 操作员使得 Fabrikam 目录能够与 Azure Stack 一起使用后，Mary 必须向 Fabrikam 的目录租户注册 Azure Stack。

#### <a name="registering-azure-stack-with-the-guest-directory"></a>将 Azure Stack 注册到来宾目录

Fabrikam 的目录管理员 Mary 在来宾目录 fabrikam.partner.onmschina.cn 中运行以下命令。

````PowerShell
## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
$tenantARMEndpoint = "https://management.local.azurestack.external"
    
## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantName = "fabrikam.partner.onmschina.cn"

Register-AzSWithMyDirectoryTenant `
 -TenantResourceManagerEndpoint $tenantARMEndpoint `
 -DirectoryTenantName $guestDirectoryTenantName `
 -Verbose 
````

> [!IMPORTANT]
> 如果你的 Azure Stack 管理员将来安装新服务或更新，则你可能需要再次运行此脚本。
>
> 随时可以再次运行此脚本来检查目录中的 Azure Stack 应用程序的状态。
>
> 如果已注意到在托管磁盘中创建 VM 时存在的问题（在 1808 更新中引入），则已添加新的**磁盘资源提供程序**，从而需要再次运行此脚本。

### <a name="direct-users-to-sign-in"></a>指导用户登录

现在，你和 Mary 已完成到加入 Mary 目录的步骤，Mary 可以指导 Fabrikam 用户登录。  Fabrikam 用户（即，具有 fabrikam.partner.onmschina.cn 后缀的用户）通过访问 https://portal.local.azurestack.external 登录。  

Mary 将指导 Fabrikam 目录中的任何[外部主体](../role-based-access-control/rbac-and-directory-admin-roles.md)（即，Fabrikam 目录中没有 fabrikam.partner.onmschina.cn 后缀的用户）使用 https://portal.local.azurestack.external/fabrikam.partner.onmschina.cn 登录。  如果他们未使用此 URL，则将被发送到其默认目录 (Fabrikam)，并收到一个错误，指出其管理员未许可。

## <a name="disable-multi-tenancy"></a>禁用多租户

如果 Azure Stack 中不再需要有多个租户，可以按顺序执行以下步骤来禁用多租户：

1. 以来宾目录的管理员身份（在此场景中为 Mary）运行 *Unregister-AzsWithMyDirectoryTenant*。 该 cmdlet 从新目录中卸载所有 Azure Stack 应用程序。

    ``` PowerShell
    ## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
    $tenantARMEndpoint = "https://management.local.azurestack.external"
        
    ## Replace the value below with the guest tenant directory. 
    $guestDirectoryTenantName = "fabrikam.partner.onmschina.cn"
    
    Unregister-AzsWithMyDirectoryTenant `
     -TenantResourceManagerEndpoint $tenantARMEndpoint `
     -DirectoryTenantName $guestDirectoryTenantName `
     -Verbose 
    ```

2. 以 Azure Stack 的服务管理员身份（在此场景中是你）运行 *Unregister-AzSGuestDirectoryTenant*。 

    ``` PowerShell  
    ## The following Azure Resource Manaager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
    $adminARMEndpoint = "https://adminmanagement.local.azurestack.external"
    
    ## Replace the value below with the Azure Stack directory
    $azureStackDirectoryTenant = "contoso.partner.onmschina.cn"
    
    ## Replace the value below with the guest tenant directory. 
    $guestDirectoryTenantToBeDecommissioned = "fabrikam.partner.onmschina.cn"
    
    ## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
    $ResourceGroupName = "system.local"
    
    Unregister-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
     -DirectoryTenantName $azureStackDirectoryTenant `
     -GuestDirectoryTenantName $guestDirectoryTenantToBeDecommissioned `
     -ResourceGroupName $ResourceGroupName
    ```

    > [!WARNING]
    > 禁用多租户步骤必须按顺序执行。 如果首先完成步骤 1，则步骤 2 将失败。

## <a name="next-steps"></a>后续步骤

- [管理委派提供程序](azure-stack-delegated-provider.md)
- [Azure Stack 的重要概念](azure-stack-key-features.md)
<!-- Update_Description: Disable multi-tenancy -->