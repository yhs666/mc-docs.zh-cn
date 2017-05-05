---
title: 在 Azure 虚拟机规模集中使用托管磁盘 | Azure
description: 了解在虚拟机规模集中使用托管磁盘的原因和如何使用
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: timlt
editor: ''
tags: azure-resource-manager

ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/14/2017
wacn.date: 03/03/2017
ms.author: negat
---

# Azure VM 规模集和托管磁盘

Azure [虚拟机规模集](/azure/virtual-machine-scale-sets/)现在支持具有托管磁盘的虚拟机。在规模集中使用托管磁盘有以下好处：

* 不再需要预先创建和管理存储帐户以存储规模集 VM 的 OS 磁盘。

* 可以将托管的数据磁盘附加到规模集。

* 使用托管磁盘后，规模集的容量可高达 1,000 个 VM（如果基于平台映像）或者 100 个 VM（如果基于自定义映像）。

## 入门

初次使用托管磁盘规模集的一种简单方法是在 Azure 门户预览中部署一个这样的规模集。有关详细信息，请参阅[此文章](./virtual-machine-scale-sets-portal-create.md)。另一种简单的入门方法是使用 [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) 部署规模集。以下示例演示如何创建具有 10 个 VM 的基于 Ubuntu 的规模集，其中每个 VM 都有 50 GB 和 100 GB 的数据磁盘：

```azurecli
az group create -l chinaeast -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

或者，你可以在 [Azure 快速入门模板 Github 存储库](https://github.com/Azure/azure-quickstart-templates)中查找包含 `vmss` 的文件夹，以查看预先构建的用于部署规模集的模板示例。若要区分哪些模板已使用托管磁盘，可以参考[此列表](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)。

## API 版本

针对使用托管磁盘的规模集的当前正式发布的 API 版本为 `2016-04-30-preview`。即使在支持托管磁盘的新 API 版本中，也可以和当前一样继续使用具有非托管磁盘的规模集。但是，使用非托管磁盘的规模集无法获得托管磁盘的优点，即使在这些新的 api 版本中也是如此。

## 后续步骤

若要查找一般情况下有关托管磁盘的详细信息，请参阅[本文](/documentation/articles/storage-managed-disks-overview/)。

若要了解如何转换 Resource Manager 模板以预配使用托管磁盘的规模集，请参阅[本文](./virtual-machine-scale-sets-convert-template-to-md.md)。对 Resource Manager 模板的修改也适用于 Azure REST API。

若要了解有关在规模集中使用托管数据磁盘的详细信息，请参阅[本文](./virtual-machine-scale-sets-attached-disks.md)。

若要开始使用大型规模集，请参阅[本文](./virtual-machine-scale-sets-placement-groups.md)。

<!---HONumber=Mooncake_0227_2017-->