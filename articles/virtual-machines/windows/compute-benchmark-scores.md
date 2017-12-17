---
title: "Windows VM 的计算基准测试分数 | Azure"
description: "比较运行 Windows Server 的 Azure VM 的 SPECint 计算基准测试分数"
services: virtual-machines-windows
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 69ae72ec-e8be-4e46-a8f0-e744aebb5cc2
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
origin.date: 05/11/2017
ms.date: 12/18/2017
ms.author: v-yeche
ms.openlocfilehash: 3921c275c455425c62b9990601c7a385b7cf4891
ms.sourcegitcommit: 408c328a2e933120eafb2b31dea8ad1b15dbcaac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2017
---
# <a name="compute-benchmark-scores-for-windows-vms"></a>Windows VM 的计算基准测试分数
以下 SPECInt 基准测试分数显示运行 Windows Server 的 Azure 高性能 VM 产品阵容的计算性能。 此外，还提供了 [Linux VM](../linux/compute-benchmark-scores.md?toc=%2fvirtual-machines%2flinux%2ftoc.json) 的计算基准测试分数。
<!-- Not Available A8-A11 ## A-series - compute-intensive -->

## <a name="dv2-series"></a>Dv2 系列
| 大小 | vCPU | NUMA 节点 | CPU | 运行次数 | 平均基本速率 | 标准偏差 |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D1_v2 |1 |1 |Intel Xeon E5-2673 v3 @ 2.4 GHz |83 |36.6 |2.6 |
| Standard_D2_v2 |2 |1 |Intel Xeon E5-2673 v3 @ 2.4 GHz |27 |70.0 |3.7 |
| Standard_D3_v2 |4 |1 |Intel Xeon E5-2673 v3 @ 2.4 GHz |19 |130.5 |4.4 |
| Standard_D4_v2 |8 |1 |Intel Xeon E5-2673 v3 @ 2.4 GHz |19 |238.1 |5.2 |
| Standard_D5_v2 |16 |2 |Intel Xeon E5-2673 v3 @ 2.4 GHz |14 |460.9 |15.4 |
| Standard_D11_v2 |2 |1 |Intel Xeon E5-2673 v3 @ 2.4 GHz |19 |70.1 |3.7 |
| Standard_D12_v2 |4 |1 |Intel Xeon E5-2673 v3 @ 2.4 GHz |2 |132.0 |1.4 |
| Standard_D13_v2 |8 |1 |Intel Xeon E5-2673 v3 @ 2.4 GHz |17 |235.8 |3.8 |
| Standard_D14_v2 |16 |2 |Intel Xeon E5-2673 v3 @ 2.4 GHz |15 |460.8 |6.5 |

## <a name="about-specint"></a>关于 SPECint
Windows 分数是通过在 Windows Server 上运行 [SPECint 2006](https://www.spec.org/cpu2006/results/rint2006.html) 计算得出的。 SPECint 是使用基本速率选项 (SPECint_rate2006) 运行的，每个 vCPU 一个副本。 SPECint 包括 12 项单独的测试，每项测试运行三次，取每次测试的中间值并为值加权，形成综合分数。 然后跨多个 VM 运行这些测试，提供所示的平均分。

## <a name="next-steps"></a>后续步骤
* 有关存储容量、磁盘详细信息以及选择 VM 大小的注意事项，请参阅 [Sizes for virtual machines](sizes.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)（虚拟机的大小）。
<!-- Update_Description: wording update -->