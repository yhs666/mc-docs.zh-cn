---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 88c361957003c619ea8e289c41f925ce530426af
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676160"
---
必须认识到有两种方法可以在 Azure 中配置可用性组侦听程序。 这些方法在创建侦听程序时使用的 Azure 负载均衡器的类型方面有所不同。 下表描述了差异：

| 负载均衡器类型 | 实现 | 使用条件如下： |
| --- | --- | --- |
| **外部** |使用托管虚拟机 (VM) 的云服务的“公用虚拟 IP 地址”。 |需要从虚拟网络外部访问侦听程序，包括从 Internet 访问。 |
| **内部** |使用“内部负载均衡器”，其地址专用于侦听程序。 |只能从同一个虚拟网络访问侦听程序。 该访问包括混合方案中的站点到站点 VPN。 |

> [!IMPORTANT]
> 对于使用云服务公共 VIP（外部负载均衡器）的侦听程序，只要客户端、侦听程序和数据库位于同一个 Azure 区域中，就不会产生传出费用。 否则，通过侦听程序返回的任何数据将被视为传出数据，因此需要支付正常的数据传输费。 
> 
> 

ILB 只能在具有区域范围的虚拟网络上配置。 已配置地缘组的现有虚拟网络无法使用 ILB。 有关详细信息，请参阅[内部负载均衡器概述](../articles/load-balancer/load-balancer-internal-overview.md)。

<!-- Update_Description: update meta properties -->