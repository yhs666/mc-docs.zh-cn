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
ms.topic: hero-article
origin.date: 05/10/2017
ms.date: 06/11/2018
ms.author: v-junlch
ms.openlocfilehash: 503331ebc3689279b94e8e30ff48c39faa737ca5
ms.sourcegitcommit: 306fba1a7125ef6f0555781524afa8f535bea2a0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2018
ms.locfileid: "35253376"
---
# <a name="secure-your-key-vault"></a>保护密钥保管库
Azure Key Vault 是一种云服务，用于保护云应用程序的加密密钥和机密（例如证书、连接字符串、密码）。 因为此数据是敏感数据和业务关键数据，会希望保护对密钥保管库的访问，以便只有得到授权的应用程序和用户可以访问密钥保管库。 本文概述密钥保管库访问模型，介绍身份验证和授权，并举例说明如何保护对云应用程序密钥保管库的访问。

## <a name="overview"></a>概述
通过两个单独的接口控制对密钥保管库的访问：管理平面和数据平面。 对于这两个平面，在调用方（用户或应用程序）可以访问密钥保管库前，需要适当的身份验证和授权。 身份验证建立调用方的标识，授权确定允许调用方执行哪些操作。

管理平面和数据平面都使用 Azure Active Directory 进行身份验证。 对于授权，管理平面使用基于角色的访问控制 (RBAC)，而数据平面使用密钥保管库访问策略。

下面是相关主题的简要概述：

