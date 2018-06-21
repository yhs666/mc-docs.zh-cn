---
title: Azure 的 vCPU 配额 | Azure
description: 了解 Azure 的 vCPU 配额。
keywords: ''
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
origin.date: 12/05/2016
ms.date: 05/14/2018
ms.author: v-yeche
ms.openlocfilehash: 9ae2275c5b40e1dc33f0e7046095036a301655b3
ms.sourcegitcommit: 6f08b9a457d8e23cf3141b7b80423df6347b6a88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2018
ms.locfileid: "34061972"
---
# <a name="virtual-machine-vcpu-quotas"></a>虚拟机 vCPU 配额

虚拟机和虚拟机规模集的 vCPU 配额已根据每个区域中的每个订阅划分成两层。 第一层是区域 vCPU 总数，第二层是各种 VM 大小系列核心，例如标准 D 系列 vCPU。 每当部署新 VM 时，其 vCPU 数目不得超过特定 VM 大小系列的 vCPU 配额或区域 vCPU 总数配额。 如果超过其中一项配额，则不允许部署该 VM。 此外，区域中的虚拟机总数也有一个配额。 可以在 [Azure 门户](https://portal.azure.cn)的“订阅”页的“用量 + 配额”部分中查看其中每项配额的详细信息，或者，可以使用 Azure CLI 查询这些值。

## <a name="check-usage"></a>检查用量

可以使用 [az vm list-usage](https://docs.azure.cn/zh-cn/cli/vm?view=azure-cli-latest#az-vm-list-usage) 检查配额用量。

```azurecli
az vm list-usage --location "China East"
[
  …
  {
    "currentValue": 4,
    "limit": 260,
    "name": {
      "localizedValue": "Total Regional vCPUs",
      "value": "cores"
    }
  },
  {
    "currentValue": 4,
    "limit": 10000,
    "name": {
      "localizedValue": "Virtual Machines",
      "value": "virtualMachines"
    }
  },
  {
    "currentValue": 1,
    "limit": 2000,
    "name": {
      "localizedValue": "Virtual Machine Scale Sets",
      "value": "virtualMachineScaleSets"
    }
  },
  {
    "currentValue": 1,
    "limit": 10,
    "name": {
      "localizedValue": "Standard B Family vCPUs",
      "value": "standardBFamily"
    }
  },
```
## <a name="reserved-vm-instances"></a>保留的 VM 实例
保留 VM 实例对应于单个订阅，会给 vCPU 配额造成新的影响。 这些值描述订阅中必须可以部署的规定大小的实例数。 它们在配额系统中用作占位符，确保保留该配额，以便能够在订阅中部署保留的实例。 例如，如果特定订阅包含 10 个 Standard_D1 保留实例，则 Standard_D1 保留实例的用量限制将是 10。 这样，Azure 便可以确保“区域 vCPU 总数”配额中始终至少有 10 个 vCPU 可用于 Standard_D1 实例，并且“标准 D 系列 vCPU”配额中至少有 10 个 vCPU 可用于 Standard_D1 实例。

如果需要提高配额才能购买单一订阅 RI，可以[请求提高订阅的配额](https://support.windowsazure.cn/support/support-azure)。
<!-- URL /azure-supportability/resource-manager-core-quotas-request SHOULD BE https://support.windowsazure.cn/support/support-azure -->

## <a name="next-steps"></a>后续步骤

有关计费和配额的详细信息，请参阅 [Azure 订阅和服务限制、配额与约束](/azure-subscription-service-limits?toc=/azure/billing/TOC.json)。
<!-- Update_Description: update meta propertiessss -->