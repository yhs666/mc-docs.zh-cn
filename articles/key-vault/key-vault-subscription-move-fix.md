---
title: 订阅移动后更改密钥保管库租户 ID | Microsoft Docs
description: 了解如何在订阅移至不同的租户后切换密钥保管库的租户 ID
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 46d7bc21-fa79-49e4-8c84-032eef1d813e
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
origin.date: 01/07/2017
ms.date: 07/10/2018
ms.author: v-junlch
ms.openlocfilehash: e2ff445d80a11fc2dc8bc2e3beeb0d3b3a0d3610
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52651126"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>订阅移动后更改密钥保管库租户 ID
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>问：我的订阅已从租户 A 移动到租户 B。如何更改我的现有密钥保管库的租户 ID，并为租户 B 中的主体设置正确的 ACL？
在订阅中创建新的密钥保管库时，该密钥保管库自动绑定到该订阅的默认 Azure Active Directory 租户 ID。 所有访问策略条目也都绑定到此租户 ID。 将 Azure 订阅从租户 A 移到租户 B 时，租户 B 中的主体（用户和应用程序）无法访问现有的密钥保管库。若要解决此问题，需要执行以下操作：

- 将此订阅中与所有现有密钥保管库关联的租户 ID 更改为租户 B 的 ID。
- 删除所有现有的访问策略条目。
- 添加与租户 B 相关联的新访问策略条目。

例如，如果订阅中的密钥保管库“myvault”从租户 A 移到租户 B，以下介绍了如何更改此密钥保管库的租户 ID，以及删除旧的访问策略。

<pre>
Select-AzureRmSubscription -SubscriptionId YourSubscriptionID
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId
$vault = Get-AzureRmResource -ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

因为移动前此保管库位于租户 A，所以 $vault.Properties.TenantId 的原始值为租户 A，而 (Get-AzureRmContext).Tenant.TenantId 的值为租户 B。

现在，保管库已与正确的租户 ID 相关联，并且旧访问策略条目已删除，请使用 *Set-AzureRmKeyVaultAccessPolicy*设置新的访问策略条目。

## <a name="next-steps"></a>后续步骤
如果在 Azure Key Vault 方面有任何问题，请访问 [Azure Key Vault 论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)。


<!-- Update_Description: code and link update -->