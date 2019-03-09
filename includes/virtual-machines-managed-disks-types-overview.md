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
ms.openlocfilehash: 299237876138c6392fe07f1913b49a58bc00c048
ms.sourcegitcommit: f1ecc209500946d4f185ed0d748615d14d4152a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463708"
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
|磁盘大小   |4,095 GiB (GA)    |4,095 (GA) GiB   |4,095 GiB (GA)   |
|最大吞吐量   |250 (GA) MiB/秒   |60 MiB/秒 (GA)   |60 Mib/秒 (GA)   |
|最大 IOPS   |7500 (GA)   |500 (GA)   |500 (GA)   |

<!--Not Available on## Ultra SSD (preview)--》


## Premium SSD

Azure premium SSDs deliver high-performance and low-latency disk support for virtual machines (VMs) with input/output (IO)-intensive workloads. To take advantage of the speed and performance of premium storage disks, you can migrate existing VM disks to Premium SSDs. Premium SSDs are suitable for mission-critical production applications.

### Disk size

Sizes marked with an asterisk are currently in preview.

<!--Not Available on P60*, P70* , P80*-->

| 高级 SSD 大小  | P4               | P6               | P10             | P15 | P20              | P30              | P40              | P50              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| 磁盘大小 (GiB)           | 32             | 64             | 128            | 256  | 512            | 1,024    | 2,048     | 4,095    |
| 每个磁盘的 IOPS       | 最多 120 | 最多 240              | 最大 500              | 最多 1,100 | 最多 2,300              | 最多 5,000              | 最多 7,500             | 最多 7,500              |
| 每个磁盘的吞吐量 | 最多 25 MiB/秒 | 最多 50 MiB/秒 | 最多 100 MiB/秒 | 最多 125 MiB/秒 | 最多 150 MiB/秒 | 最多 200 MiB/秒 | 最多 250 MiB/秒 | 最多 250 MiB/秒|

<!--Not Available on P60*, P70* , P80*-->

## <a name="standard-ssd"></a>标准 SSD

Azure 标准 SSD 是经济高效的存储选项，已针对需要一致性能和较低 IOPS 级别的工作负荷进行优化。 对于想要迁移到云的用户而言，标准 SSD 提供良好的入门级体验，尤其是在本地 HDD 解决方案中运行各种工作负荷遇到问题时。 与 HDD 磁盘相比，标准 SSD 提供更好的可用性、一致性、可靠性和延迟。 标准 SSD 适用于 Web 服务器、低 IOPS 应用程序服务器、较少使用的企业应用程序和开发/测试工作负荷。

### <a name="disk-size"></a>磁盘大小

用星号标记的大小当前处于预览阶段。

<!--Not Available on E60*, E70*, E80*-->

| 标准 SSD 大小  | E10               | E15               | E20             | E30 | E40              | E50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|
| 磁盘大小 (GiB)           | 128             | 256             | 512            | 1,024  | 2,048            | 4,095     |
| 每个磁盘的 IOPS       | 最大 500              | 最大 500              | 最大 500              | 最大 500 | 最大 500              | 最大 500              | 最大 500             | 最大 500              |
| 每个磁盘的吞吐量 | 最多 60 MiB/秒 | 最多 60 MiB/秒 | 最多 60 MiB/秒 | 最多 60 MiB/秒 | 最多 60 MiB/秒 | 最多 60 MiB/秒 | 最多 60 MiB/秒 | 最多 60 MiB/秒|

<!--Not Available on E60*, E70*, E80*-->

## <a name="standard-hdd"></a>标准 HDD

Azure 标准 HDD 为运行不区分延迟的工作负荷提供可靠、低成本的磁盘支持。 它还支持 Blob、表、队列和文件。 使用标准存储，将数据存储在硬盘驱动器 (HDD)。 使用 VM 时，可将标准 SSD 和 HDD 磁盘用于开发/测试方案和不太重要的工作负荷。 所有 Azure 区域均提供标准存储。

### <a name="disk-size"></a>磁盘大小

用星号标记的大小当前处于预览阶段。

<!--Not Available on S60*, S70*, S80*-->

| 标准磁盘类型  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| 磁盘大小 (GiB)          | 32             | 64             | 128            | 256  | 512            | 1,024    | 2,048     | 4,095    |
| 每个磁盘的 IOPS       | 最大 500              | 最大 500              | 最大 500              | 最大 500 | 最大 500              | 最大 500              | 最大 500             | 最大 500              |
| 每个磁盘的吞吐量 | 最多 60 MiB/秒 | 最多 60 MiB/秒 | 最多 60 MiB/秒 | 最多 60 MiB/秒 | 最多 60 MiB/秒 | 最多 60 MiB/秒 | 最多 60 MiB/秒 | 最多 60 MiB/秒|

<!--Not Available on S60*, S70*, S80*-->

## <a name="billing"></a>计费

使用托管磁盘时，将考虑以下计费事项：

- 磁盘类型
- 托管磁盘大小
- 快照
- 出站数据传输
- 事务数

**托管磁盘大小**：托管磁盘按预配大小计费。 Azure 将预配大小映射（向上舍入）到所提供的最接近的磁盘大小。 有关所提供的磁盘大小的详细信息，请参阅前面的表。 每个磁盘将映射到一种受支持的预配磁盘大小套餐并相应地计费。 例如，如果预配了 200 GiB 的标准 SSD，它会映射到 E15 的磁盘大小 (256 GiB)。 任何预配的磁盘根据每月的高级存储优惠价格按小时计费。 例如，如果在预配 E10 磁盘的 20 小时后删除它，则会以 20 小时计算 E10 产品/服务的费用。 这与写入磁盘的实际数据量无关。

**快照**：基于已使用大小对快照计费。 例如，如果创建预配容量为 64 GiB 且实际使用数据大小为 10 GiB 的托管磁盘的快照，则仅针对已用数据大小 10 GiB 对该快照计费。
