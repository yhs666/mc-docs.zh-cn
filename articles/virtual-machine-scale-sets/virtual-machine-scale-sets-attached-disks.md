---
title: "Azure 虚拟机规模集附加数据磁盘 | Azure"
description: "了解如何将附加数据磁盘与虚拟机规模集配合使用"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
wacn.date: 
ms.author: guybo
ms.translationtype: Human Translation
ms.sourcegitcommit: 4a18b6116e37e365e2d4c4e2d144d7588310292e
ms.openlocfilehash: 36b639cd6ff2e6505ef5b27810c67b9256fa0de8
ms.contentlocale: zh-cn
ms.lasthandoff: 05/19/2017


---
# <a name="azure-vm-scale-sets-and-attached-data-disks"></a>Azure VM 规模集及附加数据磁盘
Azure [虚拟机规模集](/virtual-machine-scale-sets/)现在支持对虚拟机附加数据磁盘。 对于使用 Azure 托管磁盘创建的规模集，可以在存储配置文件中定义数据磁盘。 以前，OS 驱动器和临时驱动器是规模集中 VM 的唯一直接连接的存储选项。

> [!NOTE]
>  创建具有已定义附加数据磁盘的规模集时，仍然需要从 VM 中装载和格式化这些磁盘才能使用它们（就像独立 Azure VM 一样）。 若要执行此操作，一种简便的方法是使用自定义脚本扩展，其调用标准脚本，在 VM 上对所有数据磁盘进行分区和格式化。

## <a name="create-a-scale-set-with-attached-data-disks"></a>创建具有附加数据磁盘的规模集
若要创建具有附加数据磁盘的规模集，一种简单的方法是使用 [Azure CLI](https://github.com/Azure/azure-cli) _vmss create_ 命令。 以下示例创建一个 Azure 资源组，以及一个包含 10 个 Ubuntu VM 的虚拟机规模集，它们都具有 2 个大小分别为 50 GB 和 100 GB 的附加数据磁盘。

```bash
az group create -l chinaeast -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

请注意，如果不指定某些配置值， _vmss create_ 命令将使用默认值。 若要查看可以重写的可用选项，请尝试：

```bash
az vmss create --help
```

若要创建包含附加数据磁盘的规模集，另一种方法是在 Azure Resource Manager 模板中定义一个规模集，在 storageProfile 中包括 dataDisks 节，然后部署模板。 上述的 50 GB 和 100 GB 磁盘示例将在模板中定义为：

```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    }
]
```

可以在此处看到完整的具有已定义附加磁盘的规模集模板，该模板已准备就绪可进行部署： [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data)。

## <a name="adding-a-data-disk-to-an-existing-scale-set"></a>将数据磁盘添加到现有规模集
> [!NOTE]
>  只能将数据磁盘附加到使用 [Azure 托管磁盘](./virtual-machine-scale-sets-managed-disks.md)创建的规模集。

可使用 Azure CLI _az vmss disk attach_ 命令将数据磁盘添加到 VM 规模集。 请确保指定的 lun 未被使用。 下面的 CLI 示例将一个 50 GB 的驱动器添加到 lun 3：

```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```


以下 PowerShell 示例将 50 GB 的驱动器添加到 lun 3：
```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myvmssrg -VMScaleSetName myvmss
$vmss = Add-AzureRmVmssDataDisk -VirtualMachineScaleSet $vmss -Lun 3 -Caching 'ReadWrite' -CreateOption Empty -DiskSizeGB 50 -StorageAccountType StandardLRS
Update-AzureRmVmss -ResourceGroupName myvmssrg -Name myvmss -VirtualMachineScaleSet $vmss
```

> [!NOTE]
> 不同的 VM 大小所支持的附加驱动器数量有不同的限制。 在添加新磁盘之前，请检查[虚拟机大小特征](../virtual-machines/windows/sizes.md)。

也可通过以下方式添加磁盘：先向规模集定义的 storageProfile 中的 dataDisks 属性添加新条目，然后应用所做的更改。 若要测试这一点，请在 [Azure 资源管理器](https://resources.azure.com/)中找到现有规模集定义。 选择“编辑”  并将新磁盘添加到数据磁盘列表。 例如 使用以上示例：

```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    },
    {
    "lun": 3,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 20
    }          
]
```

然后选择“PUT”  将更改应用于规模集。 如果使用的 VM 大小支持两个以上附加数据磁盘，则此示例适用。

> [!NOTE]
> 对规模集定义进行更改时（如添加或删除数据磁盘），更改适用于所有新建 VM，但仅在 _upgradePolicy_ 属性设置为“自动”的情况下，才适用于现有 VM。 如果此属性设置为“手动”，则需手动将新模型应用于现有 VM。 可以在门户中执行此操作，使用 Update-AzureRmVmssInstance PowerShell 命令或 az vmss update-instances CLI 命令。

## <a name="removing-a-data-disk-from-a-scale-set"></a>从规模集中删除数据磁盘
可使用 Azure CLI _az vmss disk detach_ 命令从 VM 规模集中删除数据磁盘。 例如，以下命令将删除 lun 2 处定义的磁盘：

```bash
az vmss disk detach -g dsktest -n dskvmss --lun 2
```  

类似地，也可通过以下方式从规模集中移除磁盘：删除 storageProfile 的 dataDisks 属性中的条目，然后应用所做的更改。 

## <a name="additional-notes"></a>附加说明
[2016-04-30-preview](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) 版 Microsoft.Compute API 中增加了对 Azure 托管磁盘和规模集附加数据磁盘的支持。 可以使用此版本或更高版本的 API 构建的任何 SDK 或命令行工具。

在对规模集的附加磁盘支持初始实现中，不能在规模集中将数据磁盘附加到单个 VM，也不能从中分离。

最初，对规模集中附加数据磁盘的 Azure 门户预览支持是受限制的。 根据需求，可使用 Azure 模板、CLI、PowerShell、SDK 和 REST API 来管理附加磁盘。

