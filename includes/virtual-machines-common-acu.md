---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 03/09/2018
ms.date: 04/16/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: b21902a80cd08e937e6967ebb36fe1fe4523f165
ms.sourcegitcommit: 00c8a6a07e6b98a2b6f2f0e8ca4090853bb34b14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2018
ms.locfileid: "38944644"
---
<!-- Pending on Dv3 and Ev3 series on Lanch--> Azure 计算单位 (ACU) 这一概念提供了一种比较 Azure SKU 的计算 (CPU) 性能的方法。 这有助于轻松确定最有可能满足性能需求的 SKU。  ACU 目前在小型 (Standard_A1) VM 上标准为 100，而所有其他 SKU 表示 SKU 在运行标准基准测试时大约可以有多快。 

> [!IMPORTANT]
> ACU 只是一种规则。  工作负荷的结果可能会有所不同。 
> 
> 

<br>

| SKU 系列 | ACU\vCPU | vCPU：核心 |
| --- | --- |---|
| [A0](../articles/virtual-machines/windows/sizes-general.md) |50 | 1:1 |
| [A1-A4](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A5-A7](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A1_v2-A8_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A2m_v2-A8m_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
<!-- Not Available  [A8-A11]  -->
| [D1-D14](../articles/virtual-machines/windows/sizes-general.md) |160 | 1:1 | | [D1_v2-D15_v2](../articles/virtual-machines/windows/sizes-general.md) |210 - 250* | 1:1 | | [DS1-DS14](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 | 1:1 | | [DS1_v2-DS15_v2](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |210-250* | 1:1 | <!-- Pending on Dv3 and Ev3 series on Lanch-->
| [D_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160-190* | 2:1** | | [Ds_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160-190* | 2:1** | | [E_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160-190* | 2:1** | | [Es_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160-190* | 2:1** | <!-- Pending on Dv3 and Ev3 series on Lanch -->
| [F2s_v2-F72s_v2](../articles/virtual-machines/windows/sizes-compute.md) |195-210* | 2:1** | | [F1-F16](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | 1:1 | | [F1s-F16s](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | 1:1 |

<!-- Not Available  [G1-G5]  -->
<!-- Not Available  [GS1-GS5]  -->
<!-- Not Available  [H] -->
<!-- Not Available  [L4s-L32s]  -->
<!-- Not Available  [M] -->

ACU 标有 *使用 Intel® Turbo 技术来增加 CPU 频率，并提升性能。  提升量可能因 VM 大小、工作负荷和同一主机上运行的其他工作负荷而有所不同。

**超线程。

<!--Update_Description: add Dv3 and Ev3 configuration(Pending)-->