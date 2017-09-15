---
title: "Azure 密钥保管库开发人员指南"
description: "开发人员可以使用 Azure 密钥保管库来管理 Azure 环境中的加密密钥。"
services: key-vault
author: alexchen2016
manager: digimobile
ms.service: key-vault
ms.topic: article
ms.workload: identity
origin.date: 08/04/2017
ms.date: 09/07/2017
ms.author: v-junlch
ms.openlocfilehash: 461dc27f5a1286c2d33c5344579f1074f101a8d5
ms.sourcegitcommit: 76a57f29b1d48d22bb4df7346722a96c5e2c9458
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2017
---
# <a name="azure-key-vault-developers-guide"></a>Azure 密钥保管库开发人员指南

使用 Key Vault 可以从应用程序中安全地访问敏感信息：

- 无需自己编写代码即可保护密钥和机密信息，并且能够轻松地在应用程序中使用它们。
- 能够让客户拥有和管理其自己的密钥，因此可以专注于提供核心软件功能。 这样，应用程序便不会对客户的租户密钥和机密承担职责或潜在责任。
- 应用程序可以使用密钥进行签名和加密，不过使密钥管理与应用程序分开，可以使解决方案适用于地理分散的应用。
- 自 2016 年 9 月版本的 Key Vault 发布起，应用程序现在可以使用 Key Vault [证书](https://docs.microsoft.com/rest/api/keyvault/certificate-operations)。 有关详细信息，请参阅 [About keys, secrets, and certificates](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates)（关于密钥、机密和证书）。

有关 Azure Key Vault 的更多常规信息，请参阅[什么是 Key Vault](key-vault-whatis.md)。

## <a name="public-previews"></a>公共预览版

我们会定期发布新 Key Vault 功能的公共预览版。 欢迎试用这些版本，并通过反馈电子邮件地址 azurekeyvault@microsoft.com 告知提供看法。

### <a name="storage-account-keys---july-10-2017"></a>存储帐户密钥 - 2017 年 7 月 10 日

>[!NOTE]
>在 Azure Key Vault 的此更新版中，只有**存储帐户密钥**功能以预览版提供。

此预览版包含通过 [.NET/C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault/)、[REST](https://docs.microsoft.com/rest/api/keyvault/) 和 [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) 接口提供的新存储帐户密钥功能。 

有关新存储帐户密钥功能的详细信息，请参阅 [Azure Key Vault 存储帐户密钥概述](key-vault-ovw-storage-keys.md)。

## <a name="creating-and-managing-key-vaults"></a>创建和管理密钥保管库

在代码中使用 Azure 密钥保管库之前，可以通过 REST、 Resource Manager 模板、PowerShell 或 CLI 创建和管理保管库，如以下文章中所述：

- [使用 REST 创建和管理密钥保管库](https://docs.microsoft.com/rest/api/keyvault/)
- [使用 PowerShell 创建和管理密钥保管库](key-vault-get-started.md)
- [使用 CLI 创建和管理密钥保管库](key-vault-manage-with-cli2.md)
- [通过 Azure Resource Manager 模板创建密钥保管库并添加机密](../azure-resource-manager/resource-manager-template-keyvault.md)

> [!NOTE]
> 针对密钥保管库执行的操作通过 AAD 进行身份验证并通过密钥保管库自己的访问策略（按保管库定义）进行授权。

## <a name="coding-with-key-vault"></a>使用密钥保管库进行编码

程序员的 Key Vault 管理系统包含多个接口，并以 REST 作为基础。 通过 REST 接口，可以访问所有 Key Vault 资源：密钥、机密和证书。 [Key Vault REST API Reference](https://docs.microsoft.com/rest/api/keyvault/)（Key Vault REST API 参考）。 

### <a name="supported-programming-languages"></a>支持的编程语言

#### <a name="net"></a>.NET

- [.NET API refence for Key Vault](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault)（适用于 Key Vault 的 .NET API 参考） 

有关 .NET SDK 2.x 版的详细信息，请参阅[发行说明](key-vault-dotnet2api-release-notes.md)。

#### <a name="java"></a>Java

- [Java SDK for Key Vault](/java/api/com.microsoft.azure.keyvault)（适用于 Key Vault 的 Java SDK）

#### <a name="nodejs"></a>Node.js

在 Node.js 中，保管库管理 API 和保管库对象 API 是独立的。 使用 Key Vault 管理可以创建和更新 Key Vault。 Key Vault 操作 API 用于处理保管库对象，例如密钥、机密和证书。 

- [适用于 Key Vault 管理的 Node.js API 参考](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest/)
- [Node.js API reference for Key Vault Operations](http://azure.github.io/azure-sdk-for-node/azure-keyvault/latest/)（适用于 Key Vault 操作的 Node.js API 参考） 

### <a name="quick-start"></a>快速启动

- [Create Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)（创建 Key Vault）
- [Getting started with Key Vault in Node.js](https://azure.microsoft.com/en-us/resources/samples/key-vault-node-getting-started/)（Node.js 中的 Key Vault 入门）

### <a name="code-examples"></a>代码示例

有关在应用程序中使用密钥保管库的完整示例，请参阅：

- [Azure Key Vault 代码示例](http://www.microsoft.com/download/details.aspx?id=45343) - .NET 示例应用程序 HelloKeyVault 和 Azure Web 服务示例。 
- [从 Web 应用程序使用 Azure Key Vault](key-vault-use-from-web-application.md) - 此教程介绍如何从 Azure 中的 Web 应用程序使用 Azure Key Vault。 

## <a name="how-tos"></a>操作方法

以下文章和方案提供了特定于任务的指导，方便用户使用 Azure Key Vault：

- [订阅移动后更改密钥保管库租户 ID](key-vault-subscription-move-fix.md) - 将 Azure 订阅从租户 A 移到租户 B 时，租户 B.中的主体（用户和应用程序）无法访问现有的密钥保管库。使用本指南解决此问题。
- [访问防火墙后面的密钥保管库](key-vault-access-behind-firewall.md) - 若要访问密钥保管库，密钥保管库客户端应用程序需要能够访问多个终结点才能使用各种功能。
- [如何在部署期间传递安全值（如密码）](../azure-resource-manager/resource-manager-keyvault-parameter.md) - 需要在部署期间以参数形式传递安全值（例如密码）时，可以将该值存储为 Azure Key Vault 中的机密，并在其他资源管理模板中引用该值。
- [如何使用 Key Vault，以便通过 SQL Server 进行可扩展的密钥管理](https://msdn.microsoft.com/library/dn198405.aspx) - 适用于 Azure Key Vault 的 SQL Server 连接器允许 SQL Server 和 VM 中的 SQL 将 Azure Key Vault 服务用作可扩展密钥管理 (EKM) 提供程序，以便保护其针对应用程序链接的加密密钥；透明数据加密、备份加密和列级加密。
- [如何将 Key Vault 中的证书部署到 VM](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - 在 Azure 上的 VM 中运行的云应用程序需要一个证书。 现在，如何将此证书部署到此 VM 中？
- [如何使用端到端密钥轮换和审核设置 Key Vault](key-vault-key-rotation-log-monitoring.md) - 逐步介绍如何设置 Azure Key Vault 的密钥轮换和审核。
- [通过 Key Vault 部署 Azure Web 应用证书]( https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)提供有关部署作为[应用服务证书](https://azure.microsoft.com/blog/internals-of-app-service-certificate/)产品的一部分存储在 Key Vault 中的证书的分步说明。
- 如需更多将 Key Vault 与 Azure 集成和结合使用的特定于任务的指导，请参阅 [Ryan Jones Azure Resource Manager template examples for Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)（针对 Key Vault 的 Ryan Jones Azure 资源管理器模板示例）。
- [如何将 Key Vault 软删除与 CLI 配合使用](key-vault-soft-delete-cli.md)介绍了 Key Vault 的使用和生命周期以及各种已启用软删除的 Key Vault 对象。
- [如何将 Key Vault 软删除与 PowerShell 配合使用](key-vault-soft-delete-powershell.md)介绍了 Key Vault 的使用和生命周期以及各种已启用软删除的 Key Vault 对象。

## <a name="key-vault-overviews-and-concepts"></a>Key Vault 概述和概念

- [Key Vault 软删除行为](key-vault-ovw-soft-delete.md)介绍了一种可以恢复已删除的对象的功能（不管是有意还是无意删除）。
- [Key Vault 客户端限制](key-vault-ovw-throttling.md)介绍了有关限制的基本概念，并针对应用提供了限制方法。
- [Key Vault 存储帐户密钥概述](key-vault-ovw-storage-keys.md)介绍了 Key Vault 与 Azure 存储帐户密钥的集成。
- [Key Vault 安全体系](key-vault-ovw-security-worlds.md)介绍了区域与安全领域之间的关系。

## <a name="social"></a>社交

- [密钥保管库博客](http://aka.ms/kvblog)
- [密钥保管库论坛](http://aka.ms/kvforum)

## <a name="supporting-libraries"></a>支持库

- [Azure Key Vault 核心库](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core)提供 IKey 和 IKeyResolver 接口，用于通过标识符查找密钥，以及使用密钥执行操作。
- [Azure 密钥保管库扩展](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) 为 Azure 密钥保管库提供了扩展功能。

<!-- Update_Description: wording update -->


