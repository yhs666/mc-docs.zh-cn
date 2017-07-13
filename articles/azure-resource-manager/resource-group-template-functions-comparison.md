---
title: "Azure Resource Manager 模板函数 - 比较 | Azure"
description: "介绍可在 Azure Resource Manager 模板中使用的用于比较值的函数。"
services: azure-resource-manager
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 06/13/2017
ms.date: 07/03/2017
ms.author: v-yeche
ms.openlocfilehash: e6acb78d01b9667ec5d6c6cd1946f0c7463e5485
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 用于 Azure Resource Manager 模板的比较函数
<a id="comparison-functions-for-azure-resource-manager-templates" class="xliff"></a>

Resource Manager 提供了多个用于在模板中进行比较的函数。

* [equals](#equals)
* [less](#less)
* [lessOrEquals](#lessorequals)
* [greater](#greater)
* [greaterOrEquals](#greaterorequals)

<a id="equals" />

## equals
<a id="equals" class="xliff"></a>
`equals(arg1, arg2)`

检查两个值是否相等。

### Parameters
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |int、string、array 或 object |要检查是否相等的第一个值。 |
| arg2 |是 |int、string、array 或 object |要检查是否相等的第二个值。 |

### 返回值
<a id="return-value" class="xliff"></a>

如果值相等，返回 **True**；否则返回 **False**。

### 备注
<a id="remarks" class="xliff"></a>

equals 函数通常与 `condition` 元素一起使用来测试资源是否已部署。

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

### 示例
<a id="example" class="xliff"></a>

示例模板检查不同类型的值是否相等。 所有默认值都返回 True。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 1
        },
        "firstString": {
            "type": "string",
            "defaultValue": "a"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "firstObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[equals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[equals(parameters('firstString'), parameters('secondString'))]"
        },
        "checkArrays": {
            "type": "bool",
            "value": "[equals(parameters('firstArray'), parameters('secondArray'))]"
        },
        "checkObjects": {
            "type": "bool",
            "value": "[equals(parameters('firstObject'), parameters('secondObject'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| checkInts | Bool | True |
| checkStrings | Bool | True |
| checkArrays | Bool | True |
| checkObjects | Bool | True |

<a id="less" />

## less
<a id="less" class="xliff"></a>
`less(arg1, arg2)`

检查第一个值是否小于第二个值。

### Parameters
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |int 或 string |用于小于比较的第一个值。 |
| arg2 |是 |int 或 string |用于小于比较的第二个值。 |

### 返回值
<a id="return-value" class="xliff"></a>

如果第一个值小于第二个值，返回 **True**；否则返回 **False**。

### 示例
<a id="example" class="xliff"></a>

示例模板检查一个值是否小于另一个值。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[less(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[less(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| checkInts | Bool | True |
| checkStrings | Bool | False |

<a id="lessorequals" />

## lessOrEquals
<a id="lessorequals" class="xliff"></a>
`lessOrEquals(arg1, arg2)`

检查第一个值是否小于或等于第二个值。

### Parameters
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |int 或 string |用于小于或等于比较的第一个值。 |
| arg2 |是 |int 或 string |用于小于或等于比较的第二个值。 |

### 返回值
<a id="return-value" class="xliff"></a>

如果第一个值小于或等于第二个值，返回 **True**；否则返回 **False**。

### 示例
<a id="example" class="xliff"></a>

示例模板检查一个值是否小于或等于另一个值。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| checkInts | Bool | True |
| checkStrings | Bool | False |

<a id="greater" />

## greater
<a id="greater" class="xliff"></a>
`greater(arg1, arg2)`

检查第一个值是否大于第二个值。

### Parameters
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |int 或 string |用于大于比较的第一个值。 |
| arg2 |是 |int 或 string |用于大于比较的第二个值。 |

### 返回值
<a id="return-value" class="xliff"></a>

如果第一个值大于第二个值，返回 **True**；否则返回 **False**。

### 示例
<a id="example" class="xliff"></a>

示例模板检查一个值是否大于另一个值。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greater(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greater(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| checkInts | Bool | False |
| checkStrings | Bool | True |

<a id="greaterorequals" />

## greaterOrEquals
<a id="greaterorequals" class="xliff"></a>
`greaterOrEquals(arg1, arg2)`

检查第一个值是否大于或等于第二个值。

### Parameters
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |int 或 string |用于大于或等于比较的第一个值。 |
| arg2 |是 |int 或 string |用于大于或等于比较的第二个值。 |

### 返回值
<a id="return-value" class="xliff"></a>

如果第一个值大于或等于第二个值，返回 **True**；否则返回 **False**。

### 示例
<a id="example" class="xliff"></a>

示例模板检查一个值是否大于或等于另一个值。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| checkInts | Bool | False |
| checkStrings | Bool | True |

## 后续步骤
<a id="next-steps" class="xliff"></a>
* 有关 Azure Resource Manager 模板中各部分的说明，请参阅[创作 Azure Resource Manager 模板](resource-group-authoring-templates.md)。
* 若要合并多个模板，请参阅[将链接的模板与 Azure Resource Manager 配合使用](resource-group-linked-templates.md)。
* 若要在创建资源类型时迭代指定的次数，请参阅[在 Azure Resource Manager 中创建多个资源实例](resource-group-create-multiple.md)。
* 若要查看如何部署已创建的模板，请参阅[使用 Azure Resource Manager 模板部署应用程序](resource-group-template-deploy.md)。