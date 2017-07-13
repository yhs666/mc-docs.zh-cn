---
title: "在 Azure 中扩展 Linux VM 上的虚拟硬盘 | Azure"
description: "了解如何使用 Azure CLI 2.0 在 Linux VM 上扩展虚拟硬盘"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 05/11/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.openlocfilehash: 587e5989612dc5c19c1079ba799e6930c189be9a
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 如何使用 Azure CLI 扩展 Linux VM 上的虚拟硬盘
<a id="how-to-expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli" class="xliff"></a>
在 Azure 的 Linux 虚拟机 (VM) 上，操作系统 (OS) 的默认虚拟硬盘大小通常为 30 GB。 可通过[添加数据磁盘](add-disk.md)来扩充存储空间，也可扩展 OS 磁盘或现有的数据磁盘。 本文详述如何使用 Azure CLI 2.0 扩展 Linux VM 的托管磁盘。 也可使用 [Azure CLI 1.0](expand-disks-nodejs.md) 扩展非托管 OS 磁盘。

## 扩展磁盘
<a id="expand-disk" class="xliff"></a>
确保已安装了最新的 [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) 并已使用 [az login](https://docs.microsoft.com/cli/azure/#login) 登录到 Azure 帐户。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

在以下示例中，请将示例参数名称替换为你自己的值。 示例参数名称包括 *myResourceGroup* 和 *myVM*。

1. 当 VM 正在运行时，无法执行虚拟硬盘上的操作。 使用 [az vm deallocate](https://docs.microsoft.com/cli/azure/vm#deallocate) 解除分配 VM。 以下示例在名为 myResourceGroup 的资源组中解除分配名为 myVM 的 VM：

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `az vm stop` 不释放计算资源。 若要释放计算资源，请使用 `az vm deallocate`。 只有释放 VM 才能扩展虚拟硬盘。

2. 使用 [az disk list](https://docs.microsoft.com/cli/azure/disk#list) 查看资源组中的托管磁盘列表。 以下示例显示名为 myResourceGroup 的资源组中的托管磁盘列表：

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    使用 [az disk update](https://docs.microsoft.com/cli/azure/disk#update) 扩展所需磁盘。 以下示例将名为 myDataDisk 的托管磁盘的大小扩展为 200 GB：

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > 扩展托管磁盘时，更新的大小会映射到最近的托管磁盘大小。 有关可用托管磁盘大小和层的表，请参阅 [Azure 托管磁盘概述 - 定价和计费](../../storage/storage-managed-disks-overview.md#pricing-and-billing)。

3. 使用 [az vm start](https://docs.microsoft.com/cli/azure/vm#start) 启动 VM。 以下示例在名为 myResourceGroup 的资源组中启动名为 myVM 的 VM：

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. 使用相应的凭据通过 SSH 连接到 VM。 若要验证是否已调整 OS 磁盘的大小，请使用 `df -h`。 以下示例输出显示数据驱动器 (/dev/sdc1) 现在为 200 GB：

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        194G   52M   193G   1% /datadrive
    ```

## 后续步骤
<a id="next-steps" class="xliff"></a>
如需更多存储，也可[向 Linux VM 添加数据磁盘](add-disk.md)。 有关磁盘加密的详细信息，请参阅[使用 Azure CLI 加密 Linux VM 上的磁盘](encrypt-disks.md)。