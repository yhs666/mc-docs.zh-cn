---
title: Azure 防火墙规则处理逻辑
description: 了解 Azure 防火墙规则处理逻辑
services: firewall
author: rockboyfor
ms.service: firewall
ms.topic: article
origin.date: 09/27/2018
ms.date: 07/22/2019
ms.author: v-yeche
ms.openlocfilehash: fc3a2f4a38d001f2f25c4bed94d7fd8d9f998464
ms.sourcegitcommit: 5fea6210f7456215f75a9b093393390d47c3c78d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68337326"
---
# <a name="azure-firewall-rule-processing-logic"></a>Azure 防火墙规则处理逻辑
Azure 防火墙具有 NAT 规则、网络规则和应用程序规则。 规则是根据规则类型进行处理的。

## <a name="network-rules-and-applications-rules"></a>网络规则和应用程序规则 
首先将应用网络规则，然后应用应用程序规则。 规则将终止。 因此，如果在网络规则中找到匹配项，则不会处理应用程序规则。  如果没有网络规则匹配项，并且数据包协议是 HTTP/HTTPS，则会按应用程序规则评估数据包。 如果仍未找到匹配项，则会根据基础结构规则集合评估数据包。 如果仍然没有匹配项，则默认情况下会拒绝该数据包。

## <a name="nat-rules"></a>NAT 规则
可以通过配置目标网络地址转换 (DNAT) 来启用入站连接，如[教程：使用 Azure 门户通过 Azure Firewall DNAT 筛选入站流量](tutorial-firewall-dnat.md)中所述。 首先将应用 DNAT 规则。 如果找到匹配项，则会添加一个隐式的对应网络规则来允许转换后的流量。 可以通过以下方法替代此行为：显式添加一个网络规则集合并在其中包含将匹配转换后流量的拒绝规则。 对于这些连接，不会应用应用程序规则。

## <a name="next-steps"></a>后续步骤

- 了解如何[部署和配置 Azure 防火墙](tutorial-firewall-deploy-portal.md)。

<!-- Update_Description: new article about rule processing -->
<!--ms.date: 07/22/2019-->