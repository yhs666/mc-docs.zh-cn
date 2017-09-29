---
ms.assetid: 
title: "Azure Key Vault - 如何将软删除与 PowerShell 配合使用"
description: "使用 PowerShell 代码段进行软删除的用例示例"
services: key-vault
author: alexchen2016
manager: digimobile
ms.service: key-vault
ms.topic: article
ms.workload: identity
origin.date: 08/21/2017
ms.date: 09/25/2017
ms.author: v-junlch
ms.openlocfilehash: f479e5d79dda018ac8c55c570c3251350c5490b9
ms.sourcegitcommit: c13aee6f5e18d15bcc29fae1eefd2b72f2558dfa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2017
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a>如何将 Key Vault 软删除与 PowerShell 配合使用

Azure Key Vault 的软删除功能可以恢复已删除的保管库和保管库对象。 软删除将具体探讨以下方案：

- 支持 Key Vault 的可恢复删除
- 支持密钥保管库对象、密钥、机密和证书的可恢复删除

## <a name="prerequisites"></a>先决条件

- Azure PowerShell 4.0.0 或更高版本 - 若尚未安装此产品，请安装 Azure PowerShell 并将其与 Azure 订阅相关联，请参阅[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)。 

>[!NOTE]
> 环境中可能加载了过期版本的 Key Vault PowerShell 输出格式化文件，而没有加载正确版本。 预期 PowerShell 的更新版本将包含输出格式所需的更正，届时将更新此主题。 如果遇到此格式问题，当前的解决方法是：
> - 如果发现未看到此主题中所述的已启用软删除的属性，请使用以下查询：`$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`。


有关适用于 PowerShell 的 Key Vault 特定引用信息，请参阅 [Azure Key Vault PowerShell 引用](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0)。

## <a name="required-permissions"></a>所需的权限

Key Vault 操作通过基于角色的访问控制 (RBAC) 权限单独管理，如下所示：

| 操作 | 说明 | 用户权限 |
|:--|:--|:--|
|列出|列出已删除的密钥保管库。|Microsoft.KeyVault/deletedVaults/read|
|恢复|还原已删除的 Key Vault。|Microsoft.KeyVault/vaults/write|
|清除|永久移除已删除的 Key Vault 及其所有内容。|Microsoft.KeyVault/locations/deletedVaults/purge/action|

有关权限和访问控制的详细信息，请参阅[保护 Key Vault](key-vault-secure-your-key-vault.md)。

## <a name="enabling-soft-delete"></a>启用软删除

若要能够恢复已删除的 Key Vault 或存储在 Key Vault 中的对象，必须首先为该 Key Vault 启用软删除。

### <a name="existing-key-vault"></a>现有的 Key Vault

对于名为 ContosoVault 的现有 Key Vault，请按如下所示启用软删除。 

>[!NOTE]
>目前，需要使用 Azure 资源管理器资源操作直接将 *enableSoftDelete* 属性写入 Key Vault 资源。

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>新的 Key Vault

通过向创建命令添加软删除启用标志，在创建时启用对新 Key Vault 的软删除。

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "ChinaNorth" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>验证软删除支持

若要验证 Key Vault 是否已启用软删除，请运行 *get* 命令，并查找“Soft Delete Enabled?” 属性及其设置（true 或 false）。

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>删除由软删除保护的 Key Vault

删除（或移除）Key Vault 的命令保持不变，但其行为根据是否启用了软删除而改变。

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
>如果为没有启用软删除的 Key Vault 运行上一个命令，则将永久删除此 Key Vault 及其所有内容，而没有任何恢复选项。

### <a name="how-soft-delete-protects-your-key-vaults"></a>软删除如何保护 Key Vault

已启用软删除：

- 删除一个 Key Vault 时，将它从其资源组中删除，放置在一个仅与其创建位置相关联的保留命名空间中。 
- 无法访问删除的 Key Vault 中的对象（例如密钥、机密和证书），并且在包含它们的 Key Vault 处于已删除状态时依然如此。 
- 处于已删除状态的 Key Vault 的 DNS 名称仍然保留，因此无法创建同名的新 Key Vault。  

使用以下命令，可查看与订阅关联且处于已删除状态的 Key Vault：

```powershell
PS C:\> Get-AzureRmKeyVault -InRemovedStateVault 

Name           : ContosoVault
Location             : ChinaNorth
Id                   : /subscriptions/xxx/providers/Microsoft.KeyVault/locations/ChinaNorth/deletedVaults/ContosoVault
Resource ID          : /subscriptions/xxx/resourceGroups/ContosoVault/providers/Microsoft.KeyVault/vaults/ContosoVault
Deletion Date        : 5/9/2017 12:14:14 AM
Scheduled Purge Date : 8/7/2017 12:14:14 AM
Tags                 :
```

输出中的“Resource ID”是指该保管库的原始资源 ID。 由于此 Key Vault 现在处于已删除状态，因此该资源 ID 不存在任何资源。 “Id”字段可用于在恢复或清除时识别资源。 “Scheduled Purge Date”字段指示如果不对此已删除的保管库执行任何操作，会永久删除（清除）该保管库的时间。 用于计算“Scheduled Purge Date”的默认保留期是 90 天。

## <a name="recovering-a-key-vault"></a>恢复 Key Vault

