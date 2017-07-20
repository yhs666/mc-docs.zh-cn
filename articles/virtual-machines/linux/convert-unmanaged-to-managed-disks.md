---
title: "将 Azure 中的 Linux VM 从非托管磁盘转换为托管磁盘 | Azure"
description: "如何使用 Azure CLI 2.0 将 VM 从非托管磁盘转换为 Azure 托管磁盘"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
origin.date: 02/09/2017
ms.date: 06/20/2017
ms.author: v-dazen
ms.openlocfilehash: dcfdceb144d090b9a8c37c8266425bcea5411187
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# <a name="how-to-convert-a-linux-vm-from-unmanaged-disks-to-azure-managed-disks"></a>如何将 Linux VM 从非托管磁盘转换为 Azure 托管磁盘

如果 Azure 中的现有 Linux VM 在存储帐户中使用非托管磁盘，而你希望这些 VM 能充分利用托管磁盘，可以转换这些 VM。 此过程将同时转换 OS 磁盘和任何附加的数据磁盘。 该转换过程需要重新启动 VM，因此请在预先存在的维护时段内计划 VM 迁移。 迁移过程不可逆。 在生产中执行迁移之前，请务必通过迁移测试虚拟机来测试迁移过程。

> [!IMPORTANT]
> 在转换期间，将解除分配 VM。 转换完成后，VM 在启动时会接收新的 IP 地址。 如果依赖于固定 IP，请使用保留 IP。

如果非托管磁盘所在的存储帐户已使用或曾使用 [Azure 存储服务加密 (SSE)](../../storage/storage-service-encryption.md?toc=%2fvirtual-machines%2flinux%2ftoc.json) 加密，则无法将其转换为托管磁盘。 以下步骤详细说明了如何转换位于（或曾位于）已加密存储帐户的非托管磁盘：

- 使用 [az storage blob copy start](https://docs.microsoft.com/cli/azure/storage/blob/copy#start) 将虚拟硬盘 (VHD) 复制到从未启用 Azure 存储服务加密的存储帐户。
- 使用 [az vm create](https://docs.microsoft.com/cli/azure/vm#create) 创建使用托管磁盘的 VM 并指定创建期间的 VHD 文件，或
- 使用 [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#attach) 将复制的 VHD 附加到具有托管磁盘的正在运行中的 VM。

## <a name="convert-vm-to-azure-managed-disks"></a>将 VM 转换为 Azure 托管磁盘
本部分说明如何将现有的 Azure VM 从非托管磁盘转换为托管磁盘。 可通过此过程，将高级 (SSD) 非托管磁盘转换为高级托管磁盘，或将标准 (HDD) 非托管磁盘转换为标准托管磁盘。

1. 使用 [az vm deallocate](https://docs.microsoft.com/cli/azure/vm#deallocate)解除分配 VM。 以下示例在名为 `myResourceGroup` 的资源组中解除分配名为 `myVM` 的 VM：

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. 使用 [az vm convert](https://docs.microsoft.com/cli/azure/vm#convert)将 VM 转换为托管磁盘。 以下过程转换名为 `myVM` 的 VM，包括 OS 磁盘和所有数据磁盘：

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. 转换为托管磁盘后，使用 [az vm start](https://docs.microsoft.com/cli/azure/vm#start)启动 VM。 以下示例启动名为 `myResourceGroup` 的资源组中名为 `myVM` 的 VM。

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vm-in-an-availability-set-to-managed-disks"></a>将可用性集中的 VM 转换为托管磁盘

如果要转换为托管磁盘的 VM 位于可用性集中，则需要先将可用性集转换为托管可用性集。

可用性集中的所有 VM 都必须在转换可用性集之前解除分配。 可用性集本身转换为托管可用性集后，计划将所有 VM 转换为托管磁盘。 然后可以启动所有 VM，并照常继续操作。

1. 使用 [az vm availability-set list](https://docs.microsoft.com/cli/azure/vm/availability-set#list)列出可用性集中的所有 VM。 以下示例列出了名为 `myResourceGroup` 的资源组中名为 `myAvailabilitySet` 的可用性集中的所有 VM：

    ```azurecli
    az vm availability-set show --resource-group myResourceGroup \
        --name myAvailabilitySet --query [virtualMachines[*].id] --output table
    ```

2. 使用 [az vm deallocate](https://docs.microsoft.com/cli/azure/vm#deallocate)解除分配所有 VM。 以下示例在名为 `myResourceGroup` 的资源组中解除分配名为 `myVM` 的 VM：

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. 使用 [az vm availability-set convert](https://docs.microsoft.com/cli/azure/vm/availability-set#convert)转换可用性集。 以下示例转换名为 `myResourceGroup` 的资源组中名为 `myAvailabilitySet` 的可用性集：

    ```azurecli
    az vm availability-set convert --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. 使用 [az vm convert](https://docs.microsoft.com/cli/azure/vm#convert)将所有 VM 转换为托管磁盘。 以下过程转换名为 `myVM` 的 VM，包括 OS 磁盘和所有数据磁盘：

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. 转换为托管磁盘后，使用 [az vm start](https://docs.microsoft.com/cli/azure/vm#start)启动所有 VM。 以下示例启动名为 `myResourceGroup` 的资源组中名为 `myVM` 的 VM。

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a>后续步骤
有关存储选项的详细信息，请参阅 [Azure 托管磁盘概述](../../storage/storage-managed-disks-overview.md)
