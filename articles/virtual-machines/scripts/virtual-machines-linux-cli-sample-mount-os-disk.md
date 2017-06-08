---
title: "Azure CLI 脚本示例 - 装载操作系统磁盘 | Azure"
description: "Azure CLI 脚本示例 - 装载操作系统磁盘"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
wacn.date: 
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: e0e6e13098e42358a7eaf3a810930af750e724dd
ms.openlocfilehash: e523aa8ef8d4b57eac79f4f3a12bfd69191ad4a4
ms.contentlocale: zh-cn
ms.lasthandoff: 04/06/2017

---

# <a name="troubleshoot-a-vms-operating-system-disk"></a>对 VM 操作系统磁盘进行故障排除

此脚本将失败或有问题的虚拟机的操作系统磁盘作为数据磁盘装载到第二个虚拟机。 排查磁盘问题或恢复数据时，此脚本会很有用。 

必要时，请使用 [Azure CLI 安装指南](https://docs.microsoft.com/cli/azure/install-azure-cli)中的说明安装 Azure CLI，然后运行 `az login` 创建与 Azure 的连接。 此外，还需一台现有的虚拟机。 更新此脚本示例中现有虚拟机的名称和资源组。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

此示例在 Bash shell 中正常工作。 有关在 Windows 客户端上运行 Azure CLI 脚本的选项，请参阅[在 Windows 中运行 Azure CLI](../virtual-machines-windows-cli-options.md)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Source virtual machine details.
sourcevm=<Replace with vm name>
resourcegroup=<Replace with resource group name>

# Get the disk id for the source VM operating system disk.
diskid="$(az vm show -g $resourcegroup -n $sourcevm --query [storageProfile.osDisk.managedDisk.id] -o tsv)"

# Delete the source virtual machine, this will not delete the disk.
az vm delete -g $resourcegroup -n $sourcevm --force

# Create a new virtual machine, this creates SSH keys if not present.
az vm create --resource-group $resourcegroup --name myVM --image UbuntuLTS --generate-ssh-keys

# Attach disk as a data disk to the newly created VM.
az vm disk attach --resource-group $resourcegroup --vm-name myVM --disk $diskid

# Configure disk on new VM.
ip=$(az vm list-ip-addresses --resource-group $resourcegroup --name myVM --query '[].virtualMachine.network.publicIpAddresses[0].ipAddress' -o tsv)
ssh $ip 'sudo mkdir /mnt/remountedOsDisk'
ssh $ip 'sudo mount -t ext4 /dev/sdc1 /mnt/remountedOsDisk'
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟机和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az vm show](https://docs.microsoft.com/cli/azure/vm#show) | 返回虚拟机列表。 在此示例中，查询选项用于返回虚拟机操作系统磁盘。 然后，将此值添加到名为“uri”的变量。 |
| [az vm delete](https://docs.microsoft.com/cli/azure/vm#delete) | 删除虚拟机。 |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#create) | 创建虚拟机。  |
| [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#attach) | 将磁盘附加到虚拟机。 |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | 返回虚拟机的 IP 地址。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure/overview)。

可以在 [Azure Linux VM 文档](../virtual-machines-linux-cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。
