---
title: 保护密钥保管库 | Microsoft Docs
description: 管理用于管理保管库、密钥和机密的密钥保管库的访问权限。 密钥保管库的身份验证和授权模型，以及如何保护密钥保管库
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 05/10/2017
ms.date: 11/05/2018
ms.author: v-biyu
ms.openlocfilehash: 756260b701246082847894fb8d293c45e89e64fb
ms.sourcegitcommit: 8a68d9275ddb92ea45601fed96e21559999d9579
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026952"
---
# <a name="secure-your-key-vault"></a>保护密钥保管库
Azure Key Vault 是用于保护加密密钥和机密（例如证书、连接字符串、密码）的云服务。 由于这些数据极其机密且对业务至关重要，因此必须保护对 Key Vault 的访问，以便只有经过授权的应用程序和用户才能获取访问权限。 

本文提供 Key Vault 访问模型的概述。 其中介绍了身份验证和授权，以及如何保护对 Key Vault 的访问。

## <a name="overview"></a>概述
可通过以下两个独立接口来控制对密钥保管库的访问：管理平面和数据平面。 对于这两个平面，在调用方（用户或应用程序）可以访问 Key Vault 前，需要适当的身份验证和授权。 身份验证建立调用方的标识，授权确定允许调用方执行的操作。

管理平面和数据平面都使用 Azure Active Directory 进行身份验证。 对于授权，管理平面使用基于角色的访问控制 (RBAC)，而数据平面使用密钥保管库访问策略。

下面是相关主题的简要概述：