若要恢复 Key Vault，需要指定 Key Vault 名称、资源组和位置。 请注意已删除的 Key Vault 的位置和资源组，以便用于 Key Vault 恢复过程。

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location ChinaNorth
```

恢复 Key Vault 后，会得到具有 Key Vault 原始资源 ID 的新资源。 如果 Key Vault 所在的资源组已被删除，则必须先创建同名的新资源组，然后才能恢复 Key Vault。

## <a name="key-vault-objects-and-soft-delete"></a>Key Vault 对象和软删除

下面介绍对于密钥“ContosoFirstKey”，在启用了软删除的名为“ContosoVault”的 Key Vault 中，如何删除该密钥。

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

为软删除启用 Key Vault 后，删除的密钥仍然显示为已删除，除非明确列出或检索已删除的密钥。 对处于已删除状态的密钥的大多数操作会失败，列出、恢复或清除已删除的密钥除外。 

例如，若要请求列出 Key Vault 中已删除的密钥，请使用以下命令：

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>转换状态 

在启用了软删除的 Key Vault 中删除密钥时，可能需要几秒钟时间完成转换。 在此转换状态期间，密钥可能不处于活动状态或已删除状态。 此命令会列出名为“ContosoVault”的 Key Vault 中的所有已删除的密钥。

```powershell
  Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
  Vault Name           : ContosoVault
  Name                 : ContosoFirstKey
  Id                   : https://ContosoVault.vault.azure.cn:443/keys/ContosoFirstKey
  Deleted Date         : 2/14/2017 8:20:52 PM
  Scheduled Purge Date : 5/15/2017 8:20:52 PM
  Enabled              : True
  Expires              :
  Not Before           :
  Created              : 2/14/2017 8:16:07 PM
  Updated              : 2/14/2017 8:16:07 PM
  Tags                 :
```

### <a name="using-soft-delete-with-key-vault-objects"></a>将软删除用于 Key Vault 对象

就像密钥保管库一样，除非恢复或清除已删除的密钥、机密或证书，否则它将保持已删除状态最多 90 天。 

#### <a name="keys"></a>密钥

恢复已删除的密钥：

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

永久删除密钥：

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
>清除密钥会永久删除，这意味着无法恢复。

“恢复”和“清除”操作在 Key Vault 访问策略中各自具有相关联的权限。 用户或服务主体若要执行“恢复”或“清除”操作，必须在 Key Vault 访问策略中具有该对象（密钥或机密）的相应权限。 默认情况下，使用“全部”快捷方式向用户授予所有权限时，“清除”权限不会添加到 Key Vault 访问策略中。 必须明确授予“清除”权限。 例如，以下命令授予 user@contoso.com 对“ContosoVault”中的密钥执行多项操作（包括“清除”）的权限。

#### <a name="set-a-key-vault-access-policy"></a>设置 Key Vault 访问策略

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> 如果现有 Key Vault 刚刚启用软删除，则可能没有“恢复”和“清除”权限。

#### <a name="secrets"></a>机密

像密钥一样，Key Vault 中的机密都用自己的命令进行操作。 以下是删除、列出、恢复和清除机密的命令。

- 删除名为 SQLPassword 的机密： 
  ```powershell
  Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
  ```

- 列出 Key Vault 中所有已删除的机密： 
  ```powershell
  Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
  ```

- 恢复处于已删除状态的机密： 
  ```powershell
  Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
  ```

- 清除处于已删除状态的机密： 
  ```powershell
  Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
  ```

>[!NOTE]
>清除机密会永久删除，这意味着无法恢复。

## <a name="purging-and-key-vaults"></a>清除和 Key Vault

### <a name="key-vault-objects"></a>Key Vault 对象

清除密钥、机密或证书会永久删除，这意味着无法恢复。 然而，包含已删除对象的 Key Vault 会保持不变，Key Vault 中的所有其他对象也会保持不变。 

### <a name="key-vaults-as-containers"></a>Key Vault 作为容器
清除 Key Vault 时，会永久删除其所有内容（包括密钥、机密和证书）。 若要清除 Key Vault，请使用具有 `-InRemovedState` 选项的命令 `Remove-AzureRmKeyVault`，并通过使用 `-Location location` 参数指定已删除的 Key Vault 的位置。 可以使用命令 `Get-AzureRmKeyVault -InRemovedState` 查找已删除的保管库的位置。

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location ChinaNorth
```

>[!NOTE]
>清除 Key Vault 会永久删除，这意味着无法恢复。

### <a name="purge-permissions-required"></a>所需的清除权限
- 若要清除已删除的 Key Vault，以便永久删除保管库及其所有内容，用户需要 RBAC 权限执行 *Microsoft.KeyVault/locations/deletedVaults/purge/action* 操作。 
- 若要列出已删除的密钥，用户需要 RBAC 权限执行 *Microsoft.KeyVault/deletedVaults/read* 权限。 
- 默认情况下，只有订阅管理员具有这些权限。 

### <a name="scheduled-purge"></a>计划清除

列出已删除的 Key Vault 对象会显示 Key Vault 计划将其清除的时间。 “Scheduled Purge Date”字段指示如果不采取任何操作，会永久删除 Key Vault 对象的时间。 默认情况下，已删除的 Key Vault 对象的保留期为 90 天。

>[!NOTE]
>已清除的保管库对象（由“Scheduled Purge Date”字段触发清除操作）将被永久删除。 不可恢复。

## <a name="other-resources"></a>其他资源

- 有关 Key Vault 软删除功能的概述，请参阅 [Azure Key Vault 软删除概述](key-vault-ovw-soft-delete.md)。
- 有关 Azure Key Vault 使用情况的综述，请参阅 [Azure Key Vault 入门](key-vault-get-started.md)。


<!--Update_Description: wording update-->