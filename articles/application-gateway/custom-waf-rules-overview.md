---
title: Azure Web 应用程序防火墙 (WAF) v2 自定义规则
description: 本文概述 Azure 应用程序网关中的 Web 应用程序防火墙 (WAF) v2 自定义规则。
services: application-gateway
ms.topic: article
author: vhorne
ms.service: application-gateway
origin.date: 06/18/2019
ms.date: 10/23/2019
ms.author: v-junlch
ms.openlocfilehash: 61826183987abd0dc60d87c580f5a6f7313aa8ab
ms.sourcegitcommit: 24b69c0a22092c64c6c3db183bb0655a23340420
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2019
ms.locfileid: "72798488"
---
# <a name="overview-custom-rules-for-web-application-firewall-v2"></a>概述：Web 应用程序防火墙 v2 的自定义规则

Azure 应用程序网关 Web 应用程序防火墙 (WAF) v2 附带了一个预配置的、由平台管理的规则集，用于防范多种不同类型的攻击。 这些攻击包括跨站点脚本、SQL 注入，等等。 如果你是 WAF 管理员，你可能想要编写自己的规则来补充核心规则集规则。 你的规则可以根据匹配条件阻止或允许请求的流量。

使用自定义规则，可以创建自己的规则，这些规则对通过 WAF 传递的每个请求进行评估。 这些规则的优先级高于托管规则集中的其他规则。 自定义规则包含规则名称、规则优先级和一系列匹配条件。 如果满足这些条件，则执行允许或阻止操作。

例如，可以创建一个规则来阻止来自 192.168.5.4/24 范围内的某个 IP 地址的所有请求。 在此规则中，运算符是 *IPMatch*，*matchValues* 是 IP 地址范围 (192.168.5.4/24)，操作是阻止流量。  还需要设置规则的名称和优先级。

自定义规则支持使用复合逻辑创建更高级的规则来解决安全需求。 例如，“（条件 1 *and* 条件 2，*or* 条件 3）”表示如果满足条件 1 *和*条件 2，*或者*满足条件 3，则 WAF 应执行自定义规则中指定的操作。

同一规则中的不同匹配条件始终使用 *and* 来组合。 例如，使用 *and* 的规则可以指定仅当使用特定的浏览器时，才阻止来自特定 IP 地址的流量。

若要对两个不同的条件使用 *or* 运算符，这两个条件必须在不同的规则中。 例如，使用 *or* 的规则可以指定阻止来自特定 IP 地址的流量，或者在使用特定的浏览器时阻止流量。

