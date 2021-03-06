---
title: 通过 Azure 门户配置客户管理的密钥用于 Azure 存储加密
description: 了解如何使用 Azure 门户来配置客户管理的密钥用于 Azure 存储加密。 使用客户管理的密钥可以创建、轮换、禁用和撤销访问控制。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 10/15/2019
ms.date: 10/28/2019
ms.author: v-jay
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 84546ebc3a8177566558d30a765a0de840d826a1
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72914436"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-the-azure-portal"></a>通过 Azure 门户配置客户管理的密钥用于 Azure 存储加密

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

本文介绍如何使用 [Azure 门户](https://portal.azure.cn/)配置包含客户管理的密钥的 Key Vault。 若要了解如何使用 Azure 门户创建 Key Vault，请参阅[快速入门：使用 Azure 门户在 Azure Key Vault 中设置和检索机密](../../key-vault/quick-create-portal.md)。 

> [!IMPORTANT]
> 使用带有 Azure 存储加密的客户管理密钥需要在密钥保管库上设置两个属性：“软删除”  和“不要清除”  。 默认情况下未启用这些属性。 若要启用这些属性，请使用 PowerShell 或 Azure CLI。
> 仅支持 RSA 密钥以及密钥大小 2048。

## <a name="enable-customer-managed-keys"></a>启用客户管理的密钥

若要在 Azure 门户中启用客户管理的密钥，请执行以下步骤：

1. 导航到存储帐户。
1. 在存储帐户的“设置”  边栏选项卡上，单击“加密”  。 选择“使用自己的密钥”  选项，如下图所示。

    ![显示加密选项的门户屏幕截图](./media/storage-encryption-keys-portal/ssecmk1.png)

## <a name="specify-a-key"></a>指定密钥

启用客户管理的密钥后，可以指定要与存储帐户关联的密钥。

### <a name="specify-a-key-as-a-uri"></a>将密钥指定为 URI

若要将某个密钥指定为 URI，请执行下列步骤：

1. 若要在 Azure 门户中查找密钥 URI，请导航到 Key Vault，然后选择“密钥”设置。  选择所需的密钥，然后单击该密钥以查看其设置。 复制“密钥标识符”字段的值（提供 URI）。 

    ![显示 Key Vault 密钥 URI 的屏幕截图](media/storage-encryption-keys-portal/key-uri-portal.png)

1. 在存储帐户的“加密”设置中，选择“输入密钥 URI”选项。  
1. 在“密钥 URI”  字段中，指定 URI。

   ![显示如何输入密钥 URI 的屏幕截图](./media/storage-encryption-keys-portal/ssecmk2.png)

### <a name="specify-a-key-from-a-key-vault"></a>从 Key Vault 指定密钥

若要指定 Key Vault 中的密钥，请先请确保有一个包含密钥的 Key Vault。 若要指定 Key Vault 中的密钥，请执行以下步骤：

1. 选择“从 Key Vault 中选择”  选项。
2. 选择包含要使用的密钥的 Key Vault。
3. 从 Key Vault 中选择密钥。

   ![显示客户管理的密钥选项的屏幕截图](./media/storage-encryption-keys-portal/ssecmk3.png)

## <a name="update-the-key-version"></a>更新密钥版本

创建密钥的新版本时，需将存储帐户更新为使用新版本。 执行以下步骤：

1. 导航到你的存储帐户，并显示“加密”设置。 
1. 指定新密钥版本的 URI。 或者，可以再次选择 Key Vault 和密钥以更新版本。

## <a name="next-steps"></a>后续步骤

- [静态数据的 Azure 存储加密](storage-service-encryption.md)
- [什么是 Azure Key Vault？](/key-vault/key-vault-overview)
