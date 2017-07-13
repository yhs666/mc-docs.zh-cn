---
title: "将 Azure VM 迁移到托管磁盘 | Azure"
description: "迁移使用存储帐户中的非托管磁盘创建的 Azure 虚拟机以使用托管磁盘。"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 06/15/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.openlocfilehash: 7c3eefae0e9f6bbfe350b11ca2ea9e110d638996
ms.sourcegitcommit: b3e981fc35408835936113e2e22a0102a2028ca0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# 将 Azure VM 迁移到 Azure 中的托管磁盘
<a id="migrate-azure-vms-to-managed-disks-in-azure" class="xliff"></a>

Azure 托管磁盘无需单独管理存储帐户，从而简化了存储管理。  还可以将现有的 Azure VM 迁移到托管磁盘，以便受益于可用性集中 VM 的更佳可靠性。 它可确保可用性集中不同 VM 的磁盘完全相互隔离，以避免出现单点故障。 它会自动将可用性集中不同 VM 的磁盘置于不同的存储缩放单位（戳），限制由于硬件和软件故障引起的单个存储缩放单位故障影响。
根据需求，可以从两种类型的存储选项中进行选择：

- [高级托管磁盘](../../storage/storage-premium-storage.md)是基于固态硬盘 (SSD) 的存储介质，可为运行 I/O 密集型工作负荷的虚拟机提供高性能、低延迟的磁盘支持。 可以通过迁移到高级托管磁盘，充分利用这些磁盘的速度和性能。

- [标准托管磁盘](../../storage/storage-standard-storage.md)使用基于硬盘驱动器 (HDD) 的存储介质，最适合用于对性能变化不太敏感的开发/测试和其他不频繁的访问工作负荷。

可以在以下方案中迁移到托管磁盘：

| 迁移...                                            | 文档链接                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 将可用性集中的独立 VM 和多个 VM 转换为托管磁盘   | [转换 VM 以使用托管磁盘](convert-unmanaged-to-managed-disks.md) |
| 将托管磁盘上的单个 VM 从经典部署模型迁移到 Resource Manager 部署模型     | [迁移单个 VM](migrate-single-classic-to-resource-manager.md)  | 
| 将托管磁盘上的所有 VM 从经典部署模型迁移到 Resource Manager 部署模型     | [将 IaaS 资源从经典迁移到 Resource Manager](migration-classic-resource-manager-ps.md)，然后[将 VM 从非托管磁盘转换为托管磁盘](convert-unmanaged-to-managed-disks.md) | 

## 转换为托管磁盘的计划
<a id="plan-for-the-conversion-to-managed-disks" class="xliff"></a>

本部分可帮助你针对 VM 和磁盘类型做出最佳决策。

## 位置
<a id="location" class="xliff"></a>

选取 Azure 托管磁盘可用位置。 如果要迁移到高级托管磁盘，还请确保高级存储在计划迁移到的区域中可用。

## VM 大小
<a id="vm-sizes" class="xliff"></a>

如果要迁移到高级托管磁盘，需要将 VM 的大小更新为该 VM 所在区域中支持高级存储的可用大小。 查看支持高级存储的 VM 大小。 [虚拟机大小](sizes.md)中列出了 Azure VM 大小规范。
查看适用于高级存储的虚拟机的性能特征并选择最适合工作负荷的 VM 大小。 确保 VM 上有足够的带宽来驱动磁盘通信。

## 磁盘大小
<a id="disk-sizes" class="xliff"></a>

**高级托管磁盘**

有 7 种类型的高级托管磁盘可用于 VM，每种磁盘都具有特定的 IOPS 和吞吐量限制。 根据应用程序在容量、性能、可伸缩性和峰值负载方面的需要为 VM 选择高级磁盘类型时，需要考虑这些限制。

| 高级磁盘类型  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| 磁盘大小           | 32 GB| 64 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| 每个磁盘的 IOPS       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| 每个磁盘的吞吐量 | 每秒 25 MB  | 每秒 50 MB  | 每秒 100 MB | 每秒 150 MB | 每秒 200 MB | 每秒 250 MB | 每秒 250 MB |

**标准托管磁盘**

有 7 种类型的标准托管磁盘可用于 VM。 其中每种磁盘都具有不同的容量，但具有相同的 IOPS 和吞吐量限制。 根据应用程序的容量要求，选择标准托管磁盘的类型。

| 标准磁盘类型  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| 磁盘大小           | 32 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2048 GB (2 TB)    | 4095 GB (4 TB)   | 
| 每个磁盘的 IOPS       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| 每个磁盘的吞吐量 | 每秒 60 MB | 每秒 60 MB | 每秒 60 MB | 每秒 60 MB | 每秒 60 MB | 每秒 60 MB | 每秒 60 MB | 

## 磁盘缓存策略
<a id="disk-caching-policy" class="xliff"></a>

**高级托管磁盘**

默认情况下，所有高级数据磁盘的磁盘缓存策略都是“只读”，所有附加到 VM 的高级操作系统都是“读写”。 为使应用程序的 IO 达到最佳性能，建议使用此配置设置。 对于频繁写入或只写的磁盘（例如 SQL Server 日志文件），禁用磁盘缓存可获得更佳的应用程序性能。

## 定价
<a id="pricing" class="xliff"></a>

查看[托管磁盘定价](https://www.azure.cn/pricing/details/managed-disks/)。 高级托管磁盘的定价与高级非托管磁盘相同。 但是，标准托管磁盘的定价不同于标准非托管磁盘。

## 后续步骤
<a id="next-steps" class="xliff"></a>

- 详细了解[托管磁盘](../../storage/storage-managed-disks-overview.md)