> [!NOTE]
> WAF 自定义规则的最大数目为 100。 有关应用程序网关限制的详细信息，请参阅 [Azure 订阅和服务限制、配额与约束](../azure-subscription-service-limits.md#application-gateway-limits)。

自定义规则还支持正则表达式，就像在核心规则集中一样。 有关这些规则的示例，请参阅[创建和使用自定义 Web 应用程序防火墙规则](create-custom-waf-rules.md)中的“示例 3”和“示例 5”。

## <a name="allowing-or-blocking-traffic"></a>允许或阻止流量

使用自定义规则可以方便地允许或阻止流量。 例如，可以阻止来自某个 IP 地址范围的所有流量。 可以创建另一个规则，以便在请求来自特定的浏览器时允许流量。

若要允许某种流量，请确保将 `-Action` 参数设置为 **Allow**。 若要阻止某种流量，请确保将 `-Action` 参数设置为 **Block**，如以下代码所示：

```azurepowershell
$AllowRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example1 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Allow

$BlockRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example2 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Block
```

上面的 `$BlockRule` 映射到 Azure 资源管理器中的以下自定义规则：

```json
"customRules": [
      {
        "name": "blockEvilBot",
        "priority": 2,
        "ruleType": "MatchRule",
        "action": "Block",
        "matchConditions": [
          {
            "matchVariables": [
              {
                "variableName": "RequestHeaders",
                "selector": "User-Agent"
              }
            ],
            "operator": "Contains",
            "negationConditon": false,
            "matchValues": [
              "evilbot"
            ],
            "transforms": [
              "Lowercase"
            ]
          }
        ]
      }
    ], 
```

此自定义规则包含名称、优先级、操作，以及执行该操作所要满足的一系列匹配条件。 有关自定义规则字段的说明，请参阅后续部分。 有关自定义规则的示例，请参阅[创建和使用自定义 Web 应用程序防火墙规则](create-custom-waf-rules.md)。

## <a name="custom-rule-fields"></a>自定义规则字段

### <a name="name-optional"></a>Name（可选）

这是规则的名称。 该名称将显示在日志中。

### <a name="priority-required"></a>Priority（必需）

- 优先级确定规则的评估顺序。 值越小，规则的评估顺序越靠前。 允许的范围为 1 到 100。
- 优先级在所有自定义规则中必须唯一。 优先级为 40 的规则将在优先级为 80 的规则之前评估。

### <a name="rule-type-required"></a>Rule type（必需）

目前，规则类型必须是 **MatchRule**。

### <a name="match-variable-required"></a>Match variable（必需）

匹配变量必须是下列其中一项：

- RemoteAddr：远程计算机连接的 IP 地址或主机名
- RequestMethod：HTTP 请求方法（GET、POST、PUT、DELETE 等）。
- QueryString：URI 中的变量。
- PostArgs：在 POST 正文中发送的参数。 仅当 Content-Type 标头设置为“application/x-www-form-urlencoded”和“multipart/form-data”时，才会应用使用此匹配变量的自定义规则。
- RequestUri：请求的 URI。
- RequestHeaders：请求的标头。
- RequestBody：包含整个请求正文的变量。 仅当 Content-Type 标头设置为“application/x-www-form-urlencoded”时，才会应用使用此匹配变量的自定义规则。 
- RequestCookies：请求的 Cookie。

### <a name="selector-optional"></a>Selector（可选）

描述 matchVariable 集合字段的选择器。 例如，如果 matchVariable 为“RequestHeaders”，则选择器可以位于 User-Agent 标头中。

### <a name="operator-required"></a>Operator（必需）

运算符必须是下列其中一项：

- IPMatch：仅当匹配变量为 *RemoteAddr* 时，才使用此运算符。
- Equals：输入内容与 MatchValue 相同。
- Contains
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- BeginsWith
- EndsWith
- 正则表达式

### <a name="negate-condition-optional"></a>Negate condition（可选）

对当前条件求反。

### <a name="transform-optional"></a>Transform（可选）

一个字符串列表，其中包含尝试匹配之前完成的转换的名称。 转换包括：

- 小写
- Trim
- UrlDecode
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Match values（必需）

matchValues 字段是要匹配的值列表，可被视为采用 *OR* 运算符。 例如，值可以是 IP 地址或其他字符串。 值的格式取决于上一个运算符。

### <a name="action-required"></a>Action（必需）

action 字段提供以下选项： 

- 允许：授权事务，跳过所有后续规则。 这意味着，指定的请求将添加到允许列表，并且在匹配后，该请求将停止进一步的评估，并发送到后端池。 不会根据允许列表中的规则评估其他自定义规则或托管规则。

- 阻止：基于 *SecDefaultAction*（检测/阻止模式）阻止事务。 与 Allow 操作一样，对请求进行评估并将其添加到阻止列表后，评估将会停止，请求将被阻止。 不会评估满足相同条件的后续请求， 而只会将其阻止。 

- Log：允许该规则写入日志，但允许其他规则运行以进行评估。 后续的自定义规则将接在托管规则的后面按优先顺序进行评估。

## <a name="next-steps"></a>后续步骤

了解自定义规则后，可以[创建自己的自定义规则](create-custom-waf-rules.md)。

<!-- Update_Description: wording update -->