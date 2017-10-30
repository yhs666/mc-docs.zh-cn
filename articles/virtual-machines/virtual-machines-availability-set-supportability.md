---
title: "将 Azure VM 添加到现有可用性集的可支持性 | Azure"
description: "将 Azure VM 添加到现有可用性集的可支持性。"
services: virtual-machines-linux
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
origin.date: 07/15/2017
ms.date: 10/30/2017
ms.author: v-yeche
ms.openlocfilehash: 711c2484c1685a65415f74faf64b89a60524a943
ms.sourcegitcommit: da3265de286410af170183dd1804d1f08f33e01e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>将 Azure VM 添加到现有可用性集的可支持性

将新的虚拟机 (VM) 添加到现有可用性集时，可能偶尔会遇到限制。 以下图表详细说明了可以在同一可用性集中混合的 VM 系列。

以下是混合不同类型的 VM 的可支持性矩阵：

系列和可用性集|第二个 VM|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|第一个 VM|||||||
|A||OK|OK|OK|OK|OK|
|Av2||OK|OK|OK|OK|OK|
|D||OK|OK|OK|OK|OK|
|Dv2||OK|OK|OK|OK|OK|
|Dv3||OK|OK|OK|OK|OK|

所有其他系列都不能在同一可用性集中，因为它们需要特定的硬件。

<!--Not Available on A8/A9 -->

<!--Update_Description: remove A8/A9 due to unsuitable to azure.cn-->
