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
origin.date: 05/31/2018
ms.date: 08/27/2018
ms.author: v-yeche
ms.openlocfilehash: 8007331f0bd0b8731019415c3908b018210aeed2
ms.sourcegitcommit: bdffde936fa2a43ea1b5b452b56d307647b5d373
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "42871636"
---
# <a name="virtual-machine-vcpu-quotas"></a>虚拟机 vCPU 配额

虚拟机和虚拟机规模集的 vCPU 配额已根据每个区域中的每个订阅划分成两层。 第一层是区域的 vCPU 总数，第二层是各种 VM 大小系列核心（如 D 系列 vCPU）。 每当部署新 VM 时，VM 的 vCPU 数不能超过 VM 大小系列的 vCPU 配额或区域 vCPU 配额总数。 如果超过了上述任一配额，将不允许部署 VM。 此外，区域中的虚拟机总数也有一个配额。 有关上述每个配额的详细信息，可以在 [Azure 门户](https://portal.azure.cn)的“订阅”页的“使用情况 + 配额”部分中查看，也可以使用 Azure CLI 查询值。

## <a name="check-usage"></a>检查使用情况

可以使用 [az vm list-usage](https://docs.azure.cn/zh-cn/cli/vm?view=azure-cli-latest#az-vm-list-usage) 检查配额用量。

```azurecli
az vm list-usage --location "China East" -o table
```

输出应类似于：

```
Name                                CurrentValue    Limit
--------------------------------  --------------  -------
Availability Sets                              0     2000
Total Regional vCPUs                          29      100
Virtual Machines                               7    10000
Virtual Machine Scale Sets                     0     2000
Standard DSv3 Family vCPUs                     8      100
Standard DSv2 Family vCPUs                     3      100
Standard Dv3 Family vCPUs                      2      100
Standard D Family vCPUs                        8      100
Standard Dv2 Family vCPUs                      8      100
Basic A Family vCPUs                           0      100
Standard A0-A7 Family vCPUs                    0      100
Standard DS Family vCPUs                       0      100
Standard F Family vCPUs                        0      100
Standard FS Family vCPUs                       0      100
Standard Storage Managed Disks                 5    10000
Premium Storage Managed Disks                  5    10000
```
<!-- Not Available on Standard A8-A11 Family vCPUs -->
<!-- Not Available on Standard G Family vCPUs -->
<!-- Not Available on Standard GS Family vCPUs -->

## <a name="reserved-vm-instances"></a>虚拟机预留实例
虚拟机预留实例对应于单个订阅，会给 vCPU 配额造成新的影响。 这些值描述订阅中必须可以部署的规定大小的实例数。 它们在配额系统中用作占位符，确保预留该配额，以便能够在订阅中部署 Azure 预留。 例如，如果特定订阅包含 10 个 Standard_D1 预留，则 Standard_D1 预留的用量限制将是 10。 这样，Azure 便可以确保“区域 vCPU 总数”配额中始终至少有 10 个 vCPU 可用于 Standard_D1 实例，并且“标准 D 系列 vCPU”配额中至少有 10 个 vCPU 可用于 Standard_D1 实例。

如果需要提高配额才能购买单一订阅 RI，可以[请求提高订阅的配额](https://support.windowsazure.cn/support/support-azure)。
<!-- URL /azure-supportability/resource-manager-core-quotas-request SHOULD BE https://support.windowsazure.cn/support/support-azure -->

## <a name="next-steps"></a>后续步骤

有关计费和配额的详细信息，请参阅 [Azure 订阅和服务限制、配额与约束](/azure-subscription-service-limits?toc=/azure/billing/TOC.json)。
<!-- Update_Description: update meta properties, wording update -->