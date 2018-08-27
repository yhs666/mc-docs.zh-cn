---
title: 什么是 Azure 密钥保管库？ | Microsoft Docs
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
ms.topic: get-started-article
origin.date: 01/26/2017
ms.date: 08/14/2018
ms.author: v-junlch
ms.openlocfilehash: 416d73a76af0da7b17f11572109a6468bef2102b
ms.sourcegitcommit: 56aa1615ef7402444111495f72afbdd6b2dfff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2018
ms.locfileid: "41705331"
---
# <a name="what-is-azure-key-vault"></a>什么是 Azure 密钥保管库？
Azure 密钥保管库可帮助保护云应用程序和服务使用的加密密钥和机密。 借助 Key Vault，可使用密钥来加密密钥和机密（例如身份验证密钥、存储帐户密钥、数据加密密钥、.PFX 文件和密码）。

密钥保管库简化了密钥管理过程，可让你控制用于访问和加密数据的密钥。 开发人员可以在几分钟内创建用于开发和测试的密钥，并无缝地将其迁移到生产密钥。 安全管理员可以根据需要授予（和吊销）密钥权限。

使用下表更好地了解密钥保管库如何帮助达到开发人员和安全管理员的需求。

| 角色 | 问题陈述 | Azure 密钥保管库已解决问题 |
| --- | --- | --- |
| Azure 应用程序开发人员      | “我想要编写使用密钥进行签名和加密的 Azure 应用程序，但希望这些密钥与应用程序分开，使解决方案适用于地理分散的应用程序。 <br/><br/>同时还希望这些密钥和机密都是经过加密的，而无需自己编写代码。 除此之外，希望这些密钥和机密在应用程序中易于使用，能实现最佳性能。” | √ 密钥存储在保管库中，可按需由 URI 调用。<br/><br/> √ 密钥由 Azure 通过行业标准算法和密钥长度进行保护。
| 软件即服务 (SaaS) 开发人员 |“对于客户的租户密钥和机密，我不想承担任何实际或潜在法律责任。 <br/><br/>我希望客户拥有并管理他们自己的密钥，使我能够将全部精力集中在我的专长上，也就是提供核心软件功能。” |√ 客户可以将他们自己的密钥导入 Azure 并进行管理。 当 SaaS 应用程序需要使用其客户密钥来执行加密操作时，密钥保管库将代表应用程序执行这些操作。 应用程序看不到客户的密钥。 |
| 首席安全官 (CSO) | “我想要确保我的组织掌控密钥生命周期，并可监视密钥用法。 <br/><br/>此外，即使我们使用多个 Azure 服务和资源，我也想要从 Azure 中的单个位置管理密钥。”     |√ Key Vault 设计用于确保 Microsoft 不会看到或提取你的密钥。<br/><br/>√ 准实时记录密钥的用法。<br/><br/>√ 无论 Azure 中拥有的密钥数量，以及支持的地区和使用这些密钥的应用程序，保管库都仅提供单个界面。 |

具有 Azure 订阅的任何人都可以创建和使用密钥保管库。 尽管密钥保管库能够为开发人员和安全管理员提供便利，但管理组织中其他 Azure 服务的管理员也可以实现和管理密钥保管库。 例如，此管理员可以使用 Azure 订阅登录、创建组织用来存储密钥的保管库，并负责执行操作任务，例如：

- 创建或导入密钥或机密
- 吊销或删除密钥或机密
- 授权用户或应用程序访问密钥保管库，使它们能够管理或使用其密钥和机密
- 配置密钥用法（例如，签名或加密）
- 监视密钥用法

然后，此管理员会为开发人员提供可从其应用程序调用的 URI，并为其安全管理员提供密钥用法日志记录信息。 

   ![Azure 密钥保管库概述][1]

开发人员还可通过使用 API 直接管理密钥。 有关详细信息，请参阅 [ 开发人员指南](key-vault-developers-guide.md)。

## <a name="next-steps"></a>后续步骤
有关面向管理员的入门教程，请参阅 [Azure Key Vault 入门](key-vault-get-started.md)。

有关将密钥和机密与 Azure Key Vault 配合使用的详细信息，请参阅 [关于密钥、机密和证书](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx)。

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
大多数区域都提供了 Azure 密钥保管库。 有关详细信息，请参阅 [密钥保管库定价页](https://www.azure.cn/pricing/details/key-vault/)。

<!-- Update_Description: wording update -->