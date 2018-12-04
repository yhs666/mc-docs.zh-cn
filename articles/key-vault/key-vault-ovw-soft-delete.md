---
ms.assetid: ''
title: Azure Key Vault 软删除 | Microsoft Docs
ms.service: key-vault
ms.topic: conceptual
author: bryanla
ms.author: v-biyu
manager: mbaldwin
origin.date: 09/25/2017
ms.date: 11/05/2018
ms.openlocfilehash: 9ced02a0e2396c427be7adab12b3763335f6c879
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52647653"
---
# <a name="azure-key-vault-soft-delete-overview"></a>Azure Key Vault 软删除概述

Key Vault 的软删除功能可以恢复已删除的保管库和保管库对象，称为软删除。 本文将具体探讨以下方案：

- 支持 Key Vault 的可恢复删除
- 支持 Key Vault 对象的可恢复删除（例如 提供的支持

## <a name="supporting-interfaces"></a>支持接口

软删除功能最初通过 REST、.NET/C#、PowerShell 和 CLI 接口提供。

有关常规信息，请参阅参考文献 [Key Vault 参考](/key-vault/)了解详细信息。

## <a name="scenarios"></a>方案

Azure Key Vault 是由 Azure Resource Manager 管理的跟踪资源。 Azure Resource Manager 还指定了定义明确的删除行为，要求成功的删除操作必须使该资源不再可供访问。 软删除功能解决了已删除对象的恢复问题，无论是意外删除还是有意删除。

1. 在典型情景中，用户可能无意中删除了 Key Vault 或 Key Vault 对象；如果 Key Vault 或 Key Vault 对象在预设的某个时间段内可恢复，则用户可以撤消删除并恢复其数据。

2. 在另一种情景中，恶意用户可能会试图删除 Key Vault 或 Key Vault 对象（例如保管库内的密钥），导致业务中断。 作为安全措施，可将 Key Vault 或 Key Vault 对象的删除与基础数据的实际删除区分开，例如，将数据删除的权限限制为不同的受信任角色。 此方法实际上需要对可能导致数据立即丢失的操作进行仲裁。

### <a name="soft-delete-behavior"></a>软删除行为

有此功能时，对 Key Vault 或 Key Vault 对象的 DELETE 操作是软删除，因此可以有效地在给定保留期（90 天）内保留资源，同时通过外观提示已删除对象。 该服务进一步提供了用于恢复已删除对象的机制，实质上是撤消删除。 

软删除是一种可选的 Key Vault 行为，在此版本中**默认未启用**。 

### <a name="purge-protection--flag"></a>清除保护标志
默认情况下，清除保护（Azure CLI 中的 --enable-purge-protection）标志处于禁用状态。 启用此标志时，在长达 90 天的保留期到期之前，不能清除处于已删除状态的保管库或对象。 仍可以恢复此类保管库或对象。 此标志可为客户增加保障，在保留期到期之前，永远不会永久删除保管库或对象。 仅当启用了软删除标志或在创建保管库时启用软删除和清除保护时，才能启用清除保护标志。

[!NOTE] 启用清除保护的先决条件是必须已启用软删除。 Azure CLI 2 中可实现此目的的命令是

```
az keyvault create --name "VaultName" --resource-group "ResourceGroupName" --location chinanorth --enable-soft-delete true --enable-purge-protection true
```

### <a name="permitted-purge"></a>允许的清除

可通过对代理资源执行 POST 操作永久删除、清除 Key Vault，但此操作需要特殊权限。 通常，只有订阅所有者才能清除 Key Vault。 POST 操作可触发立即删除该保管库，且此删除不可恢复。 

一种例外情况是，
- Azure 订阅已被标记为“不可删除”。 在这种情况下，只有服务可以执行实际删除，并且将作为计划的进程执行此操作。 
- 在保管库本身上启用 --enable-purge-protection 标志。 在这种情况下，Key Vault 将自原始机密对象标记为删除以永久删除该对象起等待 90 天。

### <a name="key-vault-recovery"></a>Key Vault 恢复

删除 Key Vault 后，服务会在订阅下创建代理资源，为恢复添加足够的元数据。 代理资源是一个存储对象，位于与已删除 Key Vault 相同的位置。 

### <a name="key-vault-object-recovery"></a>Key Vault 对象恢复

删除 Key Vault 对象（例如密钥）时，服务会将该对象置于已删除状态，从而使其不可供任何检索操作访问。 在此状态下，只能列出、恢复或强制/永久删除 Key Vault 对象。 

同时，Key Vault 将计划在预设的保留间隔后删除与已删除 Key Vault 或 Key Vault 对象对应的基础数据。 在保留间隔内，还会保留与该保管库相对应的 DNS 记录。

### <a name="soft-delete-retention-period"></a>软删除保留期

软删除的资源将保留设定的一段时间（90 天）。 以下项在软删除保留间隔期间适用：

- 可以列出订阅中处于软删除状态的所有 Key Vault 和 Key Vault 对象，并可访问与这些对象有关的删除和恢复信息。
    - 只有具有特殊权限的用户才能列出已删除的保管库。 我们建议用户创建一个具有这些特殊权限的自定义角色来处理已删除的保管库。
- 无法在同一位置创建具有相同名称的 Key Vault；相应地，在创建 Key Vault 对象时，如果 Key Vault 中包含具有相同名称且处于已删除状态的对象，则无法在其中创建该对象。 
- 只有特权用户可以还原 Key Vault 或 Key Vault 对象，方法是对相应的代理资源发出恢复命令。
    - 有权在资源组下创建 key vault 的用户（自定义角色的成员）可以还原该保管库。
- 只有特权用户可以强制删除 Key Vault 或 Key Vault 对象，方法是对相应的代理资源发出删除命令。

除非恢复 Key Vault 或 Key Vault 对象，否则在保留间隔结束时，服务将清除已软删除的 Key Vault 或 Key Vault 对象及其内容。 资源删除不可重新计划。

### <a name="billing-implications"></a>计费影响

一般情况下，如果对象（密钥保管库或密钥或机密）处于已删除状态，仅可执行两个操作：“清除”和“恢复”。 所有其他操作都会失败。 因此，即使对象存在，也不可执行任何操作，因此不会出现使用情况，也不会计费。 但是存在以下例外：

- “清除”和“恢复”操作计为正常密钥保管库操作并收费。

## <a name="next-steps"></a>后续步骤

以下两个指南提供有关使用软删除的主要使用方案。

- [如何将 Key Vault 软删除与 PowerShell 配合使用](key-vault-soft-delete-powershell.md) 
- [如何将 Key Vault 软删除与 CLI 配合使用](key-vault-soft-delete-cli.md)

<!-- Update_Description: wording update -->
