---
title: 理解查询语言
description: 介绍所提供的可以与 Azure Resource Graph 配合使用的 Kusto 运算符和函数。
author: DCtheGeek
ms.author: v-yiso
origin.date: 04/22/2019
ms.date: 09/16/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 0a5248f490ac803d81e843ada42bf9ce8f2349e3
ms.sourcegitcommit: dd0ff08835dd3f8db3cc55301815ad69ff472b13
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70737411"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>了解 Azure Resource Graph 查询语言

Azure Resource Graph 查询语言支持多个运算符和函数。 每项工作和操作都基于 [Azure 数据资源管理器](../../../data-explorer/data-explorer-overview.md)完成。

了解 Resource Graph 使用的查询语言的最佳方法是，从 Azure 数据资源管理器[查询语言](/kusto/query/index)文档开始入手。 此文档介绍了语言的组织结构以及各种支持的运算符和函数的协同工作原理。

## <a name="supported-tabular-operators"></a>支持的表格运算符

下面是 Resource Graph 中支持的表格运算符列表：

- [count](/kusto/query/countoperator)
- [distinct](/kusto/query/distinctoperator)
- [extend](/kusto/query/extendoperator)
- [limit](/kusto/query/limitoperator)
- [order by](/kusto/query/orderoperator)
- [project](/kusto/query/projectoperator)
- [project-away](/kusto/query/projectawayoperator)
- [sample](/kusto/query/sampleoperator)
- [sample-distinct](/kusto/query/sampledistinctoperator)
- [排序依据](/kusto/query/sortoperator)
- [summarize](/kusto/query/summarizeoperator)
- [take](/kusto/query/takeoperator)
- [返回页首](/kusto/query/topoperator)
- [top-nested](/kusto/query/topnestedoperator)
- [top-hitters](/kusto/query/tophittersoperator)
- [where](/kusto/query/whereoperator)

## <a name="supported-functions"></a>支持的函数

下面是 Resource Graph 中支持的函数列表：

- [ago()](/kusto/query/agofunction)
- [buildschema()](/kusto/query/buildschema-aggfunction)
- [strcat()](/kusto/query/strcatfunction)
- [isnotempty()](/kusto/query/isnotemptyfunction)
- [tostring()](/kusto/query/tostringfunction)
- [zip()](/kusto/query/zipfunction)

## <a name="escape-characters"></a>转义字符

某些属性名称（例如那些包含 `.` 或 `$` 的属性名称）必须在查询中进行包装或转义，否则就会解释不正确，无法提供预期的结果。

- `.` - 将属性名称包装成这样：`['propertyname.withaperiod']`
  
  包装 _odata.type_ 属性的示例查询：

  ```kusto
  where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.['odata.type']
  ```

- `$` - 对属性名称中的字符进行转义。 所使用的转义字符取决于 Resource Graph 从哪个 shell 运行。

  - **bash** - `\`

    在 bash 中对 _\$type_ 属性进行转义的示例查询：

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.\$type
    ```

  - **cmd** - 不对 `$` 字符进行转义。

  - **PowerShell** - ``` ` ```

    在 PowerShell 中对 _\$type_ 属性进行转义的示例查询：

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.`$type
    ```

## <a name="next-steps"></a>后续步骤

- 请参阅[初学者查询](../samples/starter.md)中使用中的语言
- 请参阅[高级查询](../samples/advanced.md)中的高级使用
- 了解如何[浏览资源](explore-resources.md)