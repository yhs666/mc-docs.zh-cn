---
title: include 文件
description: include 文件
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
origin.date: 05/17/2018
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: 9b6d13f029cdf56ae750b5d92a3666c4d53b56d6
ms.sourcegitcommit: 0bfa3c800b03216b89c0461e0fdaad0630200b2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2019
ms.locfileid: "72526632"
---
### <a name="virtual-machine-scale-sets"></a>虚拟机规模集

|  |  |
|---------|---------|
| [在 VM 未使用托管磁盘时审核](../articles/governance/policy/samples/create-vm-managed-disk.md) | 如果创建了未使用托管磁盘的虚拟机，则进行审核。|
| [使用托管磁盘创建 VM](../articles/governance/policy/samples/use-managed-disk-vm.md) | 要求虚拟机使用托管磁盘。|
| [拒绝混合使用权益](../articles/governance/policy/samples/deny-hybrid-use.md) | 禁止使用 Azure 混合使用权益 (AHUB)。 在想禁止使用本地许可证时使用。 |
| [只允许特定 VM 平台映像](../articles/governance/policy/samples/allow-certain-vm-image.md) | 要求虚拟机使用 UbuntuServer 的特定版本。 |