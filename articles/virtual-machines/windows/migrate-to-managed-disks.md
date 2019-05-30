---
title: 将 Azure VM 迁移到托管磁盘 | Azure
description: 迁移使用存储帐户中的非托管磁盘创建的 Azure 虚拟机以使用托管磁盘。
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 01/03/2018
ms.date: 05/20/2019
ms.author: v-yeche
ms.subservice: disks
ms.openlocfilehash: 9a0c9b81ab2fe71fc97038a0a5ee5977e9c8f24f
ms.sourcegitcommit: bf4afcef846cc82005f06e6dfe8dd3b00f9d49f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2019
ms.locfileid: "66004194"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>将 Azure VM 迁移到 Azure 中的托管磁盘

Azure 托管磁盘无需单独管理存储帐户，从而简化了存储管理。  还可以将现有的 Azure VM 迁移到托管磁盘，以便受益于可用性集中 VM 的更佳可靠性。 这可确保一个可用性集中不同 VM 的磁盘可充分地彼此隔离，避免出现单一故障点。 它会自动将可用性集中不同 VM 的磁盘置于不同的存储缩放单元（戳），限制由于硬件和软件故障引起的单个存储缩放单元故障影响。
可根据需要，从存储选项的四种类型中进行选择。 若要了解可用的磁盘类型，请参阅[选择磁盘类型](disks-types.md)一文

## <a name="migrate-scenarios"></a>迁移方案

可以在以下方案中迁移到托管磁盘：

| **迁移...**                                            | **文档链接**                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 将可用性集中的独立 VM 和多个 VM 转换为托管磁盘   | [转换 VM 以使用托管磁盘](convert-unmanaged-to-managed-disks.md) |
| 将托管磁盘上的单个 VM 从经典部署模型迁移到 Resource Manager 部署模型     | [从经典 VHD 创建 VM](create-vm-specialized-portal.md)  | 
| 将托管磁盘上的所有 VM 从经典部署模型迁移到 Resource Manager 部署模型     | [将 IaaS 资源从经典迁移到 Resource Manager](migration-classic-resource-manager-ps.md)，然后[将 VM 从非托管磁盘转换为托管磁盘](convert-unmanaged-to-managed-disks.md) | 

## <a name="next-steps"></a>后续步骤

- 详细了解[托管磁盘](managed-disks-overview.md)
- 查看[托管磁盘定价](https://www.azure.cn/pricing/details/storage/)。

<!-- Update_Description: update meta properties， update link -->