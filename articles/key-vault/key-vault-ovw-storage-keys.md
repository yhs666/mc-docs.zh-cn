---
ms.assetid: ''
title: Azure Key Vault 存储帐户密钥
description: 存储帐户密钥在 Azure Key Vault 与 Azure 存储帐户基于密钥的访问方式之间提供无缝集成。
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: bryanla
ms.author: v-biyu
manager: mbaldwin
origin.date: 10/12/2017
ms.date: 11/05/2018
ms.openlocfilehash: 96ed27bbf1fb02081b257a7185e0d4e2f032d4d8
ms.sourcegitcommit: 8a68d9275ddb92ea45601fed96e21559999d9579
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026957"
---
# <a name="azure-key-vault-storage-account-keys"></a>Azure Key Vault 存储帐户密钥

- Azure Key Vault 管理 Azure 存储帐户 (ASA) 的密钥。
    - 在内部，Azure Key Vault 可以使用 Azure 存储帐户列出（同步）密钥。
    - Azure Key Vault 定期重新生成（轮换）密钥。
    - 响应调用方时永远不会返回密钥值。
    - Azure Key Vault 管理存储帐户和经典存储帐户的密钥。

<a name="prerequisites"></a>先决条件
--------------
1. [Azure CLI](/cli/install-azure-cli) 安装 Azure CLI   
2. [创建存储帐户](https://www.azure.cn/en-us/home/features/storage/)
    - 请遵循[此文档](https://docs.azure.cn/en-us/storage/)中的步骤创建存储帐户   
    - **命名指导：** 存储帐户名称必须为 3 到 24 个字符，并且只能包含数字和小写字母。        
      
<a name="step-by-step-instructions"></a>分步说明
-------------------------

1. 获取要管理的 Azure 存储帐户的资源 ID。
    a. 创建存储帐户后，请运行以下命令来获取要管理的存储帐户的资源 ID
    ```
    az storage account show -n storageaccountname (Copy ID out of the result of this command)
    ```
2. 获取 Azure Key Vault 服务主体的应用程序 ID 
    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
3. 将“存储密钥操作员”角色分配给 Azure Key Vault 标识
    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id hhjkh --scope idofthestorageaccount
    ```
4. 创建 Key Vault 托管存储帐户。     <br /><br />
   以下命令要求 Key Vault 根据重新生成周期定期重新生成存储的访问密钥。 下面，我们将重新生成周期设置为 90 天。 90 天后，Key Vault 将重新生成“key1”，并将活动密钥从“key2”交换为“key1”。
   ### <a name="key-regeneration"></a>重新生成密钥
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key2 --auto-generate-key --regeneration-period P90D --resource-id <Resource-id-of-storage-account>
    ```
    如果用户未创建存储帐户并且对存储帐户没有权限，则以下步骤会设置帐户的权限，以确保你可以管理 Key Vault 中的所有存储权限。
    [!NOTE] 如果用户对存储帐户没有权限，则我们会先获取该用户的对象 ID

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover purge restore set setsas update
    ```

### <a name="relevant-powershell-cmdlets"></a>相关的 Powershell cmdlet

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="see-also"></a>另请参阅

- [关于键、密钥和证书](https://docs.microsoft.com/rest/api/keyvault/)
- [Key Vault 团队博客](https://blogs.technet.microsoft.com/kv/)
