---
title: "Azure CLI 脚本示例 - 使用 VHD 创建 VM | Azure"
description: "Azure CLI 脚本示例 - 使用虚拟硬盘创建 VM。"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 03/09/2017
ms.date: 04/17/2017
ms.author: v-dazen
ms.custom: mvc
ms.openlocfilehash: 8d0e7946f0a5b57567599afb52d9f8f6b8f4bf1a
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 使用虚拟硬盘创建 VM
<a id="create-a-vm-with-a-virtual-hard-disk" class="xliff"></a>

本示例使用 VHD 创建虚拟机。
本示例创建资源组、存储帐户、容器，然后通过将 VHD 上传到容器来创建 VM。
本示例将 ssh 公钥替换为用户的公钥，因此用户可以访问 VM。

用户需要可引导 VHD。
用户可以从 https://azclisamples.blob.core.windows.net/vhds/sample.vhd 下载我们所使用的 VHD，也可以使用自己的 VHD。 脚本会查找 `~/sample.vhd`。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## 示例脚本
<a id="sample-script" class="xliff"></a>

```azurecli
#!/bin/bash

# Create a resource group
az group create -n myResourceGroup -l chinanorth

# Create the storage account to upload the vhd
az storage account create -g myResourceGroup -n mystorageaccount -l chinanorth --sku PREMIUM_LRS

# Get a storage key for the storage account
STORAGE_KEY=$(az storage account keys list -g myResourceGroup -n mystorageaccount --query "[?keyName=='key1'] | [0].value" -o tsv)

# Create the container for the vhd
az storage container create -n vhds --account-name mystorageaccount --account-key ${STORAGE_KEY}

# Upload the vhd to a blob
az storage blob upload -c vhds -f ~/sample.vhd -n sample.vhd --account-name mystorageaccount --account-key ${STORAGE_KEY}

# Create the vm from the vhd
az vm create -g myResourceGroup -n myVM --image "https://myStorageAccount.blob.core.chinacloudapi.cn/vhds/sample.vhd" \
        --os-type linux --admin-username deploy --generate-ssh-keys

# Update the deploy user with your ssh key
az vm user update --resource-group myResourceGroup -n custom-vm -u deploy --ssh-key-value "$(< ~/.ssh/id_rsa.pub)"

# Get public IP address for the VM
IP_ADDRESS=$(az vm list-ip-addresses -g az-cli-vhd -n custom-vm \
    --query "[0].virtualMachine.network.publicIpAddresses[0].ipAddress" -o tsv)

echo "You can now connect using 'ssh deploy@${IP_ADDRESS}'"
```

## 清理部署
<a id="clean-up-deployment" class="xliff"></a> 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli 
az group delete -n az-cli-vhd
```

## 脚本说明
<a id="script-explanation" class="xliff"></a>

此脚本使用以下命令创建资源组、虚拟机、可用性集、负载均衡器和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | 创建用于存储所有资源的资源组。 |
| [az storage account list](https://docs.microsoft.com/cli/azure/storage/account#list) | 列出存储帐户 |
| [az storage account check-name](https://docs.microsoft.com/cli/azure/storage/account#check-name) | 检查存储帐户名称是否有效且目前还不存在 |
| [az storage account keys list](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | 列出存储帐户的密钥 |
| [az storage blob exists](https://docs.microsoft.com/cli/azure/storage/blob#exists) | 检查 Blob 是否存在 |
| [az storage container create](https://docs.microsoft.com/cli/azure/storage/container#create) | 在存储帐户中创建一个容器。 |
| [az storage blob upload](https://docs.microsoft.com/cli/azure/storage/blob#upload) | 通过上传 VHD，在容器中创建一个 Blob。 |
| [az vm list](https://docs.microsoft.com/cli/azure/vm#list) | 与 `--query` 一起使用，用于检查 VM 名称是否已使用。 | 
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | 创建虚拟机。 |
| [az vm access set-linux-user](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | 重置 SSH 密钥，以便当前用户能够访问 VM。 |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | 获取已创建虚拟机的 IP 地址。 |

## 后续步骤
<a id="next-steps" class="xliff"></a>

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure Linux VM 文档](../linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。
