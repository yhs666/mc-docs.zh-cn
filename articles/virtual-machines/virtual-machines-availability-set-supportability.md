---
title: 将 Azure VM 添加到现有可用性集的可支持性 | Azure
description: 将 Azure VM 添加到现有可用性集的可支持性。
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
origin.date: 11/03/2017
ms.date: 4/10/2018
ms.author: v-yeche
ms.openlocfilehash: a0a57b81560cec283d53bb98daabdd4f31341bb6
ms.sourcegitcommit: ffb8b1527965bb93e96f3e325facb1570312db82
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2018
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>将 Azure VM 添加到现有可用性集的可支持性

将新的虚拟机 (VM) 添加到现有可用性集时，可能偶尔会遇到限制。 以下图表详细说明了可以在同一可用性集中混合的 VM 系列。

以下是混合不同类型的 VM 的可支持性矩阵：

<!--PENDING FOR Dv3 GA ANOUNCEMENT -->
系列和可用性集|第二个 VM|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|第一个 VM|||||||
|A||OK|OK|OK|OK|OK|
|Av2||OK|OK|OK|OK|OK|
|D||OK|OK|OK|OK|OK|
|Dv2||OK|OK|OK|OK|OK|
|Dv3||OK|OK|OK|OK|OK|
<!-- Checking the last Row and Coloumn in the above matrix -->
<!--PENDING FOR Dv3 GA ANOUNCEMENT -->

所有其他系列都不能在同一可用性集中，因为它们需要特定的硬件。

<!--Not Available on A8/A9 -->

<!--Update_Description: update meta properties -->
