---
title: Azure Key Vault 概述
description: Azure Key Vault 是一项云服务，用作安全的机密存储。
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 34af20ee-3fa7-4f28-9d98-6168b1759764
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
origin.date: 05/08/2018
ms.date: 10/22/2018
ms.author: v-biyu
ms.openlocfilehash: 0a5cfba73f905b8892f678929b22c43b003ad13a
ms.sourcegitcommit: 2fdf25eb4b978855ff2832bcdcca093c141be261
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "49120616"
---
# <a name="what-is-azure-key-vault"></a>什么是 Azure 密钥保管库？

Azure Key Vault 有助于解决以下问题
- Azure Key Vault 可以用来安全地存储令牌、密码、证书、API 密钥和其他机密，并对其访问进行严格控制。
- Azure Key Vault 也可用作密钥管理解决方案。 可以通过 Azure Key Vault 轻松创建和控制用于加密数据的加密密钥。 
- Azure Key Vault 也是一项服务，可以用来轻松地预配、管理和部署公用和专用安全套接字层/传输层安全性 (SSL/TLS) 证书，这些证书可以与 Azure 以及你的内部连接资源配合使用。 


## <a name="why-use-azure-key-vault"></a>为何使用 Azure Key Vault？

### <a name="centralize-application-secrets"></a>集中管理应用程序机密

在 Azure Key Vault 中集中存储应用程序机密就可以控制其分发。 Key Vault 可以大大减少机密意外泄露的可能性。 有了 Key Vault，应用程序开发人员就再也不需要将安全信息存储在应用程序中。 这样就不需要将该信息存储在代码中。 例如，如果某个应用程序需要连接到数据库， 则可将连接字符串安全地存储在 Key Vault 中，而不是存储在应用代码中。

应用程序的密钥或机密存储在 Azure Key Vault 中以后，应用程序可以使用 URI 来检索特定版本的机密，从而安全地访问所需的信息。 这样就不需编写自定义代码来保护任何机密信息。

### <a name="securely-store-secrets-and-keys"></a>安全地存储机密和密钥

机密和密钥由 Azure 使用行业标准算法和密钥长度进行保护。

访问密钥保管库需要适当的身份验证和授权，否则调用方（用户或应用程序）无法进行访问。 身份验证用于确定调用方的身份，而授权则决定了调用方能够执行的操作。

身份验证通过 Azure Active Directory 来完成。 授权可以通过基于角色的访问控制 (RBAC) 或 Key Vault 访问策略来完成。 进行保管库的管理时，使用 RBAC；尝试访问存储在保管库中的数据时，使用密钥保管库访问策略。


最后需要指出的是，根据 Azure Key Vault 的设计，Microsoft 无法查看或提取数据。

### <a name="monitor-access-and-use"></a>监视访问和使用情况

创建多个 Key Vault 以后，需监视用户对密钥和机密进行访问的方式和时间。 为此，可以对 Key Vault 启用日志记录功能。 可以将 Azure Key Vault 配置为执行以下操作：

- 存档到存储帐户。
- 流式传输到事件中心。
- 将日志发送到 Log Analytics。

可以控制自己的日志，可以通过限制访问权限来确保日志的安全，还可以删除不再需要的日志。

### <a name="simplified-administration-of-application-secrets"></a>简化应用程序机密的管理

存储有价值的数据时，必须执行多项步骤。 安全信息必须受到保护，必须遵循某个生命周期，必须高度可用。 Azure Key Vault 通过以下措施对很多方面进行了简化：

- 收到通知后很快就可以进行纵向扩展，满足组织的使用高峰需求。
- 在某个区域内复制 Key Vault 的内容，并将其复制到次要区域。 Key Vault 可确保高可用性，不需管理员操作即可触发故障转移。
- 通过门户、Azure CLI 和 PowerShell 提供标准的 Azure 管理选项。
- 针对从公共 CA 购买的证书自动完成某些任务，如注册和续订。

另外，还可以通过 Azure Key Vault 来隔离应用程序机密。 应用程序只能访问其有权访问的保管库，并且只能执行特定的操作。 可以为每个应用程序创建一个 Azure Key Vault，仅限特定的应用程序和开发人员团队访问存储在 Key Vault 中的机密。

### <a name="integrate-with-other-azure-services"></a>与其他 Azure 服务集成

Key Vault 是 Azure 中的安全存储，一直用于简化多种方案，例如 [Azure 磁盘加密](../security/azure-security-disk-encryption.md)、SQL Server 和 Azure SQL 中的 [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) 功能。 Key Vault 本身可以与存储帐户、事件中心和 Log Analytics 集成。

## <a name="next-steps"></a>后续步骤

- [快速入门：使用 CLI 创建 Azure Key Vault](quick-create-cli.md)
- [配置 Azure Web 应用程序以从 Key Vault 读取机密](tutorial-web-application-keyvault.md)
