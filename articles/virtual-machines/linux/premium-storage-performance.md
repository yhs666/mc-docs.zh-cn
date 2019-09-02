---
title: Azure 高级存储 - 高性能设计 | Azure
description: 使用 Azure 高级存储设置高性能应用程序。 高级存储为 Azure 虚拟机上运行的 I/O 密集型工作负载提供高性能、低延迟的磁盘支持。
services: virtual-machines-linux,storage
author: rockboyfor
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.topic: article
origin.date: 06/27/2017
ms.date: 08/12/2019
ms.author: v-yeche
ms.subservice: disks
ms.openlocfilehash: a1df797f5fa63f4521d9e768f9f21537a0079ff4
ms.sourcegitcommit: 8ac3d22ed9be821c51ee26e786894bf5a8736bfc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68912958"
---
[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> 有时，显示为磁盘性能问题的原因实际上是网络瓶颈。 在这些情况下，应优化[网络性能](../../virtual-network/virtual-network-optimize-network-bandwidth.md)。
>
> 如果你希望对磁盘进行基准测试，请参阅我们的关于[磁盘基准测试](disks-benchmarks.md)的文章。
>
> 如果 VM 支持加速网络，则应确保它已启用。 如果未启用，则可以在 [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) 和 [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms) 上已部署的 VM 上启用它。

如果不熟悉高级存储，请在开始之前首先阅读[为 IaaS VM 选择 Azure 磁盘类型](disks-types.md)和[存储帐户的 Azure 存储可伸缩性和性能目标](../../storage/common/storage-scalability-targets.md)。

[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]

<!-- Update_Description: update meta properties -->