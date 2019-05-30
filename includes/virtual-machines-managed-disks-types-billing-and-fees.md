---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 01/22/2019
ms.date: 05/20/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: ddf2661bf82ca0121bd7e405842b0f5dee6582cf
ms.sourcegitcommit: bf4afcef846cc82005f06e6dfe8dd3b00f9d49f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66039221"
---
**出站数据传输**：[出站数据传输](https://www.azure.cn/pricing/details/data-transfer/)（传出 Azure 数据中心的数据）会产生带宽使用费。

**事务**：会根据你对标准托管磁盘执行的事务数向你收费。 对于标准 SSD，将使用 I/O 单元大小 256 KiB 来计算事务数。 更大的 I/O 大小被视为多个 256 KiB 大小的 I/O。 对于标准 HDD，每个 IO 操作会被视为单个事务，无论 I/O 大小如何。

有关托管磁盘定价的详细信息（包括事务成本），请参阅[托管磁盘定价](https://www.azure.cn/zh-cn/pricing/details/storage/managed-disks/)。

<!--Not Available on ### Ultra SSD VM reservation fee-->