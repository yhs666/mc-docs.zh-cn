<!-- not suitable for Mooncake -->

---
title: 从 AWS 和其他平台迁移到 Azure 中的托管磁盘 | Azure
description: 在 Azure 中使用从其他云（如 AWS 或其他虚拟化平台）上载的 VHD 并利用 Azure 托管磁盘创建 VM。
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-resource-manager

ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
wacn.date: 03/20/2017
ms.author: cynthn
---

# 从 AWS 和其他平台迁移到 Azure 中的托管磁盘

可以从 AWS 或本地虚拟化解决方案将 VHD 文件上载到 Azure，以创建利用托管磁盘的 VM。Azure 托管磁盘不需要为 Azure IaaS VM 管理存储帐户。仅需指定类型（高级或标准）以及所需的磁盘大小，Azure 将为你创建和管理磁盘。

可以上载通用和专用 VHD。**通用 VHD** - 通用 VHD 包含使用 Sysprep 删除所有个人帐户信息。**专用 VHD** - 专用 VHD 保留原始 VM 中的用户帐户、应用程序和其他状态数据。

> [!IMPORTANT]
将任何 VHD 上载到 Azure 之前，应该按照[准备要上载到 Azure 的 Windows VHD 或 VHDX](../virtual-machines-windows-prepare-for-upload-vhd-image.md) 中的步骤操作
>
>

| 方案 | 文档 |
|----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| 你有一个现有的 AWS EC2 实例，你想要将它迁移到 Azure 托管磁盘 | [从 Amazon Web Services (AWS) 迁移到 Azure 托管磁盘](../virtual-machines-windows-aws-to-azure.md) |
| 你有一个来自任何其他虚拟化平台的 VM，你想要将该 VM 作为映像使用，以便创建多个 Azure VM。 | [将通用 VHD 上载到 Azure 并使用托管磁盘创建新 VM](../virtual-machines-windows-upload-generalized-managed.md) |
| 你有一个唯一自定义 VM，你想要在 Azure 中重新创建它。 | [将专用 VHD 上载到 Azure 并使用托管磁盘创建新 VM](../virtual-machines-windows-upload-specialized.md) |

## 托管磁盘概述

Azure 托管磁盘无需管理存储帐户，从而简化了 VM 管理。托管磁盘还受益于可用性集中 VM 的更佳可靠性。它可确保可用性集中不同 VM 的磁盘完全相互隔离，以避免出现单点故障。它会自动将可用性集中不同 VM 的磁盘放置到不同存储扩展单元 (stamp) 中，以限制因硬件和软件故障而导致的单个存储扩展单元故障的影响。根据需求，可以从两种类型的存储选项中进行选择：

- [高级托管磁盘](../../storage/storage-premium-storage.md)是基于固态硬盘 (SSD) 的存储媒体，为运行 I/O 密集型工作负荷的虚拟机提供高性能、低延迟磁盘支持。可以通过迁移到高级托管磁盘，充分利用这些磁盘的速度和性能。

- [标准托管磁盘](../../storage/storage-standard-storage.md)使用基于硬盘驱动器 (HDD) 的存储媒体，并最适合对性能变化不太敏感的开发/测试和其他很少访问的工作负荷。

## 迁移到托管磁盘的计划

本部分可帮助你针对 VM 和磁盘类型做出最佳决策。

### 位置

选取 Azure 托管磁盘可用位置。如果要迁移到高级托管磁盘，还应确保高级存储在计划迁移到的区域中可用。有关可用位置的最新信息，请参阅 [Azure 服务（按区域）](https://azure.microsoft.com/regions/#services)。

### VM 大小

如果要迁移到高级托管磁盘，需要将 VM 的大小更新为该 VM 所在区域中支持高级存储的可用大小。查看支持高级存储的 VM 大小。[虚拟机大小](../virtual-machines-windows-sizes.md)中列出了 Azure VM 大小规范。查看适用于高级存储的虚拟机的性能特征并选择最适合工作负荷的 VM 大小。确保 VM 上有足够的带宽来驱动磁盘通信。

### 磁盘大小

**高级托管磁盘**

有 3 种类型的高级托管磁盘可用于 VM，每种磁盘都具有特定的 IOP 和吞吐率限制。根据应用程序在容量、性能、可伸缩性和峰值负载方面的需要为 VM 选择高级磁盘类型时，需要考虑这些限制。

| 高级磁盘类型 | P10 | P20 | P30 |
|---------------------|-------------------|-------------------|-------------------|
| 磁盘大小 | 128 GB | 512 GB | 1024 GB (1 TB) |
| 每个磁盘的 IOPS | 500 | 2300 | 5000 |
| 每个磁盘的吞吐量 | 每秒 100 MB | 每秒 150 MB | 每秒 200 MB |

**标准托管磁盘**

有 5 种类型的标准托管磁盘可用于 VM。其中每种磁盘都具有不同的容量，但具有相同的 IOPS 和吞吐量限制。根据应用程序的容量要求，选择标准托管磁盘的类型。

| 标准磁盘类型 | S4 | S6 | S10 | S20 | S30 |
|---------------------|------------------|------------------|------------------|------------------|------------------|
| 磁盘大小 | 30 GB | 64 GB | 128 GB | 512 GB | 1024 GB (1 TB) |
| 每个磁盘的 IOPS | 500 | 500 | 500 | 500 | 500 |
| 每个磁盘的吞吐量 | 每秒 60 MB | 每秒 60 MB | 每秒 60 MB | 每秒 60 MB | 每秒 60 MB |

### 磁盘缓存策略 

**高级托管磁盘**

默认情况下，所有高级数据磁盘的磁盘缓存策略都是“只读的”，所有附加到 VM 的高级操作系统都是“读写的”。要使应用程序的 IO 达到最佳性能，建议使用此配置设置。对于频繁写入或只写的磁盘（例如 SQL Server 日志文件），禁用磁盘缓存可获得更佳的应用程序性能。

### 定价

查看[托管磁盘定价](https://azure.microsoft.com/pricing/details/managed-disks/)。高级托管磁盘的定价与高级非托管磁盘相同。但是，标准托管磁盘的定价不同于标准非托管磁盘。

## 后续步骤

- 将任何 VHD 上载到 Azure 之前，应该按照[准备要上载到 Azure 的 Windows VHD 或 VHDX](../virtual-machines-windows-prepare-for-upload-vhd-image.md) 中的步骤操作

<!---HONumber=Mooncake_0313_2017-->