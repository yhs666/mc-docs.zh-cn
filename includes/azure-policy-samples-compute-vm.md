---
title: include 文件
description: include 文件
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 05/17/2018
ms.date: 11/12/2018
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: a315e5bb249b4364deeebbb96ab87969607a8018
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52658120"
---
### <a name="virtual-machines"></a>虚拟机

|  |  |
|---------|---------|
| [允许自定义资源组中的 VM 映像](../articles/governance/policy/samples/allow-custom-vm-image.md) |  要求自定义映像来自已批准的资源组。 指定已批准的资源组的名称。 |
| [允许的存储帐户和虚拟网络的 SKU](../articles/governance/policy/samples/allowed-skus-storage.md) | 要求存储帐户和虚拟机使用已批准的 SKU。 使用内置策略以确保使用已批准的 SKU。 指定已批准的虚拟机 SKU 数组和已批准的存储帐户 SKU 数组。 |
| [已批准的 VM 映像](../articles/governance/policy/samples/allowed-custom-images.md) | 要求在环境中仅部署已批准的自定义映像。 指定已批准的映像 ID 的数组。 |
| [如果扩展不存在，则进行审核](../articles/governance/policy/samples/audit-ext-not-exist.md) | 如果扩展不是通过虚拟机部署的，则进行审核。 指定扩展发布者和类型以检查是否已部署扩展。 |
| [不允许使用 VM 扩展](../articles/governance/policy/samples/not-allowed-vm-ext.md) | 禁止使用指定的扩展。 指定一个数组，其中包含被禁止的扩展类型。 |