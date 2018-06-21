---
title: Azure Active Directory 入门 | Azure
description: 获取许可证，在 Azure Active Diretory 中添加域名，创建自定义登录页，并添加自助密码重置
keywords: ''
author: yunan2016
manager: digimobile
ms.author: v-nany
ms.reviewer: jsnow
origin.date: 11/14/2017
ms.date: 1/29/2018
ms.topic: article
ms.prod: ''
ms.service: active-directory
ms.workload: identity
ms.technology: ''
ms.assetid: ''
services: active-directory
ms.custom: it-pro
ms.openlocfilehash: f81664b8c84be6e319eadf55a02b0ec04f85fbca
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
ms.locfileid: "29285107"
---
# <a name="get-started-with-azure-ad"></a>Azure AD 入门
现代的标识管理需要一贯的可伸缩性和可靠性，确保只向经过身份验证的用户提供应用程序和服务。 为了充分支持用户对标识管理的需求，IT 部门需要通过某种方式提供对已批准的公用软件即服务 (SaaS) 应用的访问，托管内部业务线应用，甚至还需要通过某些方式来增强本地应用的开发与使用。 满足所有这些要求需要一种基于云的标识管理解决方案。      

Azure Active Directory (Azure AD) 是 Microsoft 提供的多租户、基于云的目录和标识管理服务。 Azure AD 将核心目录服务、高级标识监管和应用程序访问管理相结合。 Azure AD 的多租户、地理分布、高可用性设计意味着可以依赖它来解决最关键的业务需求。


![Azure AD ](./media/get-started-azure-ad/Azure_Active_Directory.png)

本文的余下部分说明如何开始使用 Azure AD。 




## <a name="add-a-custom-domain-name"></a>添加自定义域名
每个 Azure AD 目录都带有 *domainname*.partner.onmschina.cn 形式的初始域名。 无法更改或删除初始域名，但也可以[将企业域名添加到 Azure AD](add-custom-domain.md)。 例如，你的组织可能具有用来从事商业经营的其他域名和使用公司域名登录的用户。 通过在 Azure AD 中添加自定义域名可以在目录中分配用户熟悉的用户名，例如“alice@contoso.com”。 而不是“alice@.partner.onmschina.cn”。 过程很简单：

1. 将自定义域名添加到目录
2. 在域名注册机构中为域名添加 DNS 条目
3. 在 Azure AD 中验证自定义域名

### <a name="verification-step"></a>验证步骤
添加自定义域后，请确保它在 Azure AD 门户的“自定义域名”边栏选项卡上显示的状态为“已验证”。





## <a name="add-new-users"></a>添加新用户
可以使用 Azure 门户或通过同步本地 Windows Server AD 资源数据，逐个地[将新用户添加到组织的 Azure AD](add-users-azure-active-directory.md)。 可以直接从 Azure AD 门户添加基于云的用户，或同步本地用户信息。

若要启用本地标识到 Azure AD 的同步，需在组织的服务器上安装并配置 [Azure AD Connect](./connect/active-directory-aadconnect.md)。 该应用程序负责将用户和组从现有的标识源同步到 Azure AD 租户。

### <a name="verification-step"></a>验证步骤
创建或同步新用户之后，请确保他们显示在 Azure AD 中。



## <a name="next-steps"></a>后续步骤
[Azure Active Directory 服务页](https://www.azure.cn/home/features/identity/)
[Azure Active Directory 定价信息页](https://www.azure.cn/pricing/details/identity/)