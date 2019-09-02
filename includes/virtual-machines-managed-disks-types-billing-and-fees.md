---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 01/22/2019
ms.date: 07/01/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 8a12783fdb5381c7a30e3cc5b094b47312b67971
ms.sourcegitcommit: c61b10764d533c32d56bcfcb4286ed0fb2bdbfea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68332795"
---
**出站数据传输**：[出站数据传输](https://www.azure.cn/pricing/details/data-transfer/)（传出 Azure 数据中心的数据）会产生带宽使用费。

**事务**：会根据你对标准托管磁盘执行的事务数向你收费。 对于标准 SSD，每个小于或等于 256 KiB 吞吐量的 I/O 操作被视为单个 I/O 操作。 大于 256 KiB 吞吐量的 I/O 操作被视为大小为 256 KiB 的多个 I/O。 对于标准 HDD，每个 IO 操作会被视为单个事务，无论 I/O 大小如何。

有关托管磁盘定价的详细信息（包括事务成本），请参阅[托管磁盘定价](https://www.azure.cn/zh-cn/pricing/details/storage/managed-disks/)。

<!--MOONCAKE: CORRECT ON https://www.azure.cn/zh-cn/pricing/details/storage/managed-disks/-->

<!--Not Available on ### Ultra SSD VM reservation fee-->