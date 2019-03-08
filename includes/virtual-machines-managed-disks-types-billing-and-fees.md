---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
original.date: 01/22/2019
ms.date: 03/04/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: ad6e7eabc531b10eb96d234019138be13b91291d
ms.sourcegitcommit: f1ecc209500946d4f185ed0d748615d14d4152a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463718"
---
**出站数据传输**：[出站数据传输](https://www.azure.cn/zh-cn/pricing/details/data-transfer/)（传出 Azure 数据中心的数据）会产生带宽使用费。

<!--Not Available on **Transactions**: -->

标准 SSD 使用大小为 256 KB 的 IO 单位。 如果要传输的数据小于 256 KB，该数据会被视为 1 个 I/O 单位。 更大的 I/O 大小被视为多个 256 KB 大小的 I/O。 例如，1,100 KB I/O 会被视为 5 个 I/O 单位。

<!--Not Available on transactions for a premium managed disk.-->

有关托管磁盘的详细定价信息，请参阅[托管磁盘定价](https://www.azure.cn/zh-cn/pricing/details/managed-disks)。

<!--Not Available on ### Ultra SSD VM reservation fee-->