---
title: Azure 托管磁盘简介 | Azure
description: 概述了 Azure 托管磁盘，该磁盘在使用 Linux VM 时处理存储帐户
services: virtual-machines-linux,storage
author: rockboyfor
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
origin.date: 02/11/2019
ms.date: 04/01/2019
ms.author: v-yeche
ms.subservice: disks
ms.openlocfilehash: d64446b1b71ef2668929591650d4ee0c0e33626b
ms.sourcegitcommit: 3b05a8982213653ee498806dc9d0eb8be7e70562
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2019
ms.locfileid: "59003988"
---
# <a name="introduction-to-azure-managed-disks"></a>Azure 托管磁盘简介

Azure 托管磁盘是虚拟硬盘 (VHD)。 可以将其视为本地服务器中的物理磁盘，但它是虚拟化的。 Azure 托管磁盘作为页 blob 存储，后者是 Azure 中的随机 IO 存储对象。 我们之所以将托管磁盘称为“托管”是因为，它是对页 blob、blob 容器和 Azure 存储帐户的抽象。 对于托管磁盘，你所要做的就是预配磁盘，而 Azure 负责其余的工作。

如果选择将 Azure 托管磁盘与工作负荷配合使用，Azure 将为你创建和管理该磁盘。 可用的磁盘类型包括高级固态硬盘 (SSD)、标准 SSD 和标准硬盘驱动器 (HDD)。 有关每种磁盘类型的详细信息，请参阅[选择适用于 IaaS VM 的磁盘类型](disks-types.md)。

<!--Not Available on ultra disks (Preview)-->

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [选择适用于 IaaS VM 的磁盘类型](disks-types.md)

<!--Update_Description: update meta properties, wording update -->