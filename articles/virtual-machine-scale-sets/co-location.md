---
title: 归置 Azure 虚拟机规模集 | Microsoft Docs
description: 了解归置 Azure 虚拟机规模集资源如何提高性能。
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
origin.date: 05/14/2019
ms.date: 08/06/2019
ms.author: v-junlch
ms.openlocfilehash: 84ea1b4580c3aa4c30465123eb1cf1bf89d8d45d
ms.sourcegitcommit: 17cd5461e7d99f40b9b1fc5f1d579f82b2e27be9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819539"
---
# <a name="co-location"></a>归置

导致 VM 之间延迟的最大因素之一就是距离。

## <a name="preview-proximity-placement-groups"></a>预览版：邻近放置组 

[!INCLUDE [virtual-machines-common-ppg-overview](../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>后续步骤

为规模集创建[邻近放置组](proximity-placement-groups.md)。


