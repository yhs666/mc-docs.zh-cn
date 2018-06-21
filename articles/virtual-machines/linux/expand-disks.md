---
title: 在 Azure 中扩展 Linux VM 上的虚拟硬盘 | Azure
description: 了解如何使用 Azure CLI 2.0 在 Linux VM 上扩展虚拟硬盘
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 12/13/2017
ms.date: 04/16/2018
ms.author: v-yeche
ms.openlocfilehash: 04731c0575b3286e9950c7cb6b1fa68e57c1b9f7
ms.sourcegitcommit: 966200f9807bfbe4986fa67dd34662d5361be221
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2018
ms.locfileid: "31510976"
---
# <a name="how-to-expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli"></a>如何使用 Azure CLI 扩展 Linux VM 上的虚拟硬盘
在 Azure 的 Linux 虚拟机 (VM) 上，操作系统 (OS) 的默认虚拟硬盘大小通常为 30 GB。 可通过[添加数据磁盘](add-disk.md)来扩充存储空间，但也可能想要扩展现有的数据磁盘。 本文详述如何使用 Azure CLI 2.0 扩展 Linux VM 的托管磁盘。 也可使用 [Azure CLI 1.0](expand-disks-nodejs.md) 扩展非托管 OS 磁盘。

> [!WARNING]
> 执行磁盘重设大小操作前请务必确保已备份数据。 有关详细信息，请参阅[在 Azure 中备份 Linux VM](tutorial-backup-vms.md)。

## <a name="expand-azure-managed-disk"></a>扩展 Azure 托管磁盘
确保已安装了最新的 [Azure CLI 2.0](https://docs.azure.cn/zh-cn/cli/install-az-cli2?view=azure-cli-latest) 并已使用 [az login](https://docs.azure.cn/zh-cn/cli/reference-index?view=azure-cli-latest#az-login) 登录到 Azure 帐户。

本文需要 Azure 中的现有 VM 已附加至少一个数据磁盘并且该磁盘已准备就绪。 如果尚无可用的 VM，请参阅[使用数据磁盘创建和准备 VM](tutorial-manage-disks.md#create-and-attach-disks)。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]


在以下示例中，请将示例参数名称替换成自己的值。 示例参数名称包括 myResourceGroup 和 myVM。

1. 当 VM 正在运行时，无法执行虚拟硬盘上的操作。 使用 [az vm deallocate](https://docs.azure.cn/zh-cn/cli/vm?view=azure-cli-latest#az_vm_deallocate) 解除分配 VM。 以下示例在名为 myResourceGroup 的资源组中解除分配名为 myVM 的 VM：

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > 只有释放 VM 才能扩展虚拟硬盘。 `az vm stop` 不释放计算资源。 若要释放计算资源，请使用 `az vm deallocate`。

2. 使用 [az disk list](https://docs.azure.cn/zh-cn/cli/disk?view=azure-cli-latest#az_disk_list) 查看资源组中的托管磁盘列表。 以下示例显示名为 myResourceGroup 的资源组中的托管磁盘列表：

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    使用 [az disk update](https://docs.azure.cn/zh-cn/cli/disk?view=azure-cli-latest#az_disk_update) 扩展所需磁盘。 以下示例将名为 myDataDisk 的托管磁盘的大小扩展为 200 GB：

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > 扩展托管磁盘时，更新的大小会映射到最近的托管磁盘大小。 有关可用托管磁盘大小和层的表，请参阅 [Azure 托管磁盘概述 - 定价和计费](../windows/managed-disks-overview.md#pricing-and-billing)。

3. 使用 [az vm start](https://docs.azure.cn/zh-cn/cli/vm?view=azure-cli-latest#az_vm_start) 启动 VM。 以下示例在名为 myResourceGroup 的资源组中启动名为 myVM 的 VM：

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="expand-disk-partition-and-filesystem"></a>扩展磁盘分区和文件系统
若要使用扩展磁盘，需扩展基础分区和文件系统。

1. 使用相应的凭据通过 SSH 连接到 VM。 可使用 [az vm show](https://docs.azure.cn/zh-cn/cli/vm?view=azure-cli-latest#az_vm_show) 获取 VM 的 公共 IP 地址：

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

2. 若要使用扩展磁盘，需扩展基础分区和文件系统。

    a. 如果已装载，请卸载磁盘：

    ```bash
    sudo umount /dev/sdc1
    ```

    b. 使用 `parted` 查看磁盘信息并重设分区大小：

    ```bash
    sudo parted /dev/sdc
    ```

    使用 `print` 查看有关现有分区布局的信息。 其输出类似于以下示例，该示例显示基础磁盘大小为 215Gb：

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:

    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    c. 使用 `resizepart` 展开分区。 输入分区号 1 以及新分区的大小：

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    d. 若要退出，请输入 `quit`

3. 重设分区大小后，请使用 `e2fsck` 验证分区一致性：

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

4. 现在使用 `resize2fs` 重设文件系统大小：

    ```bash
    sudo resize2fs /dev/sdc1
    ```

5. 将分区安装到目标位置，例如 `/datadrive`：

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

6. 若要验证是否已调整 OS 磁盘的大小，请使用 `df -h`。 以下示例输出显示数据驱动器 (/dev/sdc1) 现在为 200 GB：

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a>后续步骤
如需更多存储，也可[向 Linux VM 添加数据磁盘](add-disk.md)。 有关磁盘加密的详细信息，请参阅[使用 Azure CLI 加密 Linux VM 上的磁盘](encrypt-disks.md)。
<!--Update_Description: update meta properties, update link -->