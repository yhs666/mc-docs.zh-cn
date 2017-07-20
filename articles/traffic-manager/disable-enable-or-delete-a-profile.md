---
title: "禁用、启用或删除流量管理器配置文件 | Azure"
description: "本文将帮助你使用流量管理器配置文件。"
services: traffic-manager
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: f19143e2-2f4f-4ead-bb00-89b95ac92cb4
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 10/18/2016
ms.date: 01/18/2017
ms.author: v-dazen
ms.openlocfilehash: 87f506c1afc5dc9d5b129050f328b1d2f092b9ed
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
<!-- repub for nofollow -->

# <a name="disable-enable-or-delete-a-profile"></a>禁用、启用或删除配置文件
你可以禁用现有的流量管理器配置文件，以便它不再将用户请求指引到它配置的终结点。 禁用流量管理器配置文件后，配置文件自身及其中包含的信息将保持不变，并且可以在流量管理器界面中进行编辑。 当希望重新启用该配置文件时，可以轻松在 Azure 门户中执行此操作并且指引将恢复。 在 Azure 门户中创建流量管理器配置文件时，会自动启用该配置文件。

## <a name="to-disable-a-profile"></a>禁用配置文件
1. 修改你的 Internet DNS 服务器上的 DNS 资源记录以使用合适的记录类型和指向 Internet 上某个特定位置的其他名称或 IP 地址的指针。 换句话说，就是更改你的 Internet DNS 服务器上的 DNS 资源记录，使其不再使用指向你的流量管理器配置文件域名的 CNAME 资源记录。
2. 流量将停止通过流量管理器配置文件设置定向到终结点。
3. 选择你要禁用的配置文件。 若要选择配置文件，请在“流量管理器”页面上，通过单击配置文件名称旁边的列突出显示该配置文件。 不要单击配置文件的名称或名称旁边的箭头，因为这会将你带到配置文件的设置页面。
4. 在选择配置文件后，单击页面底部的“禁用”。

## <a name="to-enable-a-profile"></a>启用配置文件
1. 选择你要启用的配置文件。 若要选择配置文件，请在“流量管理器”页面上，通过单击配置文件名称旁边的列突出显示该配置文件。 不要单击配置文件的名称或名称旁边的箭头，因为这会将你带到配置文件的设置页面。
2. 在选择配置文件后，单击页面底部的“启用”。
3. 修改你的 Internet DNS 服务器上的 DNS 资源记录以使用 CNAME 记录类型，这会将你的公司域名映射到你的流量管理器配置文件的域名。 有关详细信息，请参阅[将公司 Internet 域指向流量管理器域](traffic-manager-point-internet-domain.md)。
4. 流量将重新开始定向到终结点。

## <a name="delete-a-profile"></a>删除配置文件
1. 确保你的 Internet DNS 服务器上的 DNS 资源记录不再使用指向你的流量管理器配置文件域名的 CNAME 资源记录。
2. 选择你要删除的配置文件。 若要选择配置文件，请在“流量管理器”页面上，通过单击配置文件旁边的列
3. 突出显示该配置文件。 不要单击配置文件的名称或名称旁边的箭头，因为这会将你带到配置文件的设置页面。
4. 在选择配置文件后，单击页面底部的“删除”。

## <a name="next-steps"></a>后续步骤
[流量管理器 - 禁用或启用终结点](disable-or-enable-an-endpoint.md)

[配置故障转移路由方法](traffic-manager-configure-failover-routing-method.md)

[配置轮循机制路由方法](traffic-manager-configure-round-robin-routing-method.md)

[配置性能路由方法](traffic-manager-configure-performance-routing-method.md)

[流量管理器降级状态疑难解答](traffic-manager-troubleshooting-degraded.md)