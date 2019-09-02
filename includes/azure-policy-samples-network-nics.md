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
ms.openlocfilehash: c61423f43bdacf8cba91b309660da5c8581eae8a
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52651563"
---
### <a name="network-interfaces"></a>网络接口

|  |  |
|---------|---------|
| [每个 NIC 上的 NSG X](../articles/governance/policy/samples/nsg-on-nic.md) | 要求将特定网络安全组用于所有虚拟网络接口。 指定要使用的网络安全组的 ID。 |
| [对 VM 网络接口使用已批准的子网](../articles/governance/policy/samples/use-approved-subnet-vm-nics.md) | 要求网络接口使用已批准的子网。 指定已批准的子网的 ID。 |
| [对 VM 网络接口使用已批准的 vNet](../articles/governance/policy/samples/use-approved-vnet-vm-nics.md) | 要求网络接口使用已批准的虚拟网络。 由你指定已批准的虚拟网络的 ID。 |