[使用 Azure Active Directory 进行身份验证](#authentication-using-azure-active-directory) - 此部分介绍调用方如何通过管理平面和数据平面使用 Azure Active Directory 进行身份验证，以便访问密钥保管库。 

[管理平面和数据平面](#management-plane-and-data-plane) - 管理平面和数据平面是用于访问密钥保管库的两个访问平面。 每个访问平面支持特定的操作。 此部分介绍访问终结点、支持的操作以及每个平面使用的访问控制方法。 

[管理平面访问控制](#management-plane-access-control) - 此部分介绍如何使用基于角色的访问控制来访问管理平面操作。

[数据平面访问控制](#data-plane-access-control) - 此部分介绍如何使用密钥保管库访问策略来控制数据平面访问。

[示例](#example) - 本示例说明如何设置 Key Vault 的访问控制，以允许三个不同的团队（安全小组、开发人员/操作员，以及审核员）执行特定的任务来开发、管理和监视 Azure 中的应用程序。

## <a name="authentication-using-azure-active-directory"></a>使用 Azure Active Directory 进行身份验证
在 Azure 订阅中创建 Key Vault 时，该 Key Vault 会自动关联到该订阅的 Azure Active Directory 租户。 所有调用方（用户和应用程序）必须在此租户中注册，并且必须执行身份验证才能访问 Key Vault。 此项要求适用于管理平面访问和数据平面访问。 在这两种情况下，应用程序可通过两种方式访问密钥保管库：

- **用户+ 应用访问** - 通常适用于代表登录用户访问密钥保管库的应用程序。 Azure PowerShell 和 Azure 门户就是这种访问类型的例子。 向用户授予访问权限有两种方法：一种方法是向用户授予访问权限，使他们可以从任何应用程序访问密钥保管库，另一种方法是授予用户仅当使用特定的应用程序时访问密钥保管库的权限（称之为复合标识）。 
* **仅限应用的访问** - 适用于作为守护程序服务或后台作业运行的应用程序。 向应用程序的标识授予访问密钥保管库的权限。

在这两种类型的应用程序中，应用程序均通过 Azure Active Directory 使用任一[支持的身份验证方法](../active-directory/develop/active-directory-authentication-scenarios.md)进行身份验证，并获取令牌。 使用的身份验证方法取决于应用程序类型。 然后，应用程序使用此令牌并将 REST API 请求发送到密钥保管库。 在管理平面访问模式中，请求会通过 Azure Resource Manager 终结点路由。 访问数据平面时，应用程序直接与密钥保管库终结点对话。 请参阅有关[整个身份验证流](../active-directory/develop/active-directory-protocols-oauth-code.md)的详细信息。 

应用程序请求其令牌的资源名称会有所不同，具体取决于应用程序访问的是管理平面还是数据平面。 因此，根据具体的 Azure 环境，资源名称是本部分稍后提供的表格中所述的管理平面或数据平面终结点。

使用单个机制在管理平面和数据平面上进行身份验证机制具有自身的优势：

- 组织可以集中控制对内部所有密钥保管库的访问
- 离职的用户会立即失去对组织中所有密钥保管库的访问权限
- 组织可以通过 Azure Active Directory 中的选项自定义身份验证（例如，启用多重身份验证以提高安全性）

## <a name="management-plane-and-data-plane"></a>管理平面和数据平面
Azure Key Vault 是可以在 Azure 资源管理器部署模型中使用的 Azure 服务。 当你创建 Key Vault 时，将获取一个用于存储敏感对象（例如密钥、机密和证书）的虚拟容器。 可以使用管理平面和数据平面接口对 Key Vault 执行特定的操作。 管理平面用于管理 Key Vault 本身。 这包括管理属性以及设置数据平面访问策略等操作。 数据平面接口用于在 Key Vault 中添加、删除、修改和使用其中存储的密钥、机密和证书。

可通过下面所列的不同终结点访问管理平面和数据平面接口。 第二列描述这些终结点在不同 Azure 环境中的 DNS 名称。 第三列描述可在每个访问平面中执行的操作。 每个访问平面还有自身的访问控制机制。 使用 Azure 资源管理器基于角色的访问控制 (RBAC) 来设置管理平面访问控制。 使用 Key Vault 访问策略来设置数据平面访问控制。

| 访问平面 | 访问终结点 | 操作 | 访问控制机制 |
| --- | --- | --- | --- |
| 管理平面 |**全球：**<br> management.azure.com:443<br><br> **Azure 中国世纪互联：**<br> management.chinacloudapi.cn:443<br><br> **Azure US Government：**<br> management.usgovcloudapi.net:443<br><br> **Azure Germany：**<br> management.microsoftazure.de:443 |创建/读取/更新/删除密钥保管库 <br> 设置密钥保管库的访问策略<br>设置密钥保管库的标记 |Azure Resource Manager 基于角色的访问控制 (RBAC) |
| 数据平面 |**全球：**<br> &lt;vault-name&gt;.vault.chinacloudapi.cn:443<br><br> **Azure 中国世纪互联：**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure US Government：**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Germany：**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |对于密钥：解密、加密、UnwrapKey、WrapKey、验证、签名、获取、列出、更新、创建、导入、删除、备份、还原<br><br> 机密：获取、列出、设置、删除 |密钥保管库访问策略 |

管理平面访问控制与数据平面访问控制相互独立。 例如，若要授权应用程序使用 Key Vault 中的密钥，只需授予数据平面访问权限。 通过 Key Vault 访问策略授予访问权限。 相反，需要读取保管库属性和标记、但无需访问数据（密钥、机密或证书）的用户只需要控制平面访问权限。 通过使用 RBAC 将“读取”访问权限分配给用户来授予访问权限。

## <a name="management-plane-access-control"></a>管理平面访问控制
管理平面由影响 Key Vault 本身的操作构成，例如：

- 创建或删除 Key Vault。
- 获取订阅中保管库的列表。
- 检索 Key Vault 属性（例如 SKU、标记）。
- 设置 Key Vault 访问策略，用于控制用户和应用程序对密钥和机密的访问。

管理平面访问控制使用 RBAC。 请在前一部分的表格中查看可通过管理平面执行的 Key Vault 操作的完整列表。 

### <a name="role-based-access-control-rbac"></a>基于角色的访问控制 (RBAC)
每个 Azure 订阅都有 Azure Active Directory。 可为此目录中的用户、组和应用程序授予访问权限，以便在使用 Azure Resource Manager 部署模型的 Azure 订阅中管理资源。 此类型的访问控制称为基于角色的访问控制 (RBAC)。 若要管理此访问权限，可以使用 [Azure 门户](https://portal.azure.cn/)、[Azure CLI 工具](../cli-install-nodejs.md)、[PowerShell](https://docs.microsoft.com/zh-cn/powershell/azure/overview?view=azurermps-6.10.0) 或 [Azure Resource Manager REST API](https://msdn.microsoft.com/library/azure/dn906885.aspx)。

使用 Azure Active Directory 在资源组中创建 Key Vault，并控制对管理平面的访问。 例如，可以授予用户或组管理资源组中 Key Vault 的权限。

可以通过分配相应的 RBAC 角色，向特定范围内的用户、组和应用程序授予访问权限。 例如，若要向用户授予管理 Key Vault 的访问权限，请将预定义的角色“Key Vault 参与者”角色分配给位于特定范围内的此用户。 在此情况下，该范围可以是订阅、资源组，或特定的 Key Vault。 在订阅级别分配的角色适用于该订阅中的所有资源组和资源。 在资源组级别分配的角色适用于该资源组中的所有资源。 为特定资源分配的角色适用于该资源。 有几个预定义的角色（请参阅 [RBAC：内置角色](../role-based-access-control/built-in-roles.md)）。 如果预定义角色不符合需要，你可以定义自己的角色。

> [!IMPORTANT]
> 请注意，如果用户具有密钥保管库管理平面的参与者权限 (RBAC)，则该用户可以通过设置密钥保管库访问策略（它控制对数据平面的访问）向自己授予数据平面的访问权限。 因此，建议严格控制具有密钥保管库“参与者”权限的人员，以确保只有获得授权的人员可以访问和管理密钥保管库、密钥、机密和证书。
> 
> 

## <a name="data-plane-access-control"></a>数据平面访问控制
Key Vault 数据平面操作适用于密钥、机密和证书等存储对象。 密钥操作包括创建、导入、更新、列出、备份和还原密钥。 加密操作包括签名、验证、加密、解密、包装、解包、设置标记及其他密钥属性。 同样，针对机密的操作包括获取、设置、列出和删除。

可通过设置密钥保管库的访问策略来授予数据平面访问权限。 用户、组或应用程序必须具有密钥保管库的管理平面的参与者权限 (RBAC)，才能设置该密钥保管库的访问策略。 可以向用户、组或应用程序授予针对密钥保管库中的密钥或机密执行特定操作的访问权限。 密钥保管库最多支持 1024 个密钥保管库访问策略条目。 创建一个 Azure Active Directory 安全组并将用户添加到该组，可向多个用户授予对密钥保管库的数据平面访问权限。

### <a name="key-vault-access-policies"></a>密钥保管库访问策略
密钥保管库访问策略单独授予对密钥、机密和证书的权限。 例如，可以向用户授予仅限密钥的访问权限，而不授予对机密的权限。 对访问密钥、机密或证书的权限是在保管库级别分配的。 Key Vault 访问策略不支持粒度对象级权限，例如特定的密钥/机密/证书。 可以使用 [Azure 门户](https://portal.azure.cn/)、[Azure CLI 工具](../cli-install-nodejs.md)、[PowerShell](https://docs.microsoft.com/zh-cn/powershell/azure/overview?view=azurermps-6.10.0) 或[密钥保管库管理 REST API](https://msdn.microsoft.com/library/azure/mt620024.aspx) 设置密钥保管库的访问策略。

> [!IMPORTANT]
> 请注意，密钥保管库访问策略在保管库级别应用。 例如，授予某个用户创建和删除密钥的权限时，该用户可以针对该密钥保管库中的所有密钥执行这些操作。
> 
> 

## <a name="example"></a>示例
假设开发中的某个应用程序使用证书进行 SSL 访问，使用 Azure 存储来存储数据，使用 RSA 2048 位密钥执行签名操作。 此外，假设此应用程序在 Azure 虚拟机（或虚拟机规模集）中运行。 在这种情况下，可以使用 Key Vault 来存储所有应用程序机密，并存储应用程序在 Azure Active Directory 中进行身份验证所用的启动证书。

下面是 Key Vault 中存储的密钥和机密类型的摘要：

- **SSL 证书** - 用于 SSL
- **存储密钥** - 用于获取对存储帐户的访问权限
- **RSA 2048 位密钥** - 用于签名操作
* **启动证书** - 用于在 Azure Active Directory 中进行身份验证。 获取访问权限后，可以提取存储密钥并使用 RSA 密钥进行签名。

现在，让我们与管理、部署和审核此应用程序的人员会面。 本示例使用了三个角色。

* **安全团队** - 通常是来自“CSO（首席安全官）办公室”或其类似部门的 IT 人员。 此团队负责机密的适当保管。 例如 SSL 证书、用于签名的 RSA 密钥、连接字符串和存储帐户密钥。
* **开发人员/操作人员** - 开发应用程序并将其部署到 Azure 的人员。 通常，他们不是安全团队的成员，因此不应有权访问 SSL 证书和 RSA 密钥等敏感数据。 他们部署的应用程序应有权访问这些数据。
* **审核人员** - 通常是独立于开发人员和普通 IT 人员的一组不同的人员。 他们的职责是评审证书、密钥和机密的使用和维护，确保遵从安全标准。 

在此应用程序的范围之外还有一个角色，它与本文所述的内容相关。 该角色是订阅（或资源组）管理员。 订阅管理员设置安全团队的初始访问权限。 订阅管理员使用包含此应用程序所需资源的资源组向安全团队授予访问权限。

现在，我们看看在此应用程序的上下文中，每个角色可执行哪些操作。

* **安全团队**
  * 创建密钥保管库
  * 启用密钥保管库日志记录
  * 添加密钥/机密
  * 为灾难恢复创建密钥备份
  * 设置密钥保管库访问策略，向用户和应用程序授予执行特定操作的权限
  * 定期滚动更新密钥/机密
* **开发人员/操作人员**
  * 从安全团队获取启动证书和 SSL 证书引用（指纹）、存储密钥（机密 URI）以及签名密钥（密钥 URI）
  * 开发和部署以编程方式访问密钥与机密的应用程序
* **审核人员**
  * 审查使用情况日志，确认密钥/机密是否得到适当的使用，以及是否遵从数据安全标准

现在，让我们看看每个角色和应用程序需要哪些访问权限才能执行其分配的任务。 

| 用户角色 | 管理平面权限 | 数据平面权限 |
| --- | --- | --- |
| 安全团队 |密钥保管库参与者 |密钥：备份、创建、删除、获取、导入、列出、还原 <br> 机密：所有 |
| 开发人员/操作人员 |需要密钥保管库部署权限，使部署的 VM 可以从密钥保管库中提取机密 |无 |
| 审核人员 |无 |密钥：列出<br>机密：列出 |
| 应用程序 |无 |密钥：签名<br>机密：获取 |

> [!NOTE]
> 审核员需要对密钥和机密拥有列出权限，以便能够检查日志中未发出的密钥和机密的属性，例如标记、激活日期和过期日期。
> 
> 

除了 Key Vault 权限以外，所有三个角色还需要有权访问其他资源。 例如，能够部署 VM（或 Web 应用等）。开发人员/操作员也需要这些资源类型的“参与者”访问权限。 审核人员需要对 Key Vault 日志所存储到的存储帐户拥有“读取”访问权限。

由于本文的重点是保护对 Key Vault 的访问，因此我们仅阐述与此主题相关的概念。 有关部署证书、以编程方式访问密钥和机密等的详细信息将在其他文章中介绍。 例如：

- 博客文章 [Deploy Certificates to VMs from customer-managed Key Vault](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)（将证书从客户管理的 Key Vault 部署到 VM）中介绍了如何将存储在 Key Vault 中的证书部署到 VM
- [Azure Key Vault 客户端示例下载内容](https://www.microsoft.com/download/details.aspx?id=45343)演示了如何使用启动证书对 Azure AD 进行身份验证，以访问 Key Vault。

可以使用 Azure 门户授予大多数访问权限。 若要授予粒度权限，可能需要使用 Azure PowerShell 或 Azure CLI 来实现所需的结果。 

以下 PowerShell 代码片段假设：

* Azure Active Directory 管理员创建了代表三个角色（Contoso 安全团队、Contoso 应用开发人员、Contoso 应用审核人员）的安全组。 管理员还将用户添加到了他们所属的组。
* **ContosoAppRG** 是所有资源所在的资源组。 **contosologstorage** 是日志的存储位置。 
* 密钥保管库 ContosoKeyVault 和用于密钥保管库日志 contosologstorage 的存储帐户必须位于相同的 Azure 位置

首先，订阅管理员向安全小组分配“密钥保管库参与者”和“用户访问管理员”角色。 这些角色使安全团队能够管理对其他资源的访问，以及管理资源组 ContosoAppRG 中的 Key Vault。

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

以下脚本演示安全团队如何创建 Key Vault 并设置日志记录和访问权限。 有关 Key Vault 访问策略权限的详细信息，请参阅[关于 Azure Key Vault 密钥、机密和证书](about-keys-secrets-and-certificates.md)。

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'chinanorth' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $devopsrole

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

自定义的角色只能分配给创建 ContosoAppRG 资源组所在的订阅。 如果要为其他订阅中的其他项目使用相同的自定义角色，应该在角色范围中添加更多的订阅。

为拥有“部署/操作”权限的开发人员/操作人员分配的自定义角色会划归到资源组。 这样，只有在资源组“ContosoAppRG”中创建的 VM 才有权访问机密（SSL 证书和启动证书）。 开发/操作团队成员在其他资源组中创建的 VM 无权访问这些机密，即使它们拥有机密 URI。

本示例描绘了一个简单方案。 现实的方案可能更复杂，需要你根据需求调整对密钥保管库的权限。 例如，在本示例中，假设安全团队将提供开发人员/操作员团队需要用来在其应用程序中进行引用的密钥和机密引用（URI 和指纹）。 这样，它们便无需向开发人员/操作员授予任何数据平面访问权限。 此外，还请注意本示例重点介绍对密钥保管库的保护。 应对类似的情形予以考虑，以保护 [VM](https://www.azure.cn/home/features/virtual-machines/)、[存储帐户](../storage/common/storage-security-guide.md)和其他 Azure 资源。

> [!NOTE]
> 注意：本示例演示如何在生产环境中锁定密钥保管库访问权限。 开发人员应该获取自己的、拥有完全权限的订阅或资源组，以便能够管理用于开发应用程序的保管库、VM 和存储帐户。
> 
> 

## <a name="resources"></a>资源
- [Azure Active Directory 基于角色的访问控制](../role-based-access-control/role-assignments-portal.md)
  
  此文解释了 Azure Active Directory 基于角色的访问控制及其工作原理。
- [RBAC：内置角色](../role-based-access-control/built-in-roles.md)
  
  此文详细说明了 RBAC 中所有可用内置角色。
- [了解 Resource Manager 部署和经典部署](../azure-resource-manager/resource-manager-deployment-model.md)
  
  此文介绍了 Resource Manager 部署和经典部署模型，并说明使用 Resource Manager 和资源组的优点
- [使用 Azure PowerShell 管理基于角色的访问控制](../role-based-access-control/role-assignments-powershell.md)
  
  此文介绍如何使用 Azure PowerShell 管理基于角色的访问控制
- [Managing Role-Based Access Control with the REST API（使用 REST API 管理基于角色的访问控制）](../role-based-access-control/role-assignments-rest.md)
  
  此文说明如何使用 REST API 来管理 RBAC。
- 
  
  
- [使用 OAuth 2.0 和 Azure Active Directory 授权访问 Web 应用程序](../active-directory/develop/active-directory-protocols-oauth-code.md)
  
  此文介绍使用 Azure Active Directory 进行身份验证时遵循的整个 OAuth 2.0 流程。
  
- [key vault Management REST APIs（密钥保管库管理 REST API）](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  此参考文档介绍用于以编程方式管理密钥保管库的 REST API，内容包括如何设置密钥保管库访问策略。
- [key vault REST APIs（密钥保管库 REST API）](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  密钥保管库 REST API 参考文档的链接。
- [Key access control（密钥访问控制）](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  机密访问控制参考文档的链接。
- [Secret access control（机密访问控制）](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  密钥访问控制参考文档的链接。
* 使用 PowerShell [设置](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy)和[删除](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Remove-AzureRmKeyVaultAccessPolicy)密钥保管库访问策略
  
  有关用于管理密钥保管库访问策略的 PowerShell cmdlet 的参考文档链接。

## <a name="next-steps"></a>后续步骤
有关面向管理员的入门教程，请参阅 [Azure Key Vault 入门](key-vault-get-started.md)。

有关将密钥和机密与 Azure 密钥保管库配合使用的详细信息，请参阅 [关于密钥和机密](https://msdn.microsoft.com/library/azure/dn903623.aspx)。

如果在密钥保管库方面有任何问题，请访问 [Azure 密钥保管库论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)


<!-- Update_Description: link update -->