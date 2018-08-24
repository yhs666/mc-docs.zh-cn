---
title: Azure 的 vCPU 配额 | Azure
description: 了解 Azure 的 vCPU 配额。
keywords: ''
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 05/31/2018
ms.date: 08/27/2018
ms.author: v-yeche
ms.openlocfilehash: 05d733de6e647cd7b5a5a98bcfe5a151394e2bf6
ms.sourcegitcommit: bdffde936fa2a43ea1b5b452b56d307647b5d373
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "42872365"
---
# <a name="virtual-machine-vcpu-quotas"></a>虚拟机 vCPU 配额

虚拟机和虚拟机规模集的 vCPU 配额已根据每个区域中的每个订阅划分成两层。 第一层是区域的 vCPU 总数，第二层是各种 VM 大小系列核心（如 D 系列 vCPU）。 每当部署新 VM 时，VM 的 vCPU 数不能超过 VM 大小系列的 vCPU 配额或区域 vCPU 配额总数。 如果超过了上述任一配额，将不允许部署 VM。 此外，区域中的虚拟机总数也有一个配额。 可以在 [Azure 门户](https://portal.azure.cn)的“订阅”页的“用量 + 配额”部分中查看其中每项配额的详细信息，或者，可以使用 PowerShell 查询这些值。

## <a name="check-usage"></a>检查使用情况

可以使用 [Get-AzureRmVMUsage](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmusage) cmdlet 检查配额使用情况。

```PowerShell
Get-AzureRmVMUsage -Location "China East"
```

输出类似于以下内容：

<!--PENDING FOR NC series GA ANOUNCEMENT -->
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
Standard D Family vCPUs                      0   100 Count
Standard DS Family vCPUs                     0   100 Count
Standard F Family vCPUs                      0   100 Count
Standard FS Family vCPUs                     0   100 Count
Standard Av2 Family vCPUs                    0   100 Count
Standard LS Family vCPUs                     0   100 Count
Standard Dv2 Promo Family vCPUs              0   100 Count
Standard DSv2 Promo Family vCPUs             0   100 Count
Standard Dv3 Family vCPUs                    0   100 Count
Standard DSv3 Family vCPUs                   0   100 Count
Standard Ev3 Family vCPUs                    0   100 Count
Standard ESv3 Family vCPUs                   0   100 Count
Standard FSv2 Family vCPUs                   0   100 Count
Standard NCv3 Family vCPUs                   0     0 Count
Standard LSv2 Family vCPUs                   0     0 Count
Standard Storage Managed Disks               2 10000 Count
Premium Storage Managed Disks                1 10000 Count

```

<!--PENDING FOR NC series GA ANOUNCEMENT -->

<!-- Not Available on Standard A8-A11 -->
<!-- Not Available on Standard G, GS -->
<!-- Not Available on Standard NV, NC -->
<!-- Not Available on Standard H, MS -->
<!-- Not Available on Standard ND, NCV2 -->

## <a name="reserved-vm-instances"></a>虚拟机预留实例
虚拟机预留实例对应于单个订阅，会给 vCPU 配额造成新的影响。 这些值描述订阅中必须可以部署的规定大小的实例数。 它们在配额系统中用作占位符，确保预留该配额，以便能够在订阅中部署虚拟机预留实例。 例如，如果特定订阅包含 10 个 Standard_D1 虚拟机预留实例，则 Standard_D1 虚拟机预留实例的用量限制将是 10。 这样，Azure 便可以确保“区域 vCPU 总数”配额中始终至少有 10 个 vCPU 可用于 Standard_D1 实例，并且“标准 D 系列 vCPU”配额中至少有 10 个 vCPU 可用于 Standard_D1 实例。

如果需要增加配额来购买单个订阅 RI，则可以在订阅上[请求增加配额](https://support.windowsazure.cn/support/support-azure)。

## <a name="next-steps"></a>后续步骤

有关计费和配额的详细信息，请参阅 [Azure 订阅和服务限制、配额与约束](/azure-subscription-service-limits?toc=/azure/billing/TOC.json)。
<!--Update_Description: update meta properties, wording update-->
