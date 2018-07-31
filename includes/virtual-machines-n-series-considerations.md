---
title: include 文件
description: include 文件
services: virtual-machines-linux
author: rockboyfor
ms.service: virtual-machines-linux
ms.topic: include
origin.date: 06/19/2018
ms.date: 07/30/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: ed99e6bae5ea1029d340bb8e9cf4f04f12c9dbad
ms.sourcegitcommit: 720d22231ec4b69082ca03ac0f400c983cb03aa1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/27/2018
ms.locfileid: "39307090"
---
## <a name="deployment-considerations"></a>部署注意事项

* 有关 N 系列 VM 的可用性，请查看[可用产品(按区域)](https://www.azure.cn/support/service-dashboard/)。

* N 系列 VM 只能按 Resource Manager 部署模型部署。

* N 系列的 VM 在对其磁盘支持的 Azure 存储类型方面有所不同。 NC 和 NV VM 仅支持标准磁盘存储 (HDD) 所支持的 VM 磁盘。 NCv2、ND 和 NCv3 VM 仅支持高级磁盘存储 (SSD) 所支持的 VM 磁盘。

* 如果需要部署的 N 系列 VM 较多，请考虑使用即用即付订阅或其他购买选项。 如果使用的是 [Azure 试用帐户](https://www.azure.cn/pricing/1rmb-trial/)，则仅可使用有限数量的 Azure 计算核心。

* 可能需要提高 Azure 订阅中的核心配额（按区域）以及单独针对 NC、NCv2、NCv3、ND 或 NV 核心的配额。 若要请求提高配额，可免费[提出在线客户支持请求](https://www.azure.cn/support/support-ticket-form/?l=zh-cn)。 默认限制可能因订阅类别而异。