---
title: Azure CLI 脚本示例 - 使用 Azure Monitor 创建 Linux VM | Azure
description: Azure CLI 脚本示例 - 使用 Azure Monitor 创建 Linux VM
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 02/27/2017
ms.date: 05/15/2019
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 17935ef598ef659ceb6d5f5b97b1bdfded29cbd0
ms.sourcegitcommit: 0e83be63445bc68bcf7b9a7ea1cd9a42f3ed2b25
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2019
ms.locfileid: "67427827"
---
<!--Verify successfully-->
# <a name="monitor-a-vm-with-azure-monitor"></a>使用 Azure Monitor 监视 VM

此脚本创建一个 Azure 虚拟机，安装 Log Analytics 代理，并将系统注册到 Log Analytics 工作区。 运行脚本后，该虚拟机会显示在控制台中。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/sh

# OMS Id and OMS key.
omsid=<Replace with your OMS Id>
omskey=<Replace with your OMS key>

# Create a resource group.
az group create --name myResourceGroup --location chinanorth

# Create a new virtual machine, this creates SSH keys if not present. 
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys

# Install and configure the OMS agent.
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.0 --protected-settings '{"workspaceKey": "'"$omskey"'"}' \
  --settings '{"workspaceId": "'"$omsid"'"}'

```

## <a name="clean-up-deployment"></a>清理部署

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟机和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az group create](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az vm create](https://docs.azure.cn/zh-cn/cli/vm?view=azure-cli-latest#az-vm-create) | 创建虚拟机并将其连接到网卡、虚拟网络、子网和 NSG。 此命令还指定要使用的虚拟机映像和管理凭据。  |
| [azure vm extension set](https://docs.azure.cn/zh-cn/cli/vm/extension?view=azure-cli-latest) | 针对虚拟机运行 VM 扩展。 在此示例中，使用 Azure Monitor 代理扩展安装 Log Analytics 代理，并在 Log Analytics 工作区中注册 VM。 |
| [az group delete](https://docs.azure.cn/zh-cn/cli/vm/extension?view=azure-cli-latest#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/zh-cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Linux VM 文档](../linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。

<!--Update_Description: new articles of create vm OMS-->
<!--ms.date: 05/20/2019-->