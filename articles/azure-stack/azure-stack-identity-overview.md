---
title: "Azure Stack 的标识概述 | Microsoft Docs"
description: "了解可与 Azure Stack 配合使用的标识系统。"
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
ms.openlocfilehash: b78141b7e38216a75e07a664ff5dfbb691870693
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="overview-of-identity-for-azure-stack"></a>Azure Stack 的标识概述

Azure Stack 要求以 Active Directory (AD) 支持的 Azure AD 或 Active Directory Federated Services (AD FS) 作为标识提供者。 以下概念和授权详细信息可帮助你选择适当的标识提供者。 提供者的选择是首次部署 Azure Stack 时做出的一次性决定。  

选择 Azure AD 或 AD FS 可能受限于 Azure Stack 的部署模式： 
- 在连接模式下部署时，可以使用 Azure AD 或 AD FS。 
- 在未建立 Internet 连接的断开连接模式下部署时，仅支持 AD FS。

有关这些选择的详细信息，请根据 Azure Stack 环境查看以下文章：
- 对于 Azure Stack 部署工具包，请参阅[标识注意事项](azure-stack-datacenter-integration.md#identity-considerations)。
- 对于 Azure Stack 集成系统，请参阅 [Azure Stack 集成系统的部署规划决策](azure-stack-deployment-decisions.md)。

 
## <a name="common-concepts-for-identity"></a>标识的一般概念
以下部分介绍标识提供者的一般概念及其在 Azure Stack 中的用法。
![标识提供者的术语](./media/azure-stack-identity-overview/terminology.png)


### <a name="directories-tenants-and-organizations"></a>目录租户和组织
目录是保留用户、应用程序、组和服务主体相关信息的容器。
 
目录租户是一个组织，例如 Microsoft 或你自己的公司。 
- Azure AD 支持多个租户并可支持多个组织（各自位于自身的目录中）。 如果使用 Azure AD 并且有多个租户，则可以授权应用程序和用户从一个租户访问同一目录中的其他租户。
- AD FS 仅支持单个租户，因此支持单个组织。 

### <a name="users-and-groups"></a>用户和组
用户帐户（标识）是标准帐户，可使用用户 ID 和密码对个人进行身份验证。 组可以包含用户或其他组。 

创建和管理用户与组的方式取决于所用的标识解决方案。 

在 Azure Stack 中，用户帐户： 
- 以 *username@domain* 格式创建。 尽管 AD FS 可将用户帐户映射到 Active Directory，但 AD FS 不支持使用 _&lt;域>\<别名>_ 格式。 
- 可以设置为使用多重身份验证。 
- 限制为它们首先注册到的目录，即其组织目录。
- 可从本地目录导入。 有关详细信息，请参阅 Azure 文档中的[将本地目录与 Azure Active Directory 集成](/active-directory/connect/active-directory-aadconnect)。  

登录到组织的租户门户时，请使用 https://portal.local.azurestack.external。 

### <a name="guest-users"></a>来宾用户
来宾用户是其他目录租户中的用户帐户，这些用户已获得目录中资源的访问权限。 若要支持来宾用户，必须使用 Azure AD 并启用多租户支持。 若已提供支持，可以邀请来宾用户访问目录租户中的资源，以便与外部组织协作。 

来宾用户可以登录其他组织的目录租户。 为此，必须将组织的目录名称追加到门户 URL。  例如，如果你属于 contoso.com，但想要登录 Fabrikam 目录，可以使用 https://portal.local.azurestack.external/fabrikam.partner.onmschina.cn。

### <a name="applications"></a>应用程序
可以向 Azure AD 或 AD FS 注册应用程序，然后将其提供给组织中的用户。 

应用程序包括：
- **Web 应用程序** – 示例包括 Azure 门户和 Azure 资源管理器。  它们支持 Web API 调用。 
- **本机客户端** – 示例包括 Azure PowerShell、Visual Studio 和 Azure 命令行接口 (CLI)。

应用程序可支持两种租户： 
- **单租户**应用程序仅支持应用程序注册所在的同一目录中的用户和服务。 

  > [!NOTE]    
  > 由于 AD FS 仅支持单个目录，因此在 AD FS 拓扑中创建的应用设计为单租户应用程序。
- **多租户**应用程序支持供以下目录中的用户和服务使用：应用程序注册所在的目录，以及其他租户目录。  另一个租户目录（另一个 Azure AD 租户）的用户可以使用多租户应用程序登录到应用程序。     

  有关多租户的详细信息，请参阅[启用多租户](azure-stack-enable-multitenancy.md)。  

  有关如何开发多租户应用的详细信息，请参阅[多租户应用](/active-directory/develop/active-directory-devhowto-multi-tenant-overview)。

注册应用程序时，需创建两个对象：
- **应用程序对象** – 应用程序对象是应用程序在所有租户中的全局表示形式。 此关系是与软件应用程序的一对一关系，且只存在于应用程序首先注册到的目录中。

 
   
- **服务主体对象** – 服务主体是在首先注册应用程序的目录中针对应用程序所创建的凭据。 服务主体也是在使用该应用程序的其他每个租户的目录中创建的。 此关系可以是与软件应用程序的一对多关系。   

有关应用程序和服务主体的详细信息，请参阅 [Azure Active Directory (Azure AD) 中的应用程序和服务主体对象](/active-directory/develop/active-directory-application-objects)。 

### <a name="service-principals"></a>服务主体 
服务主体是应用程序或服务的一组凭据，可授予 Azure Stack 中资源的访问权限。 使用服务主体可将应用程序权限与使用应用程序的用户权限区分开来。

服务主体是在使用应用程序的每个租户中创建的。 服务主体建立标识用于登录和访问受该租户保护的资源（类似于用户）。   

- 单租户应用程序在其首次创建所在的目录中只有一个服务主体。  创建的此服务主体在应用程序注册期间授予使用权限。 
- 对于多租户 Web 应用程序或 API，将在用户许可使用应用程序的每个租户中创建一个服务主体。  

服务主体的凭据可以是密钥（通过 Azure 门户生成）或证书。  证书的使用很适合自动化，因为证书被视为比密钥更安全。 


> [!NOTE]    
> 在 Azure Stack 中使用 AD FS 时，只有管理员可以创建服务主体。 使用 AD FS 时，服务主体需要证书，并需要通过特权终结点 (PEP) 来创建。 有关详细信息，请参阅[提供对 Azure Stack 的应用程序访问权限](azure-stack-create-service-principals.md)。

若要了解 Azure Stack 的服务主体，请参阅[创建服务主体](azure-stack-create-service-principals.md)。


### <a name="services"></a>服务
在 Azure Stack 中与标识提供者交互的服务，标识提供者将其注册为应用程序。 如同应用程序，注册可让服务在标识系统中进行身份验证。 

所有 Azure 服务都使用 [OpenID Connect](/active-directory/develop/active-directory-protocols-openid-connect-code) 协议和 [JSON Web 令牌](/active-directory/develop/active-directory-token-and-claims) (JWT) 来建立其标识。 由于 Azure AD 和 AD FS 使用的协议一致，因此可以使用 [Azure Active Directory 身份验证库](/active-directory/develop/active-directory-authentication-libraries) (ADAL) 在本地环境或 Azure 中进行身份验证（在已连接方案中）。 通过 ADAL 还能使用 Azure PowerShell 和 CLI 等工具进行跨云和本地资源管理。 


### <a name="identities-and-your-identity-system"></a>标识和标识系统 
Azure Stack 的标识包括用户帐户、组和服务主体。 当安装 Azure Stack 时，有多个内置应用程序和服务在目录租户中自动向标识提供者注册。 有些注册的服务用于管理。 还有些服务供用户使用。 默认注册提供核心服务标识，此类标识可彼此交互，以及与今后添加的标识交互。
如果设置采用多租户的 Azure AD，则有些应用程序会传播到新的目录。  

## <a name="authentication-and-authorization"></a>身份验证和授权
 

### <a name="authentication-by-applications-and-users"></a>应用程序和用户的身份验证
  
![Azure Stack 层之间的标识](./media/azure-stack-identity-overview/identity-layers.png)

对应用程序和用户而言，Azure Stack 的体系结构可以用四个层来描述。 各层之间的交互可以使用不同类型的身份验证。


|层    |各层之间的身份验证  |
|---------|---------|
|工具与客户端，例如管理门户     | 为了访问或修改 Azure Stack 中的资源，工具和客户端将使用 [JSON Web 令牌](/active-directory/develop/active-directory-token-and-claims) (JWT) 来调用 Azure 资源管理器。  <br><br> Azure 资源管理器验证 JWT 并扫视所颁发令牌中的声明，以评估用户或服务主体在 Azure Stack 中的授权级别。        |
|Azure 资源管理器及其核心服务     |Azure 资源管理器与资源提供程序通信，以传输用户的通信。 <br><br> 传输通过 [Azure 资源管理器模板](/azure-stack/user/azure-stack-arm-templates.md)使用直接命令式调用或声明式调用。|
|资源提供程序     |传递给资源提供程序的调用以基于证书的身份验证进行保护。 <br><br>随后，Azure 资源管理器和资源提供程序持续通过 API 通信。 对于从 Azure 资源管理器 收到的每个调用，资源提供程序使用该证书来验证调用。|
|基础结构和业务逻辑     |资源提供程序使用所选的身份验证模式来与业务逻辑和基础结构通信。 Azure Stack 随附的默认资源提供程序使用 Windows 身份验证来保护此通信。|

![身份验证所需的信息](./media/azure-stack-identity-overview/authentication.png)


### <a name="authenticate-to-azure-resource-manager"></a>对 Azure 资源管理器进行身份验证
若要在标识提供者中进行身份验证并接收 JWT，必须提供以下信息： 
1. **标识系统（颁发机构）的 URL** – 可用于访问标识提供者的 URL。 例如 *&lt;https://login.chinacloudapi.cn>*。 
2. **Azure 资源管理器的应用 ID URI** – 已向标识提供者注册的 Azure 资源管理器的唯一标识符，并且是每个 Azure Stack 安装中的唯一标识符。
3. **凭据** – 用于在标识提供者中进行身份验证的凭据。  
4. **Azure 资源管理器 的 URL** – 该 URL 是 Azure 资源管理器服务的位置。 例如 *&lt;https://management.chinacloudapi.cn>* 或 *&lt;https://management.local.azurestack.external>*。

当主体（客户端、应用程序或用户）发出身份验证请求以访问资源时，该请求必须包含：
- 主体的凭据。
- 它们想要访问的资源的应用 ID URI。  

凭据已由标识提供者验证。 标识提供者还会验证应用 ID URI 是否用于已注册的应用程序，以及主体是否拥有适当的权限，可获取该资源的令牌。 如果请求有效，则授予 JSON Web 令牌。 

然后，必须将此令牌传入发往 Azure 资源管理器的请求的标头中。 Azure 资源管理器执行以下验证（不遵循特定的顺序）：
- 验证 *issuer* (iss) 声明，确认令牌来自正确的标识提供者。 
- 身份验证 *audience* (aud) 声明，确认令牌已颁发给 Azure 资源管理器。 
- 验证 JWT 是否已使用通过 OpenID 配置的证书进行签名，并且为 Azure 资源管理器所知。 
- 检查 *issued at* (iat) 和 *expiry* (exp) 声明，确认令牌处于活动状态并可接受。 

完成所有验证后，Azure 资源管理器使用 *objected* (oid) 和 *groups* 声明来生成主体可以访问的资源列表。 

![令牌交换协议示意图](./media/azure-stack-identity-overview/token-exchange.png)


### <a name="use-role-based-access-control"></a>使用基于角色的访问控制  
Azure Stack 中基于角色的访问控制 (RBAC) 与 Azure 中的实现一致。  可以通过将相应的 RBAC 角色分配给用户、组和应用程序，来管理资源访问权限。 有关如何在 Azure Stack 中使用 RBAC 的信息，请查看以下文章：
- [Azure 门户中基于角色的访问控制入门](/active-directory/role-based-access-control-what-is)。
- [使用基于角色的访问控制管理 Azure 订阅资源的访问权限](/active-directory/role-based-access-control-configure)
- [创建用于 Azure 基于角色的访问控制的自定义角色](/active-directory/role-based-access-control-custom-roles)
- 在 Azure Stack 中[管理基于角色的访问控制](azure-stack-manage-permissions.md)。


### <a name="authenticate-with-azure-powershell"></a>使用 Azure PowerShell 进行身份验证  
有关使用 Azure PowerShell 在 Azure Stack 中进行身份验证的详细信息，请参阅[配置 Azure Stack 用户的 PowerShell 环境](azure-stack-powershell-configure-user.md)。

### <a name="authenticate-with-azure-cli"></a>使用 Azure CLI 进行身份验证
有关使用 Azure PowerShell 在 Azure Stack 中进行身份验证的详细信息，请参阅[安装和配置与 Azure Stack 配合使用的 CLI](/azure-stack/user/azure-stack-connect-cli.md)。

## <a name="next-steps"></a>后续步骤
- [标识体系结构](azure-stack-identity-architecture.md)   
- [数据中心集成 - 标识](azure-stack-integrate-identity.md)





