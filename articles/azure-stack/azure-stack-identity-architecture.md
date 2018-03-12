---
title: "Azure Stack 的标识体系结构 | Microsoft Docs"
description: "了解可以与 Azure Stack 一起使用的标识体系结构。"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 02/22/2018
ms.date: 03/02/2018
ms.author: v-junlch
ms.reviewer: 
ms.openlocfilehash: 74b6f96537b0061f84007ff539872f7b13332d37
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="identity-architecture-for-azure-stack"></a>Azure Stack 的标识体系结构
在选择要与 Azure Stack 一起使用的标识提供程序之前，请了解 Azure Active Directory (Azure AD) 的选项与 Active Directory 联合身份验证服务 (AD FS) 的选项之间的重要区别。 

## <a name="capabilities-and-limitations"></a>功能和限制 
你选择的标识提供程序可能会限制你的选项，包括对多租户的支持。 

  

|功能或方案        |Azure AD  |AD FS  |
|------------------------------|----------|-------|
|连接到 Internet     |是       |可选|
|对多租户的支持     |是       |否      |
|Marketplace 联合       |是       |是 - 需要使用[脱机 Marketplace 联合](azure-stack-download-azure-marketplace-item.md#download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity)工具。|
|对 Active Directory 身份验证库 (ADAL) 的支持 |是 |是|
|对 Azure 命令行接口 (CLI)、Visual Studio (VS) 和 PowerShell 等工具的支持  |是 |是|
|通过门户创建服务主体     |是 |否|
|使用证书创建服务主体      |是 |是|
|使用机密（密钥）创建服务主体    |是 |否|
|应用程序可以使用 Graph 服务           |是 |否|
|应用程序可以将标识提供程序用于登录 |是 |是 - 要求应用程序与本地 AD FS 进行联合。 |

## <a name="topologies"></a>拓扑
以下各部分提供了有关你可以使用的标识拓扑的信息。

### <a name="azure-ad---single-tenant"></a>Azure AD - 单租户 
默认情况下，当安装 Azure Stack 并使用 Azure AD 时，Azure Stack 使用单租户拓扑。 

单租户拓扑非常适用于下列情况：
- 所有用户都属于同一租户。
- 服务提供程序托管着组织的 Azure Stack 实例。  

![将单租户拓扑与 Azure AD 配合使用的 Azure Stack 拓扑](./media/azure-stack-identity-architecture/single-tenant.png)

采用此拓扑时：
- Azure Stack 将所有应用程序和服务注册到同一 Azure AD 租户目录。 
- Azure Stack 仅通过该目录对用户和应用程序进行身份验证，包括令牌。 
- 管理员（云操作员）和租户用户的标识位于同一目录租户中。 
- 若要使其他目录中的用户能够访问此 Azure Stack 环境，必须[将用户作为来宾邀请](azure-stack-identity-overview.md#guest-users)到该租户目录。  

### <a name="azure-ad---multi-tenant"></a>Azure AD - 多租户
云操作员可以将 Azure Stack 配置为允许一个或多个组织中的租户访问应用程序。 用户通过用户门户访问应用程序。 在此配置中，管理员门户（由云操作员使用）仅限单个目录中的用户访问。 

多租户拓扑非常适用于下列情况：
- 服务提供商希望允许多个组织中的用户访问 Azure Stack。

![将多租户拓扑与 Azure AD 配合使用的 Azure Stack 拓扑](./media/azure-stack-identity-architecture/multi-tenant.png)

采用此拓扑时：
- 对资源的访问权限应当以组织为单位。 
- 一个组织中的用户不应当能够向其组织外部的用户授予对资源的访问权限。  
- 管理员（云操作员）标识可以位于与用户标识所在目录租户不同的目录租户中。 此分离在标识提供者级别提供了帐户隔离。 
 
### <a name="ad-fs"></a>AD FS  
当以下任一情况属实时，需要使用 AD FS 拓扑：
- Azure Stack 不连接到 Internet。
- Azure Stack 可以连接到 Internet，但你选择为标识提供者使用 AD FS。
  
![使用 AD FS 的 Azure Stack 拓扑](./media/azure-stack-identity-architecture/adfs.png)

采用此拓扑时：
- 为支持在生产中使用，必须通过联合身份验证信任将内置的 Azure Stack AD FS 实例与由 Active Directory (AD) 提供支持的现有 AD FS 实例进行集成。 
- 可以将 Azure Stack 中的 Graph 服务与现有 AD 进行集成。  还可以使用基于 OData 的 Graph API 服务，该服务支持与 Azure AD Graph API 一致的 API。  

  为了与 AD 进行交互，Graph API 需要使用 AD 中对 AD 具有只读权限的用户凭据。 
  - 内置的 AD FS 基于 Server 2016。 
  - AD FS 和 AD 必须基于 Server 2012 或较低版本。 
  
  在 AD 与内置的 AD FS 之间，交互不限于 OpenID Connect 并且可以使用任何相互支持的协议。  
  - 用户帐户是在本地 AD 中创建和管理的。
  - 应用程序的服务主体和注册是在内置 AD 中管理的。



## <a name="next-steps"></a>后续步骤
- [标识概述](azure-stack-identity-overview.md)   
- [数据中心集成 - 标识](azure-stack-integrate-identity.md)

