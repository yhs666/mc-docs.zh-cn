---
title: 什么是 Azure 密钥保管库？
description: Azure 密钥保管库可帮助保护云应用程序和服务使用的加密密钥和机密。
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 01/26/2017
ms.date: 11/05/2018
ms.author: v-biyu
ms.openlocfilehash: 9e60ff04b6bc254dfc5b5d2410d416e052e53ba9
ms.sourcegitcommit: 8a68d9275ddb92ea45601fed96e21559999d9579
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026960"
---
# <a name="what-is-azure-key-vault"></a>什么是 Azure 密钥保管库？
Azure Key Vault 有助于解决以下问题
- **机密管理** - Azure Key Vault 可以用来安全地存储令牌、密码、证书、API 密钥和其他机密，并对其访问进行严格控制
- **密钥管理** - Azure Key Vault 也可用作密钥管理解决方案。 可以通过 Azure Key Vault 轻松创建和控制用于加密数据的加密密钥。 

## <a name="basic-concepts"></a>基本概念

Azure Key Vault 是一个用于安全地存储和访问机密的工具。 机密是你希望严格控制对其的访问的任何东西，例如 API 密钥、密码或证书。 **保管库**是机密的逻辑组。 现在，若要使用密钥保管库执行任何操作，首先需要向其进行身份验证。 下面是一些关键术语：
- **租户** - 租户是拥有和管理特定的 Microsoft 云服务实例的组织。 它通常用来以确切的方式引用组织的 Azure 和 Office 365 服务集。
- **保管库所有者**：保管库所有者可以创建密钥保管库并获得它的完全访问权限和控制权。 保管库所有者还可以设置审核来记录谁访问了机密和密钥。 管理员可以控制密钥生命周期。 他们可以滚动到密钥的新版本、对其进行备份，以及执行相关的任务。
- **保管库使用者**：当保管库所有者为保管库使用者授予了访问权限时，使用者可以对密钥保管库内的资产执行操作。 可用操作取决于所授予的权限。
- **资源**：资源是可通过 Azure 获取的可管理项。 部分常见资源包括虚拟机、存储帐户、Web 应用、数据库和虚拟网络，但这只是其中一小部分。
- **资源组**：资源组是用于保存 Azure 解决方案相关资源的容器。 资源组可以包含解决方案的所有资源，也可以只包含想要作为组来管理的资源。 根据对组织有利的原则，决定如何将资源分配到资源组。
- **服务主体** - 可以将服务主体视为应用程序的凭据。
- **[Azure Active Directory (Azure AD)](/active-directory/fundamentals/active-directory-whatis)**：Azure AD 是租户的 Active Directory 服务。 每个目录有一个或多个域。 每个目录可以有多个订阅与之关联，但只有一个租户。 
- **Azure 租户 ID**：租户 ID 是用于在 Azure 订阅中标识 Azure AD 实例的唯一方法。


    

## <a name="key-vault-roles"></a>Key Vault 角色

使用下表详细了解密钥保管库如何帮助达到开发人员和安全管理员的需求。

| 角色 | 问题陈述 | Azure 密钥保管库已解决问题 |
| --- | --- | --- |
| Azure 应用程序开发人员      | “我想要编写使用密钥进行签名和加密的 Azure 应用程序，但希望这些密钥与应用程序分开，使解决方案适用于地理分散的应用程序。 <br/><br/>同时还希望这些密钥和机密都是经过加密的，而无需自己编写代码。 除此之外，希望这些密钥和机密在应用程序中易于使用，能实现最佳性能。” | √ 密钥存储在保管库中，可按需由 URI 调用。<br/><br/> √ 密钥由 Azure 使用行业标准的算法、密钥长度和硬件安全模块进行保护。
| 软件即服务 (SaaS) 开发人员 |“对于客户的租户密钥和机密，我不想承担任何实际或潜在法律责任。 <br/><br/>我希望客户拥有并管理他们自己的密钥，使我能够将全部精力集中在我的专长上，也就是提供核心软件功能。” |√ 客户可以将他们自己的密钥导入 Azure 并进行管理。 当 SaaS 应用程序需要使用其客户密钥来执行加密操作时，密钥保管库将代表应用程序执行这些操作。 应用程序看不到客户的密钥。 |
| 首席安全官 (CSO) |“我想要确保我的组织掌控密钥生命周期，并可监视密钥的使用。 <br/><br/>此外，即使我们使用多个 Azure 服务和资源，我也想要从 Azure 中的单个位置管理密钥。” |√ Key Vault 设计用于确保 Microsoft 不会看到或提取你的密钥。<br/><br/>√ 以近实时方式记录密钥的使用。<br/><br/>√ 无论 Azure 中拥有的密钥数量，以及支持的地区和使用这些密钥的应用程序，保管库都仅提供单个界面。 |

具有 Azure 订阅的任何人都可以创建和使用密钥保管库。 尽管密钥保管库能够为开发人员和安全管理员提供便利，但管理组织中其他 Azure 服务的管理员也可以实现和管理密钥保管库。 例如，此管理员可以使用 Azure 订阅登录、创建组织用来存储密钥的保管库，并负责执行操作任务，例如：

* 创建或导入密钥或机密
* 吊销或删除密钥或机密
* 授权用户或应用程序访问密钥保管库，使它们能够管理或使用其密钥和机密
* 配置密钥用法（例如，签名或加密）
* 监视密钥用法

然后，此管理员会为开发人员提供可从其应用程序调用的 URI，并为其安全管理员提供密钥用法日志记录信息。 

![Azure 密钥保管库概述][1]

开发人员还可通过使用 API 直接管理密钥。 有关详细信息，请参阅 [ 开发人员指南](key-vault-developers-guide.md)。

## <a name="next-steps"></a>后续步骤

有关面向管理员的入门教程，请参阅 [Azure Key Vault 入门](key-vault-get-started.md)。


有关将密钥和机密与 Azure Key Vault 配合使用的详细信息，请参阅 [About keys, secrets, and certificates](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx)（关于密钥、机密和证书）。

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
大多数区域都提供了 Azure 密钥保管库。 有关详细信息，请参阅 [密钥保管库定价页](https://www.azure.cn/pricing/details/key-vault/)。

<!-- Update_Description: wording update -->