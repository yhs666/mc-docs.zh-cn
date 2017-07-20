---
title: "禁用或启用流量管理器终结点 | Azure"
description: "本文将帮助你禁用或启用流量管理器配置文件终结点。"
services: traffic-manager
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 9b2264ce-be06-43b2-a00b-5c724e5d71fd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 10/18/2016
ms.date: 01/18/2017
ms.author: v-dazen
ms.openlocfilehash: da6f5a1d0f63842397e27b450f03a2f51cb408de
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
<!-- repub for nofollow -->

# <a name="disable-or-enable-a-traffic-manager-endpoint"></a>禁用或启用流量管理器终结点
你还可以禁用属于流量管理器配置文件的一部分的个体终结点。 终结点包括云服务和网站。 禁用某个终结点会将其保留为配置文件的一部分，但是配置文件的行为就如同其中不包括该终结点一样。 此操作对于临时删除处于维护模式或正在重新部署的终结点非常有用。 终结点再次运行后，可以启用它。

> [!NOTE]
> **禁用某个终结点对其在 Azure 中的部署状态没有任何影响。正常的终结点将保持运行并能够接收流量，即使在流量管理器中已将其禁用也是如此。此外，在一个配置文件中禁用某个终结点不会影响它在其他配置文件中的状态。**
> 
> 

## <a name="to-disable-an-endpoint"></a>禁用终结点
1. 在 Azure 管理门户的“流量管理器”窗格中，找到包含要修改的终结点设置的流量管理器配置文件，然后单击配置文件名称右侧的箭头。 这将打开配置文件的设置页面。
2. 在页面顶部，单击“终结点”来查看配置中包括的终结点。
3. 单击要禁用的终结点，然后单击页面底部的“禁用”。
4. 流量将根据为流量管理器域名配置的 DNS 生存时间 (TTL) 停止流向该终结点。 你可以从流量管理器配置文件的“配置”页面更改 TTL。

## <a name="to-enable-an-endpoint"></a>启用终结点
1. 在 Azure 管理门户的“流量管理器”窗格中，找到包含要修改的终结点设置的流量管理器配置文件，然后单击配置文件名称右侧的箭头。 这将打开配置文件的设置页面。
2. 在页面顶部，单击“终结点”来查看配置中包括的终结点。
3. 单击要启用的终结点，然后单击页面底部的“启用”。
4. 流量将重新开始流向配置文件所指定的服务。

## <a name="next-steps"></a>后续步骤
[流量管理器 - 禁用、启用或删除配置文件](disable-enable-or-delete-a-profile.md)

[流量管理器降级状态疑难解答](traffic-manager-troubleshooting-degraded.md)

[流量管理器性能注意事项](traffic-manager-performance-considerations.md)