[使用 Azure Active Directory 进行身份验证](#authentication-using-azure-active-directory) - 此部分介绍调用方如何通过管理平面和数据平面使用 Azure Active Directory 进行身份验证，以便访问密钥保管库。 

[管理平面和数据平面](#management-plane-and-data-plane) - 管理平面和数据平面是用于访问密钥保管库的两个访问平面。 每个访问平面支持特定的操作。 此部分介绍访问终结点、支持的操作以及每个平面使用的访问控制方法。 

[管理平面访问控制](#management-plane-access-control) - 此部分介绍如何使用基于角色的访问控制来访问管理平面操作。

[数据平面访问控制](#data-plane-access-control) - 此部分介绍如何使用密钥保管库访问策略来控制数据平面访问。

[示例](#example) - 本示例介绍了如何设置密钥保管库的访问控制，以允许三个不同的团队（安全小组、开发人员/操作员，以及审核员）执行特定的任务来开发、管理和监视 Azure 中的应用程序。

## <a name="authentication-using-azure-active-directory"></a>使用 Azure Active Directory 进行身份验证
在 Azure 订阅中创建密钥保管库时，该密钥保管库自动与订阅的 Azure Active Directory 租户关联。 所有调用方（用户和应用程序）必须在此租户中注册，才能访问此密钥保管库。 应用程序或用户必须使用 Azure Active Directory 进行身份验证，才能访问密钥保管库。 这一点适用于管理平面访问和数据平面访问。 在这两种情况下，应用程序可通过两种方式访问密钥保管库：

- **用户+ 应用访问** - 通常适用于代表登录用户访问密钥保管库的应用程序。 Azure PowerShell 和 Azure 门户就是这种访问类型的例子。 向用户授予访问权限有两种方法：一种方法是向用户授予访问权限，使他们可以从任何应用程序访问密钥保管库，另一种方法是授予用户仅当使用特定的应用程序时访问密钥保管库的权限（称之为复合标识）。 
- 仅限应用访问 - 适用于运行守护程序服务、后台作业等的应用程序。向应用程序的标识授予访问密钥保管库的权限。

在这两种类型的应用程序中，应用程序均通过 Azure Active Directory 使用任一[支持的身份验证方法](../active-directory/develop/active-directory-authentication-scenarios.md)进行身份验证，并获取令牌。 使用的身份验证方法取决于应用程序类型。 然后，应用程序使用此令牌并将 REST API 请求发送到密钥保管库。 在管理平面访问模式中，请求会通过 Azure Resource Manager 终结点路由。 访问数据平面时，应用程序直接与密钥保管库终结点对话。 请参阅有关[整个身份验证流](../active-directory/develop/active-directory-protocols-oauth-code.md)的详细信息。 

应用程序请求其令牌的资源名称会有所不同，具体取决于应用程序访问的是管理平面还是数据平面。 因此，根据具体的 Azure 环境，资源名称是本部分稍后提供的表格中所述的管理平面或数据平面终结点。

使用单个机制在管理平面和数据平面上进行身份验证机制具有自身的优势：

- 组织可以集中控制对内部所有密钥保管库的访问
- 离职的用户会立即失去对组织中所有密钥保管库的访问权限
- 组织可以通过 Azure Active Directory 中的选项自定义身份验证（例如，启用多重身份验证以提高安全性）

## <a name="management-plane-and-data-plane"></a>管理平面和数据平面
Azure 密钥保管库是可以在 Azure Resource Manager 部署模型中使用的 Azure 服务。 在创建密钥保管库时，会获取一个虚拟容器，可以在其内部创建密钥、机密和证书等其他对象。 然后，可以使用管理平面和数据平面访问密钥保管库来执行特定的操作。 管理平面接口用于管理密钥保管库本身，例如，创建、删除、更新密钥保管库属性，以及设置数据平面的访问策略。 数据平面接口用于在密钥保管库中添加，以及删除、修改和使用其中存储的密钥、机密和证书。

可通过不同的终结点访问管理平面和数据平面接口（请参见表）。 表格中的第二列描述这些终结点在不同 Azure 环境中的 DNS 名称。 第三列描述了可在每个访问平面中执行的操作。 每个访问平面也具有自己的访问控制机制：对于管理平面访问控制，使用 Azure Resource Manager 基于角色的访问控制 (RBAC) 进行设置；而对于数据平面访问控制，则使用密钥保管库访问策略进行设置。

| 访问平面 | 访问终结点 | 操作 | 访问控制机制 |
| --- | --- | --- | --- |
| 管理平面 |**全球：**<br> management.azure.com:443<br><br> **Azure China：**<br> management.chinacloudapi.cn:443<br><br> **Azure US Government：**<br> management.usgovcloudapi.net:443<br><br> **Azure Germany：**<br> management.microsoftazure.de:443 |创建/读取/更新/删除密钥保管库 <br> 设置密钥保管库的访问策略<br>设置密钥保管库的标记 |Azure Resource Manager 基于角色的访问控制 (RBAC) |
| 数据平面 |**全球：**<br> &lt;vault-name&gt;.vault.chinacloudapi.cn:443<br><br> **Azure China：**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure US Government：**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Germany：**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |对于密钥：解密、加密、UnwrapKey、WrapKey、验证、签名、获取、列出、更新、创建、导入、删除、备份、还原<br><br> 机密：获取、列出、设置、删除 |密钥保管库访问策略 |

管理平面访问控制与数据平面访问控制相互独立。 例如，如果想要授权应用程序使用密钥保管库中的密钥，只需使用密钥保管库访问策略授予数据平面访问权限，而不需要向此应用程序授予管理平面访问权限。 相反，如果希望用户有权读取保管库属性和标记但无权访问密钥、机密或证书，则可以使用 RBAC 向此用户授予“读取”访问权限，而不需要授予数据平面访问权限。

## <a name="management-plane-access-control"></a>管理平面访问控制
管理平面由影响密钥保管库本身的操作构成。 例如，可以创建或删除密钥保管库。 可在订阅中获取保管库的列表。 可以检索密钥保管库属性（如 SKU、标记），并设置密钥保管库访问策略，该策略可控制可以访问密钥保管库中的密钥和机密的用户和应用程序。 管理平面访问控制使用 RBAC。 请在前一部分的表格中查看可通过管理平面执行的密钥保管库操作的完整列表。 

### <a name="role-based-access-control-rbac"></a>基于角色的访问控制 (RBAC)
每个 Azure 订阅都有 Azure Active Directory。 可为此目录中的用户、组和应用程序授予访问权限，以便在使用 Azure Resource Manager 部署模型的 Azure 订阅中管理资源。 此类型的访问控制称为基于角色的访问控制 (RBAC)。 若要管理此访问权限，可以使用 [Azure 门户](https://portal.azure.cn/)、[Azure CLI 工具](../cli-install-nodejs.md)、[PowerShell](/powershell-install-configure) 或 [Azure Resource Manager REST API](https://msdn.microsoft.com/library/azure/dn906885.aspx)。

使用 Azure Resource Manager 模型时，可以在资源组中创建密钥保管库，使用 Azure Active Directory 来控制对此密钥保管库的管理平面的访问。 例如，可以向用户或组授予管理特定资源组中的密钥保管库的功能。

可以通过分配相应的 RBAC 角色，向特定范围内的用户、组和应用程序授予访问权限。 例如，若要向用户授予管理密钥保管库的访问权限，需要将预定义的角色“密钥保管库参与者”分配给位于特定范围内的此用户。 在此情况下，该范围可以是订阅、资源组，或特定的密钥保管库。 在订阅级别分配的角色适用于该订阅中的所有资源组和资源。 在资源组级别分配的角色适用于该资源组中的所有资源。 为特定资源分配的角色仅适用于该资源。 有几个预定义的角色（请参阅 [RBAC：内置角色](../role-based-access-control/built-in-roles.md)），并且如果预定义的角色不满足你的需求，也可以定义你自己的角色。

> [!IMPORTANT]
> 请注意，如果用户具有密钥保管库管理平面的参与者权限 (RBAC)，则该用户可以通过设置密钥保管库访问策略（它控制对数据平面的访问）向自己授予数据平面的访问权限。 因此，建议严格控制具有密钥保管库“参与者”权限的人员，以确保只有获得授权的人员可以访问和管理密钥保管库、密钥、机密和证书。
> 
> 

## <a name="data-plane-access-control"></a>数据平面访问控制
密钥保管库数据平面由影响密钥保管库中的对象（例如密钥、机密和证书）的操作组成。  这包括创建、导入、更新、列出、备份和还原密钥等密钥操作，签名、验证、加密、解密、包装和解包等加密操作，以及设置密钥的标记和其他属性。 同样，还包括获取、设置、列出和删除等有关机密的操作。

可通过设置密钥保管库的访问策略来授予数据平面访问权限。 用户、组或应用程序必须具有密钥保管库的管理平面的参与者权限 (RBAC)，才能设置该密钥保管库的访问策略。 可以向用户、组或应用程序授予针对密钥保管库中的密钥或机密执行特定操作的访问权限。 密钥保管库支持最多 16 个密钥保管库访问策略条目。 创建一个 Azure Active Directory 安全组并将用户添加到该组，可向多个用户授予对密钥保管库的数据平面访问权限。

### <a name="key-vault-access-policies"></a>密钥保管库访问策略
密钥保管库访问策略单独授予对密钥、机密和证书的权限。 例如，可以向用户提供仅针对密钥的访问权限，但不提供针对机密的权限。 但是，对访问密钥、机密或证书的权限是在保管库级别分配的。 换而言之，密钥保管库访问策略不支持对象级权限。 可以使用 [Azure 门户](https://portal.azure.cn/)、[Azure CLI 工具](../cli-install-nodejs.md)、[PowerShell](/powershell-install-configure) 或[密钥保管库管理 REST API](https://msdn.microsoft.com/library/azure/mt620024.aspx) 设置密钥保管库的访问策略。

> [!IMPORTANT]
> 请注意，密钥保管库访问策略在保管库级别应用。 例如，授予某个用户创建和删除密钥的权限时，该用户可以针对该密钥保管库中的所有密钥执行这些操作。
> 
> 

## <a name="example"></a>示例
假设开发中的某个应用程序使用证书进行 SSL 访问，使用 Azure 存储来存储数据，使用 RSA 2048 位密钥执行签名操作。 再假设此应用程序正在 VM（或 VM 规模集）中运行。 在这种情况下，可以使用密钥保管库来存储所有应用程序机密，使用密钥保管库来存储应用程序在 Azure Active Directory 中进行身份验证所用的启动证书。

下面是密钥保管库中存储的所有密钥和机密的摘要。

- **SSL 证书** - 用于 SSL
- **存储密钥** - 用于获取对存储帐户的访问权限
- **RSA 2048 位密钥** - 用于签名操作
- **启动证书** - 用于在 Azure Active Directory 中进行身份验证，获取对密钥保管库的访问权限，以便提取存储密钥和使用 RSA 密钥进行签名。

现在，让我们与管理、部署和审核此应用程序的人员会面。 本示例使用了三个角色。

- 安全团队 - 他们通常是来自“首席安全官 (Chief Security Officer) 办公室”或其类似部门的 IT 人员，负责机密（如 SSL 证书、用于签名的 RSA 密钥、数据库的连接字符串、存储帐户密钥）的安全保护。
- 
            **开发人员/操作人员** - 开发此应用程序，并将其部署到 Azure 的人员。 通常，他们不是安全团队的成员，因此不应有权访问 SSL 证书、RSA 密钥等任何敏感数据，但他们部署的应用程序应有权访问这些数据。
- **审核人员** - 通常是独立于开发人员和普通 IT 人员的一组不同的人员。 他们的职责是评审证书、密钥等是否得到适当的使用和维护，确保遵从数据安全标准。 

在此应用程序的范围之外还有一个角色，它与这里所提到的内容有关，那就是订阅（或资源组）管理员。 订阅管理员设置安全团队的初始访问权限。 在本示例中，我们假设订阅管理员已向安全团队授予此应用程序所需的全部资源所在的资源组的访问权限。

现在，我们看看在此应用程序的上下文中，每个角色可执行哪些操作。

- **安全团队**
  - 创建密钥保管库
  - 启用密钥保管库日志记录
  - 添加密钥/机密
  - 为灾难恢复创建密钥备份
  - 设置密钥保管库访问策略，向用户和应用程序授予执行特定操作的权限
  - 定期滚动更新密钥/机密
- **开发人员/操作人员**
  - 从安全团队获取启动证书和 SSL 证书引用（指纹）、存储密钥（机密 URI）以及签名密钥（密钥 URI）
  - 开发和部署以编程方式访问密钥和机密的应用程序
- **审核人员**
  - 审查使用情况日志，确认密钥/机密是否得到适当的使用，以及是否遵从数据安全标准

现在，让我们看看每个角色（和应用程序）执行其分配的任务所需的密钥保管库访问权限。 

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

除了密钥保管库的权限外，所有三个角色还需要其他资源的访问权限。 例如，能够部署 VM（或 Web 应用等）。开发人员/操作员也需要这些资源类型的“参与者”访问权限。 审核员需要存储密钥保管库日志的存储帐户的读取权限。

由于本文的重点是保护对密钥保管库的访问，因此我们将仅阐述与该主题相关的部分，并跳过有关部署证书、以编程方式访问密钥和机密等的详细信息。这些详细信息已涵盖在其他部分中。 [博客文章](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)中介绍了如何将存储在密钥保管库中的证书部署到 VM，并且提供了可用的[示例代码](https://www.microsoft.com/download/details.aspx?id=45343)，演示如何使用启动证书向 Azure AD 进行身份验证，以获取密钥保管库的访问权限。

可以使用 Azure 门户授予大多数访问权限，但若要授予粒度权限，则可能需要使用 Azure PowerShell（或 Azure CLI）来实现所期望的结果。 

以下 PowerShell 代码片段假设：

- Azure Active Directory 管理员创建了代表三个角色（即 Contoso 安全团队，Contoso 应用开发人员、Contoso 应用审核人员）的安全组。 管理员也已经向他们所属的组添加了用户。
- **ContosoAppRG** 是所有资源所在的资源组。 **contosologstorage** 是日志的存储位置。 
- 密钥保管库 ContosoKeyVault 和用于密钥保管库日志 contosologstorage 的存储帐户必须位于相同的 Azure 位置

首先，订阅管理员向安全小组分配“密钥保管库参与者”和“用户访问管理员”角色。 这样，安全团队便可以管理对其他资源的访问权限，以及管理资源组 ContosoAppRG 中的密钥保管库。

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

以下脚本演示安全团队如何创建密钥保管库、设置日志记录，以及设置对其他角色和应用程序的访问权限。 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'chinanorth' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets all

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

具有“部署/操作”权限的开发人员/操作员的自定义角色分配的范围限定为资源组。 这样，只有在资源组 "ContosoAppRG" 中创建的 VM 才能获取机密（SSL 证书和启动证书）。 开发/操作团队成员在其他资源组中创建的任何 VM 都无法获取这些机密，即使它们知道机密 URI。

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
- [使用 REST API 管理基于角色的访问控制](../role-based-access-control/role-assignments-rest.md)
  
  此文说明如何使用 REST API 来管理 RBAC。
- [Role-Based Access Control for Azure from Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)（Ignite 中提供的适用于 Azure 的基于角色的访问控制）
  
  这是第 9 频道中提供的 2015 MS Ignite 会议视频链接。 此次研讨会讨论了 Azure 中的访问管理和报告功能，并探索使用 Azure Active Directory 安全访问 Azure 订阅的最佳实践。
- [使用 OAuth 2.0 和 Azure Active Directory 来授权访问 Web 应用程序](../active-directory/develop/active-directory-protocols-oauth-code.md)
  
  此文介绍使用 Azure Active Directory 进行身份验证时遵循的整个 OAuth 2.0 流程。
- [key vault Management REST APIs（密钥保管库管理 REST API）](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  此参考文档介绍用于以编程方式管理密钥保管库的 REST API，内容包括如何设置密钥保管库访问策略。
- [key vault REST APIs（密钥保管库 REST API）](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  密钥保管库 REST API 参考文档的链接。
- [Key access control（密钥访问控制）](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  机密访问控制参考文档的链接。
- [Secret access control（机密访问控制）](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  密钥访问控制参考文档的链接。
- 使用 PowerShell [设置](https://msdn.microsoft.com/library/mt603625.aspx)和[删除](https://msdn.microsoft.com/library/mt619427.aspx)密钥保管库访问策略
  
  有关用于管理密钥保管库访问策略的 PowerShell cmdlet 的参考文档链接。

## <a name="next-steps"></a>后续步骤
有关面向管理员的入门教程，请参阅 [Azure Key Vault 入门](key-vault-get-started.md)。

有关将密钥和机密与 Azure 密钥保管库配合使用的详细信息，请参阅[关于密钥和机密](https://msdn.microsoft.com/library/azure/dn903623.aspx)。

如果在密钥保管库方面有任何问题，请访问 [Azure 密钥保管库论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)


<!-- Update_Description: link update -->