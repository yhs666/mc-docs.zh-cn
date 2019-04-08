---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 01/22/2019
ms.date: 04/01/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 392dd7a0fcb600f3459301243fae852e583d50e6
ms.sourcegitcommit: 3b05a8982213653ee498806dc9d0eb8be7e70562
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2019
ms.locfileid: "59004540"
---
# <a name="what-disk-types-are-available-in-azure"></a>Azure 有哪些可用的磁盘类型？

Azure 托管磁盘当前提供了三种磁盘类型，它们都已公开发布 (GA)。 这三种磁盘类型的每一种都有自己的相应目标客户方案。

## <a name="disk-comparison"></a>磁盘比较

下表对托管磁盘的高级 SSD、标准 SSD 和标准硬盘驱动器 (HDD) 进行了比较，方便你确定使用哪一种。

<!--Not Available on ultra solid-state-drives (SSD) (preview)-->

|    | 高级·SSD   | 标准 SSD   | 标准 HDD   |
|---------|---------|---------|---------|
|磁盘类型   |SSD   |SSD   |HDD   |
|方案   |生产和性能敏感型工作负荷   |Web 服务器、不常使用的企业应用程序和开发/测试   |备份、非关键、不常访问   |
|磁盘大小   |4,095 GiB     |4,095 GiB   |4,095 GiB   |
|最大吞吐量   |900 MiB/秒   |750 MiB/秒   |500 MiB/秒   |
|最大 IOPS   |20,000   |6,000   |2,000   |

<!--MOONCAKE: Disk size is less than 4095 GiB-->
<!--Not Available on## Ultra SSD (preview)-->
