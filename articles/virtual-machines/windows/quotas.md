---
title: "Azure 的 vCPU 配额 | Azure"
description: "了解 Azure 的 vCPU 配额。"
keywords: 
services: virtual-machines-windows
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 12/05/2016
ms.date: 01/08/2018
ms.author: v-yeche
ms.openlocfilehash: 6e56bee060733a7ad9186e2041cc88ae2fe7b850
ms.sourcegitcommit: f02cdaff1517278edd9f26f69f510b2920fc6206
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2018
---
# <a name="virtual-machine-vcpu-quotas"></a>虚拟机 vCPU 配额

虚拟机和虚拟机规模集的 vCPU 配额已根据每个区域中的每个订阅划分成两层。 第一层是区域 vCPU 总数，第二层是各种 VM 大小系列核心，例如标准 D 系列 vCPU。 每当部署新 VM 时，其 vCPU 数目不得超过特定 VM 大小系列的 vCPU 配额或区域 vCPU 总数配额。 如果超过其中一项配额，则不允许部署该 VM。 此外，区域中的虚拟机总数也有一个配额。 可以在 [Azure 门户](https://portal.azure.cn)的“订阅”页的“用量 + 配额”部分中查看其中每项配额的详细信息，或者，可以使用 PowerShell 查询这些值。

## <a name="check-usage"></a>检查使用情况

可以使用 [Get-AzureRmVMUsage](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmusage) cmdlet 检查配额使用情况。

```azurepowershell-interactive
Get-AzureRmVMUsage -Location "China East"
```

输出类似于以下内容：

```
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional vCPUs                         4   260 Count
Virtual Machines                             4 10000 Count
Virtual Machine Scale Sets                   1  2000 Count
Standard B Family vCPUs                      1    10 Count
Standard DSv2 Family vCPUs                   1   100 Count
Standard Dv2 Family vCPUs                    2   100 Count
Basic A Family vCPUs                         0   100 Count
Standard A0-A7 Family vCPUs                  0   250 Count
Standard A8-A11 Family vCPUs                 0   100 Count
Standard D Family vCPUs                      0   100 Count
Standard G Family vCPUs                      0   100 Count
Standard DS Family vCPUs                     0   100 Count
Standard GS Family vCPUs                     0   100 Count
Standard F Family vCPUs                      0   100 Count
Standard FS Family vCPUs                     0   100 Count
Standard NV Family vCPUs                     0    24 Count
Standard NC Family vCPUs                     0    48 Count
Standard H Family vCPUs                      0     8 Count
Standard Av2 Family vCPUs                    0   100 Count
Standard LS Family vCPUs                     0   100 Count
Standard Dv2 Promo Family vCPUs              0   100 Count
Standard DSv2 Promo Family vCPUs             0   100 Count
Standard MS Family vCPUs                     0     0 Count
Standard Dv3 Family vCPUs                    0   100 Count
Standard DSv3 Family vCPUs                   0   100 Count
Standard Ev3 Family vCPUs                    0   100 Count
Standard ESv3 Family vCPUs                   0   100 Count
Standard FSv2 Family vCPUs                   0   100 Count
Standard ND Family vCPUs                     0     0 Count
Standard NCv2 Family vCPUs                   0     0 Count
Standard NCv3 Family vCPUs                   0     0 Count
Standard LSv2 Family vCPUs                   0     0 Count
Standard Storage Managed Disks               2 10000 Count
Premium Storage Managed Disks                1 10000 Count

```

## <a name="reserved-vm-instances"></a>保留 VM 实例
保留 VM 实例对应于单个订阅，会给 vCPU 配额造成新的影响。 这些值描述订阅中必须可以部署的规定大小的实例数。 它们在配额系统中用作占位符，确保保留该配额，以便能够在订阅中部署保留的实例。 例如，如果特定订阅包含 10 个 Standard_D1 保留实例，则 Standard_D1 保留实例的用量限制将是 10。 这样，Azure 便可以确保“区域 vCPU 总数”配额中始终至少有 10 个 vCPU 可用于 Standard_D1 实例，并且“标准 D 系列 vCPU”配额中至少有 10 个 vCPU 可用于 Standard_D1 实例。

如果需要提高配额才能购买单一订阅 RI，可以[请求提高订阅的配额](/azure-supportability/resource-manager-core-quotas-request)。

## <a name="next-steps"></a>后续步骤

有关计费和配额的详细信息，请参阅 [Azure 订阅和服务限制、配额与约束](/azure-subscription-service-limits?toc=/azure/billing/TOC.json)。
<!--Update_Description: new article on quotas in Windows VM-->
<!--ms.date: 01/08/2018, update the output screen -->