---
title: Azure 中的 Linux VM 大小 | Azure
description: 列出 Azure 中 Linux 虚拟机的不同可用大小。
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
origin.date: 05/22/2018
ms.date: 06/25/2018
ms.author: v-yeche
ms.openlocfilehash: 98ac31780fb34b5a2801d0ce5063420ec5ad81c9
ms.sourcegitcommit: 092d9ef3f2509ca2ebbd594e1da4048066af0ee3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "36315386"
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Azure 中 Linux 虚拟机的大小
本文介绍可用于运行 Linux 应用和工作负荷的 Azure 虚拟机的可用大小与选项。 此外，还提供在计划使用这些资源时要考虑的部署注意事项。 本文也适用于 [Windows 虚拟机](../windows/sizes.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。

<!--PENDING FOR B-SERIES, Dv3, Ev3, FsV2 GA ANOUNCEMENT -->
| 类型                     | 大小           |    说明       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [常规用途](sizes-general.md)          | B、Dsv3、Dv3、DSv2、Dv2、DS、D、Av2、A0-7  | CPU 与内存之比平衡。 适用于测试和开发、小到中型数据库和低到中等流量 Web 服务器。 |
| [计算优化](sizes-compute.md)        | Fsv2、Fs、F            | 高 CPU 与内存之比。 适用于中等流量的 Web 服务器、网络设备、批处理和应用程序服务器。        |
| [内存优化](sizes-memory.md)         | Esv3、Ev3、DSv2、DS、Dv2、D   | 高内存与 CPU 之比。 适用于关系数据库服务器、中到大型规模的缓存和内存中分析。                 |
<!--PENDING FOR B-SERIES, Dv3, Ev3, FsV2 GA ANOUNCEMENT -->
<!--Not Available M, GS, G -->
<!--Not Available Ls-->
<!--Not Available NV, NC, NCv2, NCv3, ND-->
<!--Not Available H, A8-11-->

<br>

- 有关不同大小的定价信息，请参阅[虚拟机定价](https://www.azure.cn/pricing/details/virtual-machines/#Linux)。 
- 有关各 Azure 区域中推出的 VM 大小，请查看[可用产品(按区域)](https://www.azure.cn/support/service-dashboard/)。
- 若要查看 Azure VM 的一般限制，请参阅 [Azure 订阅和服务限制、配额与约束](../../azure-subscription-service-limits.md)。
- 了解有关 [Azure 计算单元 (ACU)](acu.md) 如何帮助跨 Azure SKU 比较计算性能的详细信息。

## <a name="rest-api"></a>REST API

有关使用 REST API 来查询 VM 大小的信息，请参阅以下文章：

- [List available virtual machine sizes for resizing](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)（列出可用的虚拟机大小以便调整大小）
- [List available virtual machine sizes for a subscription](https://docs.microsoft.com/rest/api/compute/virtualmachines/listall)（列出订阅的可用虚拟机大小）
- [List available virtual machine sizes in an availability set](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)（列出可用性集中的可用虚拟机大小）

## <a name="acu"></a>ACU

了解有关 [Azure 计算单元 (ACU)](acu.md) 如何帮助跨 Azure SKU 比较计算性能的详细信息。

## <a name="benchmark-scores"></a>基准评分

使用 [CoreMark 基准测试分数](compute-benchmark-scores.md)，详细了解 Linux VM 的计算性能。

## <a name="next-steps"></a>后续步骤

了解关于可用的各种 VM 大小的详细信息：
- [常规用途](sizes-general.md)
- [计算优化](sizes-compute.md)
- [内存优化](sizes-memory.md)
- 查看[上一代](sizes-previous-gen.md)页面，了解 A Standard、Dv1（D1-4 和 D11-14 v1）<!--Not Available on A8-A11 series-->
<!--Not Available- [Storage optimized](sizes-storage.md) -->
<!--Not Available- [GPU](sizes-gpu.md)-->
<!--Not Available- [High performance compute](sizes-hpc.md)-->

<!--Update_Description: update meta properties, wording update, update link -->
<!--PENDING FOR B-SERIES, Dv3, Ev3, FsV2 GA ANOUNCEMENT -->