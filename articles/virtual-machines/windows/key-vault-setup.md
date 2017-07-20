---
title: "在 Azure Resource Manager 中为 Windows VM 设置 Key Vault | Azure"
description: "如何设置与 Azure Resource Manager 虚拟机搭配使用的密钥保管库。"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 01/24/2017
ms.date: 03/28/2017
ms.author: v-dazen
ms.openlocfilehash: 71f961c5a9c9eb78cc54d743e76df9afb9008eff
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>在 Azure Resource Manager 中为虚拟机设置密钥保管库

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

在 Azure Resource Manager 堆栈中，密码/证书被建模为密钥保管库资源提供程序所提供的资源。 若要了解有关 Key Vault 的详细信息，请参阅[什么是 Azure Key Vault？](../../key-vault/key-vault-whatis.md)

> [!NOTE]
> 1. 为了让密钥保管库能与 Azure Resource Manager 虚拟机搭配使用，必须将密钥保管库上的 **EnabledForDeployment** 属性设置为 true。 你可以在各种客户端中执行此操作。
> 2. 需要在与虚拟机相同的订阅和位置中创建 Key Vault。
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>使用 PowerShell 设置密钥保管库
若要使用 PowerShell 创建 Key Vault，请参阅 [Azure Key Vault 入门](../../key-vault/key-vault-get-started.md#vault)。

对于新的密钥保管库，可以使用此 PowerShell cmdlet：

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'China East' -EnabledForDeployment

对于现有的密钥保管库，可以使用此 PowerShell cmdlet：

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a>使用 CLI 设置密钥保管库
若要使用命令行接口 (CLI) 创建密钥保管库，请参阅[使用 CLI 管理密钥保管库](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault)。

使用 CLI 时，必须先创建密钥保管库，然后分配部署策略。 可以使用以下命令来执行此操作：

    azure keyvault set-policy ContosoKeyVault -enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>使用模板设置密钥保管库
使用模板时，必须将 Key Vault 资源的 `enabledForDeployment` 属性设置为 `true`。

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

有关使用模板创建密钥保管库时可以配置的其他选项，请参阅 [Create a key vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create/)（创建密钥保管库）。

>[!NOTE]
> 必须修改从 GitHub 存储库“azure-quickstart-templates”下载的模板，以适应 Azure 中国云环境。 例如，替换某些终结点（将“blob.core.windows.net”替换为“blob.core.chinacloudapi.cn”，将“cloudapp.azure.com”替换为“chinacloudapp.cn”）；更改某些不受支持的 VM 映像；更改某些不受支持的 VM 大小。
