---
title: include 文件
description: include 文件
services: azure-policy
author: WenJason
ms.service: azure-policy
ms.topic: include
ms.date: 07/09/2018
origin.date: 05/17/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: e77fb94c8446cc86fa38b88bcdf26d89b7b13b80
ms.sourcegitcommit: 18810626635f601f20550a0e3e494aa44a547f0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2018
ms.locfileid: "37405424"
---
### <a name="virtual-machine-scale-sets"></a>虚拟机规模集

|  |  |
|---------|---------|
| [在 VM 未使用托管磁盘时审核](../articles/azure-policy/scripts/create-vm-managed-disk.md) | 如果创建了未使用托管磁盘的虚拟机，则进行审核。|
| [使用托管磁盘创建 VM](../articles/azure-policy/scripts/use-managed-disk-vm.md) | 要求虚拟机使用托管磁盘。|
| [拒绝混合使用权益](../articles/azure-policy/scripts/deny-hybrid-use.md) | 禁止使用 Azure 混合使用权益 (AHUB)。 想禁止使用本地许可证时使用。 |
| [只允许特定 VM 平台映像](../articles/azure-policy/scripts/allow-certain-vm-image.md) | 要求虚拟机使用 UbuntuServer 的特定版本。 |