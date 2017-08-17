---
title: "将 Azure 中的 Linux 虚拟机从非托管磁盘转换为托管磁盘 - Azure 托管磁盘 | Azure"
description: "如何在资源管理器部署模型中使用 Azure CLI 2.0 将 Linux VM 从非托管磁盘转换为托管磁盘"
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
origin.date: 06/23/2017
ms.date: 08/14/2017
ms.author: v-dazen
ms.openlocfilehash: 1c34afdcda7a2279e73aa67f97b219b7a9de3b23
ms.sourcegitcommit: f858adac6a7a32df67bcd5c43946bba5b8ec6afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2017
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>将 Linux 虚拟机从非托管磁盘转换为托管磁盘

如果有使用非托管磁盘的现有 Linux 虚拟机 (VM)，可通过 [Azure 托管磁盘](../../storage/storage-managed-disks-overview.md)服务转换 VM 以使用托管磁盘。 此过程将同时转换 OS 磁盘和任何附加的数据磁盘。

本文介绍如何使用 Azure CLI 转换 VM。 如果需要对其进行安装或升级，请参阅[安装 Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)。 

## <a name="before-you-begin"></a>开始之前

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]

## <a name="convert-single-instance-vms"></a>转换单实例 VM
本节介绍如何将单实例 Azure VM 从非托管磁盘转换为托管磁盘。 （如果 VM 位于可用性集中，请参阅下一部分。）可通过此过程，将 VM 从高级 (SSD) 非托管磁盘转换为高级托管磁盘，或从标准 (HDD) 非托管磁盘转换为标准托管磁盘。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

1. 使用 [az vm deallocate](https://docs.microsoft.com/cli/azure/vm#deallocate) 解除分配 VM。 以下示例在名为 `myResourceGroup` 的资源组中解除分配名为 `myVM` 的 VM：

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. 使用 [az vm convert](https://docs.microsoft.com/cli/azure/vm#convert) 将 VM 转换为托管磁盘。 以下过程转换名为 `myVM` 的 VM，包括 OS 磁盘和任何数据磁盘：

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. 使用 [az vm start](https://docs.microsoft.com/cli/azure/vm#start) 在转换为托管磁盘后启动 VM。 以下示例启动名为 `myResourceGroup` 的资源组中名为 `myVM` 的 VM。

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a>转换可用性集中的 VM

如果要转换为托管磁盘的 VM 位于可用性集中，则需要先将可用性集转换为托管可用性集。

可用性集中的所有 VM 都必须在转换可用性集之前解除分配。 可用性集本身转换为托管可用性集后，计划将所有 VM 转换为托管磁盘。 然后，启动所有 VM，并继续照常操作。

1. 使用 [az vm availability-set list](https://docs.microsoft.com/cli/azure/vm/availability-set#list) 列出可用性集中的所有 VM。 以下示例列出了名为 `myResourceGroup` 的资源组中名为 `myAvailabilitySet` 的可用性集中的所有 VM：

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. 使用 [az vm deallocate](https://docs.microsoft.com/cli/azure/vm#deallocate) 解除分配所有 VM。 以下示例在名为 `myResourceGroup` 的资源组中解除分配名为 `myVM` 的 VM：

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. 使用 [az vm availability-set convert](https://docs.microsoft.com/cli/azure/vm/availability-set#convert) 转换可用性集。 以下示例转换名为 `myResourceGroup` 的资源组中名为 `myAvailabilitySet` 的可用性集：

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. 使用 [az vm convert](https://docs.microsoft.com/cli/azure/vm#convert) 将所有 VM 转换为托管磁盘。 以下过程转换名为 `myVM` 的 VM，包括 OS 磁盘和任何数据磁盘：

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. 使用 [az vm start](https://docs.microsoft.com/cli/azure/vm#start) 在转换为托管磁盘后启动所有 VM。 以下示例在名为 `myResourceGroup` 的资源组中启动名为 `myVM` 的 VM：

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="managed-disks-and-azure-storage-service-encryption"></a>托管磁盘和 Azure 存储服务加密
如果非托管磁盘所在的存储帐户曾使用 [Azure 存储服务加密](../../storage/storage-service-encryption.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)进行了加密，则无法使用之前的步骤将该非托管磁盘转换为托管磁盘。 下列步骤详细说明了如何复制和使用曾位于加密存储帐户中的非托管磁盘：

1. 使用 [az storage blob copy start](https://docs.microsoft.com/cli/azure/storage/blob/copy#start) 将 VHD 复制到从未启用过 Azure 存储服务加密的存储帐户。

2. 通过以下任一方式使用复制的 VM：

   * 使用 [az vm create](https://docs.microsoft.com/cli/azure/vm#create) 创建使用托管磁盘的 VM 并在创建期间指定该 VHD 文件。

   * 使用 [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#attach) 将复制的 VHD 附加到正在运行且使用托管磁盘的 VM。

## <a name="next-steps"></a>后续步骤
有关存储选项的详细信息，请参阅 [Azure 托管磁盘概述](../../storage/storage-managed-disks-overview.md)。

<!--Update_Description: add Section "Managed disks and Azure Storage Service Encryption"-->