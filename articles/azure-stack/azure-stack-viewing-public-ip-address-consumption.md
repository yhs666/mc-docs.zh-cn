---
title: 查看 Azure Stack 中的公共 IP 地址使用情况 | Microsoft Docs
description: 管理员可以查看区域中公共 IP 地址的使用情况
services: azure-stack
documentationcenter: ''
author: ScottNapolitan
manager: darmour
editor: ''
ms.assetid: 0f77be49-eafe-4886-8c58-a17061e8120f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 09/25/2017
ms.date: 03/04/2018
ms.author: v-junlch
ms.openlocfilehash: 3321843eed3c0ae9f710c7cd8b3c6fcdb7bf4584
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="view-public-ip-address-consumption-in-azure-stack"></a>查看 Azure Stack 中的公共 IP 地址使用情况

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

云管理员可以查看分配给租户的公共 IP 地址数目、仍可供分配的公共 IP 地址数目，以及该位置中已分配的公共 IP 地址百分比。

“公共 IP 池用量”磁贴显示结构中所有公共 IP 地址池使用的公共 IP 地址总数，以及这些地址是用于租户 IaaS VM 实例、结构基础结构服务，还是租户显式创建的公共 IP 地址资源。

此磁贴的用途是让 Azure Stack 管理员了解此位置中已使用的公共 IP 地址总数。 此数字可帮助管理员确定此资源是否不足。

在“资源提供程序”>“网络”边栏选项卡中，“租户资源”下的“公共 IP 地址”菜单项只列出租户显式创建的公共 IP 地址。 因此，“公共 IP 池用量”磁贴上的“已使用”公共 IP 地址数目始终不同于（大于）“租户资源”下“公共 IP 地址”磁贴上的数目。

## <a name="view-the-public-ip-address-usage-information"></a>查看公共 IP 地址用量信息
查看区域中已使用的公共 IP 地址总数：

1. 在 Azure Stack 管理员门户中选择“更多服务”，然后在“管理资源”下面单击“资源提供程序”。
2. 从“资源提供程序”列表中选择“网络”。
3. “网络”边栏选项卡将在“概述”部分显示“公共 IP 池用量”磁贴。

![“网络资源提供程序”边栏选项卡](./media/azure-stack-viewing-public-ip-address-consumption/image01.png)

“已用”数字代表该位置的所有公共 IP 地址池中已分配的公共 IP 地址数目。 “可用”数字代表所有公共 IP 地址池中尚未分配的且仍可用的公共 IP 地址数目。 “已用百分比”数字代表已使用或已分配地址占该位置所有公共 IP 地址池中公共 IP 地址总数的百分比。

## <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>查看租户订阅创建的公共 IP 地址
若要查看特定区域中租户订阅显式创建的公共 IP 地址列表，请单击“租户资源”下的“公共 IP 地址”。

![租户公共 IP 地址](./media/azure-stack-viewing-public-ip-address-consumption/image02.png)

可能会看到，某些动态分配的公共 IP 地址显示在列表中，但这些地址没有关联的地址。 这是因为，地址资源已在网络资源提供程序中创建，但尚未在网络控制器中创建。

在将某个地址绑定到接口、网络接口卡 (NIC)、负载均衡器或虚拟网络网关之前，网络控制器不会将该地址分配给此资源。 将公共 IP 地址绑定到接口后，网络控制器会向其分配一个 IP 地址。此地址显示在“地址”字段中。

## <a name="view-the-public-ip-address-information-summary-table"></a>查看公共 IP 地址信息摘要表
在许多不同的情况下，分配的公共 IP 地址决定了要将地址显示在哪个列表中。

| **公共 IP 地址分配案例** | **显示在用量摘要中** | **显示在租户公共 IP 地址列表中** |
| --- | --- | --- |
| 尚未分配给 NIC 或负载均衡器（暂时性）的动态公共 IP 地址 |否 |是 |
| 已分配给 NIC 或负载均衡器的动态公共 IP 地址。 |是 |是 |
| 已分配给租户 NIC 或负载均衡器的静态公共 IP 地址。 |是 |是 |
| 已分配给结构基础结构服务终结点的静态公共 IP 地址。 |是 |否 |
| 为 IaaS VM 实例隐式创建的、在虚拟网络上用于出站 NAT 的公共 IP 地址。 每当租户创建 VM 实例，使 VM 能够将信息发送到 Internet 时，将在幕后创建这些地址。 |是 |否 |

## <a name="next-steps"></a>后续步骤
[管理 Azure Stack 中的存储帐户](azure-stack-manage-storage-accounts.md)

