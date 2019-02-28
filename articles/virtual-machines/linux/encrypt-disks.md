---
title: 加密 Azure 中 Linux VM 上的磁盘 | Azure
description: 如何使用 Azure CLI 加密 Linux VM 上的虚拟磁盘以增强安全性
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 10/30/2018
ms.date: 02/18/2019
ms.author: v-yeche
ms.openlocfilehash: 8494bead361d350a26224d1b3df69c5c8b24ad94
ms.sourcegitcommit: dd6cee8483c02c18fd46417d5d3bcc2cfdaf7db4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56666393"
---
# <a name="how-to-encrypt-a-linux-virtual-machine-in-azure"></a>如何加密 Azure 中的 Linux 虚拟机

为了增强虚拟机 (VM) 的安全性以及符合性，可以加密虚拟磁盘和 VM 本身。 VM 是使用 Azure 密钥保管库中受保护的加密密钥加密的。 可以控制这些加密密钥，以及审核对它们的使用。 本文介绍如何使用 Azure CLI 加密 Linux VM 上的虚拟磁盘。 

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.30 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="overview-of-disk-encryption"></a>磁盘加密概述
Linux VM 上的虚拟磁盘是使用 [dm-crypt](https://wikipedia.org/wiki/Dm-crypt) 静态加密的。 加密 Azure 中的虚拟磁盘不会产生费用。 可以使用软件保护将加密密钥存储在 Azure Key Vault 中。可以控制这些加密密钥，以及审核对它们的使用。 这些加密密钥用于加密和解密附加到 VM 的虚拟磁盘。 

<!--Not Available on or you can import or generate your keys in Hardware Security Modules (HSMs) certified to FIPS 140-2 level 2 standards.-->
<!--Not Available on 或者，可在已通过 FIPS 140-2 级别 2 标准认证的硬件安全模块 (HSM) 中导入或生成密钥。-->

加密 VM 的过程如下：

1. 在 Azure 密钥保管库中创建加密密钥。
1. 配置可用于加密磁盘的加密密钥。
1. 为虚拟磁盘启用磁盘加密。
1. 从 Azure Key Vault 请求所需的加密密钥。
1. 使用提供的加密密钥加密虚拟磁盘。

## <a name="requirements-and-limitations"></a>要求和限制
磁盘加密支持的方案和要求

* 以下 Linux 服务器 SKU - Ubuntu、CentOS、SUSE、SUSE Linux Enterprise Server (SLES)。

<!-- Not Avaiable on  Red Hat Enterprise Linux -->

* 所有资源（例如密钥保管库、存储帐户和 VM）必须在同一个 Azure 区域和订阅中。
* 标准 A、D 和 DS 系列 VM。
* 在已加密的 Linux VM 上更新加密密钥。

<!-- Not Available on G and GS series -->

以下方案目前不支持磁盘加密：

* 基本层 VM。
* 使用经典部署模型创建的 VM。
* 在 Linux VM 上禁用 OS 磁盘加密。
* 使用自定义 Linux 映像。

有关支持的方案和限制的详细信息，请参阅[适用于 IaaS VM 的 Azure 磁盘加密](../../security/azure-security-disk-encryption.md)

## <a name="create-an-azure-key-vault-and-keys"></a>创建 Azure Key Vault 和密钥
在以下示例中，请将示例参数名称替换成自己的值。 示例参数名称包括 *myResourceGroup*、*myKey* 和 *myVM*。

第一步是创建用于存储加密密钥的 Azure 密钥保管库。 Azure 密钥保管库可以存储能够在应用程序和服务中安全实现的密钥、机密或密码。 对于虚拟磁盘加密，可以使用密钥保管库来存储用于加密或解密虚拟磁盘的加密密钥。

在 Azure 订阅中使用 [az provider register](https://docs.azure.cn/zh-cn/cli/provider?view=azure-cli-latest#az-provider-register) 启用 Azure Key Vault 提供程序，并使用 [az group create](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-create) 创建一个资源组。 以下示例在 `chinaeast` 位置创建名为 myResourceGroup 的资源组：

```azurecli
az provider register -n Microsoft.KeyVault
resourcegroup="myResourceGroup"
az group create --name $resourcegroup --location chinaeast
```

包含加密密钥和关联的计算资源（例如存储和 VM 本身）的 Azure 密钥保管库必须位于同一区域。 使用 [az keyvault create](https://docs.azure.cn/zh-cn/cli/keyvault?view=azure-cli-latest#az-keyvault-create) 创建 Azure Key Vault，并启用 Key Vaul，将其用于磁盘加密。 指定 *keyvault_name* 的唯一 Key Vault 名称，如下所示：

```azurecli
keyvault_name=myvaultname$RANDOM
az keyvault create \
    --name $keyvault_name \
    --resource-group $resourcegroup \
    --location chinaeast \
    --enabled-for-disk-encryption True
```

可以使用软件保护来存储加密密钥。  由于创建的是标准密钥保管库，以下示例使用了受软件保护的密钥。

<!--Not Available on or Hardware Security Model (HSM) protection 或硬件安全模型 (HSM) 保护-->
<!--Not Available on Using an HSM requires a premium Key Vault.-->
<!--Not Available on There is an additional cost to creating a premium Key Vault rather than standard Key Vault that stores software-protected keys. To create a premium Key Vault, in the preceding step add `--sku Premium` to the command.-->
<!--Not Available on 高级密钥保管库-->

对于这种保护模型，在启动 VM 以解密虚拟磁盘时，需要向 Azure 平台授予访问权限，使之能够请求加密密钥。 使用 [az keyvault key create](https://docs.azure.cn/zh-cn/cli/keyvault/key?view=azure-cli-latest#az-keyvault-key-create) 在 Key Vault 中创建加密密钥。 以下示例创建名为 *myKey* 的密钥：

<!--Not Available on 两种， 都-->

```azurecli
az keyvault key create \
    --vault-name $keyvault_name \
    --name myKey \
    --protection software
```

## <a name="create-a-virtual-machine"></a>创建虚拟机
使用 [az vm create](https://docs.azure.cn/zh-cn/cli/vm?view=azure-cli-latest#az-vm-create) 创建 VM 并附加一个 10Gb 数据磁盘。 只有特定市场映像才支持磁盘加密。 以下示例使用 Ubuntu 16.04 LTS 映像创建一个名为 myVM 的 VM：

```azurecli
az vm create \
    --resource-group $resourcegroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 10
```

使用上一命令输出中显示的 *publicIpAddress* 通过 SSH 连接到 VM。 创建分区和文件系统，并安装数据磁盘。 有关详细信息，请参阅[连接 Linux VM 以安装新磁盘](add-disk.md?toc=%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk)。 关闭 SSH 会话。

## <a name="encrypt-the-virtual-machine"></a>加密虚拟机

使用 [az vm encryption enable](https://docs.azure.cn/zh-cn/cli/vm/encryption?view=azure-cli-latest#az-vm-encryption-enable) 加密 VM。 下面的示例使用之前的 [az ad sp create-for-rbac](https://docs.azure.cn/zh-cn/cli/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) 命令中的 *$sp_id* 和 *$sp_password* 变量：

<!--NOTICE: MOONCAKE CUSTOMIZED-->

```azurecli
az ad sp create-for-rbac --query [appId,password] -o tsv
```

在命令成功执行以后，输出会显示 appId 和密码，请将 <sp_id> 和 <sp_password> 替换为输出值。 

```azurecli
az keyvault set-policy --name $keyvault_name `
     --spn <sp_id> --key-permissions wrapKey `
     --secret-permissions set
```

```azurecli
az vm encryption enable \
    --resource-group $resourcegroup \
    --name myVM \
    --aad-client-id <sp_id> \
    --aad-client-secret <sp_password> \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

<!--NOTICE: MOONCAKE CUSTOMIZED-->

需要一段时间才能完成磁盘加密过程。 使用 [az vm encryption show](https://docs.azure.cn/zh-cn/cli/vm/encryption?view=azure-cli-latest#az-vm-encryption-show) 监视过程状态：

```azurecli
az vm encryption show --resource-group $resourcegroup --name myVM --query 'status'
```

完成后，输出将类似于以下示例：

```json
[
  {
    "code": "ProvisioningState/succeeded",
    "displayStatus": "Provisioning succeeded",
    "level": "Info",
    "message": "Encryption succeeded for all volumes",
    "time": null
  }
]
```

## <a name="add-additional-data-disks"></a>添加更多数据磁盘
加密数据磁盘后，可以向 VM 添加更多虚拟磁盘并对其进行加密。 

将数据磁盘添加到 VM 后，重新运行该命令以加密虚拟磁盘，如下所示：

<!--ONLY CUSTOMZIED ON MOONCAKE-->

> [!NOTE]
> 在运行命令 [az ad sp create-for-rbac](https://docs.azure.cn/zh-cn/cli/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) 之前，应将 <sp_id> 和 <sp_password> 替换为上一步中的输出值。

```azurecli
az vm encryption enable \
    --resource-group $resourcegroup \
    --name myVM \
    --aad-client-id <sp_id> \
    --aad-client-secret <sp_password> \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

<!--ONLY CUSTOMZIED ON MOONCAKE-->

## <a name="next-steps"></a>后续步骤
* 有关管理 Azure 密钥保管库的详细信息，包括删除加密密钥和保管库，请参阅 [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md)（使用 CLI 管理密钥保管库）。
* 有关磁盘加密的详细信息，例如准备要上传到 Azure 的已加密自定义 VM，请参阅 [Azure Disk Encryption](../../security/azure-security-disk-encryption.md)（Azure 磁盘加密）。

<!--Update_Description: wording update, update meta properties -->