---
title: 基于 Azure 防火墙威胁情报的筛选
description: 了解 Azure 防火墙威胁情报筛选
services: firewall
author: rockboyfor
ms.service: firewall
ms.topic: article
origin.date: 03/11/2019
ms.date: 07/22/2019
ms.author: v-yeche
ms.openlocfilehash: 31f7301748b8d1576d3d1ed9e38155abbebbe4ba
ms.sourcegitcommit: 5fea6210f7456215f75a9b093393390d47c3c78d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68337584"
---
# <a name="azure-firewall-threat-intelligence-based-filtering---public-preview"></a>基于 Azure 防火墙威胁情报的筛选 - 公共预览版

可以为防火墙启用基于威胁智能的筛选，以提醒和拒绝来自/到达已知恶意 IP 地址和域的流量。 IP 地址和域源自 Azure 威胁智能源。 [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) 支持 Azure 威胁情报，并已由多种服务（包括 Azure 安全中心）使用。

![防火墙威胁情报](media/threat-intel/firewall-threat.png)

> [!IMPORTANT]
> 基于威胁情报的筛选目前以公共预览版提供，附带预览版的服务级别协议。 某些功能可能不受支持或者受限。  有关详细信息，请参阅 [Azure 预览版补充使用条款](https://www.azure.cn/support/legal/subscription-agreement/)。

如果启用了基于威胁情报的筛选，则会先处理相关的规则，然后再处理任何 NAT 规则、网络规则或应用程序规则。 在预览期，只会包含最高置信度记录。

可以选择仅在触发规则时记录警报，或者选择警报和拒绝模式。

默认情况下，基于威胁情报的筛选将在警报模式下启用。 门户界面在你所在的区域中推出之前，你无法关闭此功能或更改模式。

![基于威胁情报的筛选门户界面](media/threat-intel/threat-intel-ui.png)

## <a name="logs"></a>日志

以下日志摘录显示了一个触发的规则：

```
{
    "category": "AzureFirewallNetworkRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallThreatIntelLog",
    "properties": {
         "msg": "HTTP request from 10.0.0.5:54074 to somemaliciousdomain.com:80. Action: Alert. ThreatIntel: Bot Networks"
    }
}
```

## <a name="testing"></a>测试

- **出站测试** - 出站流量警报极少发生，因为这意味着环境已遭到入侵。 为了帮助测试出站警报是否正常运行，已创建一个用于触发警报的测试 FQDN。 使用 **testmaliciousdomain.chinaeast.cloudapp.chinacloudapi.cn** 进行出站测试。

- **入站测试** - 如果在防火墙上配置了 DNAT 规则，则预期可以看到针对传入流量的警报。 即使 DNAT 规则中仅允许特定的源，并拒绝其他源的流量，也是如此。 Azure 防火墙不会针对所有已知的端口扫描程序发出警报，而只针对已知参与了恶意活动的扫描程序发出警报。

## <a name="next-steps"></a>后续步骤

- 参阅 [Azure 防火墙 Log Analytics 示例](log-analytics-samples.md)
- 了解如何[部署和配置 Azure 防火墙](tutorial-firewall-deploy-portal.md)
- 查看 [Microsoft 安全情报](https://www.microsoft.com/security/operations/security-intelligence-report)

<!-- Update_Description: new article about threat intel -->
<!--ms.date: 07/22/2019-->