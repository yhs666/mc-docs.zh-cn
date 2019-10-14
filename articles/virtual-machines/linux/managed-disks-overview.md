---
title: Azure 托管磁盘简介 | Azure
description: 概述了 Azure 托管磁盘，该磁盘在使用 Linux VM 时处理存储帐户
author: rockboyfor
ms.service: virtual-machines-linux
ms.topic: overview
origin.date: 08/15/2019
ms.date: 10/14/2019
ms.author: v-yeche
ms.subservice: disks
ms.openlocfilehash: 73c8bd7dda887e474490329817efdfa6796c1762
ms.sourcegitcommit: c9398f89b1bb6ff0051870159faf8d335afedab3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72272829"
---
# <a name="introduction-to-azure-managed-disks"></a>Azure 托管磁盘简介

Azure 托管磁盘是虚拟硬盘 (VHD)。 可以将其视为本地服务器中的物理磁盘，但它是虚拟化的。 Azure 托管磁盘作为页 blob 存储，后者是 Azure 中的随机 IO 存储对象。 我们之所以将托管磁盘称为“托管”是因为，它是对页 blob、blob 容器和 Azure 存储帐户的抽象。 对于托管磁盘，你所要做的就是预配磁盘，而 Azure 负责其余的工作。

如果选择将 Azure 托管磁盘与工作负荷配合使用，Azure 将为你创建和管理该磁盘。 可用的磁盘类型包括高级固态硬盘 (SSD)、标准 SSD 和标准硬盘驱动器 (HDD)。 有关每种磁盘类型的详细信息，请参阅[选择适用于 IaaS VM 的磁盘类型](disks-types.md)。

<!--Not Available on ultra disks -->

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [选择适用于 IaaS VM 的磁盘类型](disks-types.md)

<!--Update_Description: update meta properties, wording update -->