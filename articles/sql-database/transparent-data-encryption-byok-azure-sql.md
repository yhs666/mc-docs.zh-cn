---
title: TDE - Azure Key Vault 集成或创建自己的密钥 (BYOK) - Azure SQL 数据库 |Microsoft Docs
description: SQL 数据库和数据仓库 Azure Key Vault 的透明数据加密 (TDE) 的“创建自己的密钥”(BYOK) 支持。 支持 BYOK 的 TDE 概述、优势、工作原理、注意事项和建议。
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: vanto
manager: digimobile
origin.date: 03/12/2019
ms.date: 04/15/2019
ms.openlocfilehash: 7bfff1626d7b953344d381c0e28b393b9bc5eff5
ms.sourcegitcommit: 9f7a4bec190376815fa21167d90820b423da87e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "59529507"
---
# <a name="azure-sql-transparent-data-encryption-with-customer-managed-keys-in-azure-key-vault-bring-your-own-key-support"></a>使用 Azure Key Vault 中由客户管理的密钥进行 Azure SQL 透明数据加密：自带密钥支持

集成了 Azure Key Vault 的[透明数据加密 (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) 允许使用由客户管理的非对称密钥（称为 TDE 保护器）对数据库加密密钥 (DEK) 进行加密。 这通常也称为透明数据加密的创建自己的密匙 (BYOK) 支持。  在 BYOK 方案中，TDE 保护器存储在由客户拥有和管理的 [Azure Key Vault](/key-vault/key-vault-secure-your-key-vault)（Azure 的基于云的外部密钥管理系统）中。 TDE 保护器可由密钥保管库[生成](/key-vault/about-keys-secrets-and-certificates)，也可以从本地 HSM 设备[转移](/key-vault/key-vault-hsm-protected-keys)到密钥保管库。 TDE DEK 存储在数据库的启动页上，由 TDE 保护器进行加密和解密，该保护器存储在 Azure Key Vault 中并且从不会离开密钥保管库。  需要向 SQL 数据库授予对客户管理的密钥保管库的权限才能对 DEK 进行解密和加密。 如果撤销了逻辑 SQL Server 对密钥保管库的权限，则数据库将无法访问，并且所有数据都是加密的。 对于 Azure SQL 数据库，TDE 保护器是在逻辑 SQL Server 级别设置的，并由该服务器关联的所有数据库继承。

使用集成了 Azure Key Vault 的 TDE，用户可以控制密钥管理任务，包括密钥轮换、密钥保管库权限、密钥备份，以及使用 Azure Key Vault 功能对所有 TDE 保护器启用审核/报告。 Key Vault 提供了集中化密钥管理功能，利用严格监控的硬件安全模块 (HSM)，并可在密钥与数据管理之间实现职责分离，以帮助满足安全策略的要求。  

集成了 Azure Key Vault 的 TDE 具有以下优势：

- 更高的透明度和细化控制，能够自我管理 TDE 保护器
- 随时能够撤销权限，使数据库不可访问
- 通过将 TDE 保护器（以及其他 Azure 服务中使用的其他密钥和机密）托管在 Key Vault 中，对其进行集中管理
- 将组织内部的密钥与数据管理责任相分离，以支持职责分离
- 自己的客户端更值得信赖，因为密钥保管库的设计可以防止 Azure 查看或提取任何加密密钥。
- 支持密钥轮换

> [!IMPORTANT]
> 对于目前正在使用服务托管的 TDE，但想要开始使用 Key Vault 的用户，在切换到 Key Vault 中 TDE 保护器的过程中，会保持启用 TDE。 这既不会造成停机，也无需重新加密数据库文件。 从服务托管的密钥切换到 Key Vault 密钥只需重新加密数据库加密密钥 (DEK)，此操作非常快捷且可在线完成。

## <a name="how-does-tde-with-azure-key-vault-integration-support-work"></a>支持 Azure Key Vault 集成的 TDE 如何工作

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure 资源管理器模块仍受 Azure SQL 数据库的支持，但所有未来的开发都是针对 Az.Sql 模块的。 若要了解这些 cmdlet，请参阅 [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)。 Az 模块和 AzureRm 模块中的命令参数大体上是相同的。

![在 Key Vault 中对服务器进行身份验证](./media/transparent-data-encryption-byok-azure-sql/tde-byok-server-authentication-flow.PNG)

首次将 TDE 配置为使用 Key Vault 中的 TDE 保护器后，服务器会将每个已启用 TDE 的数据库的 DEK 发送到 Key Vault，以发出包装密钥请求。 Key Vault 返回存储在用户数据库中的已加密数据库加密密钥。  

> [!IMPORTANT]
> 必须注意，将 TDE 保护器存储在 Azure Key Vault 中之后，它永远不会离开 Azure Key Vault。 服务器只能将密钥操作请求发送到 Key Vault 中的 TDE 保护器密钥材料，并且永远不会访问或缓存 TDE 保护器。 Key Vault 管理员随时有权撤销服务器的 Key Vault 权限，在这种情况下，与该服务器的所有连接将会断开。

## <a name="guidelines-for-configuring-tde-with-azure-key-vault"></a>有关配置采用 Azure Key Vault 的 TDE 的准则

### <a name="general-guidelines"></a>一般性指导

- 确保 Azure Key Vault 和 Azure SQL 数据库位于同一租户中。  **不支持** Key Vault 与服务器进行跨租户的交互。
- 确定要对所需的资源使用哪些订阅 – 以后跨订阅移动服务器需要重新设置支持 BYOK 的 TDE。 详细了解如何[移动资源](/azure-resource-manager/resource-group-move-resources)
- 配置采用 Azure Key Vault 的 TDE 时，必须考虑到重复的包装/解包操作在密钥保管库中施加的负载。 例如，由于与 SQL 数据库服务器关联的所有数据库使用相同的 TDE 保护器，因此，该服务器的故障转移将会针对保管库触发密钥操作：服务器中有多少个数据库，就会触发多少次密钥操作。 根据我们的经验以及 [Key Vault 服务限制](/key-vault/key-vault-service-limits)中所述，我们建议将最多 500 个标准/常规用途库或 200 个高级/业务关键型数据库与单个订阅中的 1 个 Azure Key Vault 相关联，以确保在访问保管库中的 TDE 保护器时能够获得一致的高可用性。

### <a name="guidelines-for-configuring-azure-key-vault"></a>有关配置 Azure Key Vault 的指导

- 创建启用[软删除](/key-vault/key-vault-ovw-soft-delete)的 Key Vault，以防在意外删除了密钥或 Key Vault 时丢失数据。  必须使用 [PowerShell 在 Key Vault 中启用“软删除”属性](/key-vault/key-vault-soft-delete-powershell)（目前无法从 AKV 门户使用此选项 – 但 Azure SQL 需要此选项）：  
  - 除非进行恢复或清除，否则软删除的资源将保留设置的一段时间（90 天）。
  - “恢复”和“清除”操作在 Key Vault 访问策略中各自具有相关联的权限。
- 在 Key Vault 中设置资源锁可以控制谁能删除此关键资源，并帮助防止意外或未经授权的删除。  [详细了解资源锁](/azure-resource-manager/resource-group-lock-resources)

- 使用 SQL 数据库服务器的 Azure Active Directory (Azure AD) 标识授予其对密钥保管库的访问权限。  使用门户 UI 时，会自动创建 Azure AD 标识，并向服务器授予 Key Vault 访问权限。  使用 PowerShell 配置支持 BYOK 的 TDE 时，必须手动创建 Azure AD 标识，且验证完成进度。 有关使用 PowerShell 进行配置的详细分步说明，请参阅[配置支持 BYOK 的 TDE](transparent-data-encryption-byok-azure-sql-configure.md)。

  > [!NOTE]
  > 如果**意外删除了 Azure AD 标识，或使用 Key Vault 的访问策略撤销了服务器的权限**，服务器将失去 Key Vault 的访问权限，并且 TDE 加密的数据库在 24 小时内将无法访问。

- 借助 Azure Key Vault 使用防火墙和虚拟网络时，必须配置以下内容： 
  - 允许受信任的 Microsoft 服务跳过此防火墙 - 选择“是”

  > [!NOTE]
  > 如果 TDE 加密 SQL 数据库由于无法绕过防火墙而失去对密钥保管库的访问权限，那么数据库在 24 小时内将无法访问。

- 为了确保已加密数据库的高可用性，请在每个 SQL 数据库服务器上配置两个驻留在不同区域的 Azure Key Vault。
