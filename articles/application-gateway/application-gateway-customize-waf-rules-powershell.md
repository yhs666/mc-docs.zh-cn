---
title: "自定义 Azure 应用程序网关的 Web 应用程序防火墙规则 - PowerShell | Microsoft Docs"
description: "此页提供有关如何使用 PowerShell 自定义应用程序网关的 Web 应用程序防火墙规则的信息。"
documentationcenter: na
services: application-gateway
author: alexchen2016
manager: digimobile
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
origin.date: 07/26/2017
ms.date: 09/14/2017
ms.author: v-junlch
ms.openlocfilehash: b6507c9d0441d8902438c712c607b81b8fdc6d12
ms.sourcegitcommit: 9d9b56416d6f1f5f6df525b94232eba6e86e516b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2017
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a>通过 PowerShell 自定义 Web 应用程序防火墙规则

> [!div class="op_single_selector"]
> * [Azure 门户](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

应用程序网关 Web 应用程序防火墙可为 Web 应用程序提供保护。 这些保护由 OWASP CRS 规则集提供。 某些规则可能会导致误报，并会阻止实际流量。  出于此原因，应用程序网关在启用了 Web 应用程序防火墙的应用程序网关上提供自定义规则组和规则的功能。 有关特定规则组和规则的详细信息，请访问 [Web 应用程序防火墙 CRS 规则组和规则](application-gateway-crs-rulegroups-rules.md)

## <a name="view-rule-groups-and-rules"></a>查看规则组和规则

以下示例演示如何查看可在启用了 WAF 的应用程序网关上配置的规则和规则组。

### <a name="view-rule-groups"></a>查看规则组

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

下面是截取自前一示例的响应。

```
OWASP (Ver. 3.0):

    REQUEST-910-IP-REPUTATION:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            910011     Rule 910011
            910012     Rule 910012
            ...        ...

    REQUEST-911-METHOD-ENFORCEMENT:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            911011     Rule 911011
            ...        ...

OWASP (Ver. 2.2.9):

    crs_20_protocol_violations:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            960911     Invalid HTTP Request Line
            981227     Apache Error: Invalid URI in Request.
            960000     Attempted multipart/form-data bypass
            ...        ...
```

## <a name="disable-rules"></a>禁用规则

以下示例在应用程序网关上禁用规则 `910018` 和 `910017`。

```azurecli
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a>后续步骤

配置已禁用的规则后，可通过访问[应用程序网关诊断](application-gateway-diagnostics.md#diagnostic-logging)来了解如何查看 WAF 日志

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png

