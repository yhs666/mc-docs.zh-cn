---
title: 基于 URL 的内容路由概述 | Microsoft Docs
description: 本页提供基于应用程序网关 URL 的内容路由、UrlPathMap 配置和 PathBasedRouting 规则的概述。
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
origin.date: 11/07/2018
ms.date: 11/21/2018
ms.author: v-junlch
ms.openlocfilehash: 8307d111fe49f7041a1c4da8476d4f8915e066c4
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52672919"
---
# <a name="url-path-based-routing-overview"></a>基于 URL 路径的路由概述

基于 URL 路径的路由可让你根据请求的 URL 路径，将流量路由到后端服务器池。 

方案之一是将针对不同内容类型的请求路由到不同的后端服务器池。

在以下示例中，应用程序网关针对 contoso.com 从三个后端服务器池提供流量，例如：VideoServerPool、ImageServerPool 和 DefaultServerPool。

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

对 http://contoso.com/video/* 的请求会路由到 VideoServerPool，而对 http://contoso.com/images/* 的请求则会路由到 ImageServerPool。 如果没有任何路径模式匹配，则选择 DefaultServerPool。

> [!IMPORTANT]
> 规则将按照门户中的列出顺序进行处理。 我们强烈建议先配置多站点侦听器，然后再配置基本侦听器。  这确保将流量路由到适当的后端。 如果基本侦听器先列出并且与传入的请求匹配，则该侦听器将处理该请求。

## <a name="urlpathmap-configuration-element"></a>UrlPathMap 配置元素

urlPathMap 元素用于指定后端服务器池映射的路径模式。 以下代码示例是模板文件中 urlPathMap 元素的代码片段。

```json
"urlPathMaps": [{
    "name": "{urlpathMapName}",
    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/urlPathMaps/{urlpathMapName}",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/    {subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName1}"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpSettingsList/{settingname1}"
        },
        "pathRules": [{
            "name": "{pathRuleName}",
            "properties": {
                "paths": [
                    "{pathPattern}"
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName2}"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpsettingsList/{settingName2}"
                }
            }
        }]
    }
}]
```

> [!NOTE]
> PathPattern：此设置是要匹配的路径模式列表。 每个模式必须以 / 开头，只允许在后接“/”的末尾处添加“*”。 提供给路径匹配器的字符串在第一个“?” 或“#”之后不包含任何文本，而且这些字符不允许在这里。 否则，URL 中允许的任何字符在 PathPattern 中都是允许的。

有关详细信息，可以查看[使用基于 URL 的路由的 Resource Manager 模板](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing)。

## <a name="pathbasedrouting-rule"></a>PathBasedRouting 规则

PathBasedRouting 类型的 RequestRoutingRule 可用于将侦听器绑定到 urlPathMap。 此侦听器收到的所有请求将根据 urlPathMap 中指定的策略进行路由。
PathBasedRouting 规则的代码片段：

```json
"requestRoutingRules": [
    {

"name": "{ruleName}",
"id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/requestRoutingRules/{ruleName}",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/ urlPathMaps/{urlpathMapName}"
    },

}
    }
]
```

## <a name="next-steps"></a>后续步骤

了解基于 URL 的内容路由之后，请转到[使用基于 URL 的路由创建应用程序网关](application-gateway-create-url-route-portal.md)，使用 URL 路由规则创建应用程序网关。

<!-- Update_Description: wording update -->