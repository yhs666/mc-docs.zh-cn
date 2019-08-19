---
title: Azure 高级存储：针对 Windows VM 的性能进行设计 | Azure
description: 使用 Azure 高级存储设置高性能应用程序。 高级存储为 Azure 虚拟机上运行的 I/O 密集型工作负载提供高性能、低延迟的磁盘支持。
services: virtual-machines-windows,storage
author: rockboyfor
ms.service: virtual-machines-windows
ms.tgt_pltfrm: na
ms.topic: article
origin.date: 06/27/2017
ms.date: 08/12/2019
ms.author: v-yeche
ms.subservice: disks
ms.openlocfilehash: 3a7f8a791267b825f54755a2d1c828eb1fb71b5b
ms.sourcegitcommit: d624f006b024131ced8569c62a94494931d66af7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69539166"
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