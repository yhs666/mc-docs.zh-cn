---
title: "Azure Resource Manager 模板函数 - 字符串 | Azure"
description: "介绍了可在 Azure Resource Manager 模板中用来处理字符串的函数。"
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
ms.openlocfilehash: 903054cd9d512889a8aac92b6b1c97ff7232af96
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 用于 Azure Resource Manager 模板的字符串函数
<a id="string-functions-for-azure-resource-manager-templates" class="xliff"></a>

Resource Manager 提供以下用于处理字符串的函数：

* [base64](#base64)
* [base64ToJson](#base64tojson)
* [base64ToString](#base64tostring)
* [bool](#bool)
* [concat](#concat)
* [contains](#contains)
* [dataUri](#datauri)
* [dataUriToString](#datauritostring)
* [empty](#empty)
* [endsWith](#endswith)
* [first](#first)
* [indexOf](#indexof)
* [last](#last)
* [lastIndexOf](#lastindexof)
* [length](#length)
* [padLeft](#padleft)
* [replace](#replace)
* [skip](#skip)
* [split](#split)
* [startsWith](resource-group-template-functions-string.md#startswith)
* [字符串](#string)
* [substring](#substring)
* [take](#take)
* [toLower](#tolower)
* [toUpper](#toupper)
* [trim](#trim)
* [uniqueString](#uniquestring)
* [uri](#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)

<a id="base64" />

## base64
<a id="base64" class="xliff"></a>
`base64(inputString)`

返回输入字符串的 base64 表示形式。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| inputString |是 |字符串 |要以 base64 表示形式返回的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

包含 base64 表示形式的字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例演示如何使用 base64 函数。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| base64Output | String | b25lLCB0d28sIHRocmVl |
| toStringOutput | String | one, two, three |
| toJsonOutput | 对象 | {"one": "a", "two": "b"} |

<a id="base64tojson" />

## base64ToJson
<a id="base64tojson" class="xliff"></a>
`base64tojson`

将 base64 表示形式转换为 JSON 对象。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| base64Value |是 |字符串 |要转换为 JSON 对象的 base64 表示形式。 |

### 返回值
<a id="return-value" class="xliff"></a>

一个 JSON 对象。

### 示例
<a id="examples" class="xliff"></a>

以下示例使用 base64ToJson 函数转换 base64 值：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| base64Output | String | b25lLCB0d28sIHRocmVl |
| toStringOutput | String | one, two, three |
| toJsonOutput | 对象 | {"one": "a", "two": "b"} |

<a id="base64tostring" />

## base64ToString
<a id="base64tostring" class="xliff"></a>
`base64ToString(base64Value)`

将 base64 表示形式转换为字符串。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| base64Value |是 |字符串 |要转换为字符串的 base64 表示形式。 |

### 返回值
<a id="return-value" class="xliff"></a>

转换后的 base64 值的字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例使用 base64ToString 函数转换 base64 值：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| base64Output | String | b25lLCB0d28sIHRocmVl |
| toStringOutput | String | one, two, three |
| toJsonOutput | 对象 | {"one": "a", "two": "b"} |

<a id="bool" />

## bool
<a id="bool" class="xliff"></a>
`bool(arg1)`

将参数转换为布尔值。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |字符串或整数 |要转换为布尔值的值。 |

### 返回值
<a id="return-value" class="xliff"></a>
转换后的值的布尔值。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何对字符串或整数使用 bool。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| trueString | Bool | True |
| falseString | Bool | False |
| trueInt | Bool | True |
| falseInt | Bool | False |

<a id="concat" />

## concat
<a id="concat" class="xliff"></a>
`concat (arg1, arg2, arg3, ...)`

合并多个字符串值并返回串联的字符串，或合并多个数组并返回串联的数组。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |字符串或数组 |串联的第一个值。 |
| 其他参数 |否 |字符串 |要按顺序串联的其他值。 |

### 返回值
<a id="return-value" class="xliff"></a>
由串联值构成的字符串或数组。

### 示例
<a id="examples" class="xliff"></a>

以下示例演示如何组合两个字符串值并返回串联的字符串。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "prefix": {
            "type": "string",
            "defaultValue": "prefix"
        }
    },
    "resources": [],
    "outputs": {
        "concatOutput": {
            "value": "[concat(parameters('prefix'), '-', uniqueString(resourceGroup().id))]",
            "type" : "string"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| concatOutput | String | prefix-5yj4yjf5mbg72 |

以下示例演示如何组合两个数组。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "firstArray": { 
            "type": "array", 
            "defaultValue": [ 
                "1-1", 
                "1-2", 
                "1-3" 
            ] 
        },
        "secondArray": {
            "type": "array", 
            "defaultValue": [ 
                "2-1", 
                "2-2",
                "2-3" 
            ] 
        }
    },
    "resources": [
    ],
    "outputs": {
        "return": {
            "type": "array",
            "value": "[concat(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| return | Array | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

<a id="contains" />

## contains
<a id="contains" class="xliff"></a>
`contains (container, itemToFind)`

检查数组是否包含某个值、某个对象是否包含某个键，或者某个字符串是否包含某个子字符串。

### Parameters
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| container |是 |数组、对象或字符串 |包含要查找的值的值。 |
| itemToFind |是 |字符串或整数 |要查找的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

如果找到该项，则为 **True**；否则为 **False**。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何对不同的类型使用 contains：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "OneTwoThree"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringTrue": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'e')]"
        },
        "stringFalse": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'z')]"
        },
        "objectTrue": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'one')]"
        },
        "objectFalse": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'a')]"
        },
        "arrayTrue": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'three')]"
        },
        "arrayFalse": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'four')]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| stringTrue | Bool | True |
| stringFalse | Bool | False |
| objectTrue | Bool | True |
| objectFalse | Bool | False |
| arrayTrue | Bool | True |
| arrayFalse | Bool | False |

<a id="datauri" />

## dataUri
<a id="datauri" class="xliff"></a>
`dataUri(stringToConvert)`

将一个值转换为数据 URI。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| stringToConvert |是 |字符串 |要转换为数据 URI 的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

格式为数据 URI 的字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例将一个值转换为数据 URI，然后将数据 URI 转换为字符串：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| dataUriOutput | String | data:text/plain;charset=utf8;base64,SGVsbG8= |
| toStringOutput | String | Hello, World! |

<a id="datauritostring" />

## dataUriToString
<a id="datauritostring" class="xliff"></a>
`dataUriToString(dataUriToConvert)`

将采用数据 URI 格式的值转换为字符串。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| dataUriToConvert |是 |字符串 |要转换的数据 URI 值。 |

### 返回值
<a id="return-value" class="xliff"></a>

包含转换后的值的字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例将一个值转换为数据 URI，然后将数据 URI 转换为字符串：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| dataUriOutput | String | data:text/plain;charset=utf8;base64,SGVsbG8= |
| toStringOutput | String | Hello, World! |

<a id="empty" /> 

## empty
<a id="empty" class="xliff"></a>
`empty(itemToTest)`

确定数组、对象或字符串是否为空。

### Parameters
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| itemToTest |是 |数组、对象或字符串 |要检查是否为空的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

如果该值为空，则返回 **True**；否则返回 **False**。

### 示例
<a id="examples" class="xliff"></a>

以下示例检查某个数组、对象和字符串是否为空。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": []
        },
        "testObject": {
            "type": "object",
            "defaultValue": {}
        },
        "testString": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testArray'))]"
        },
        "objectEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testObject'))]"
        },
        "stringEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testString'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| arrayEmpty | Bool | True |
| objectEmpty | Bool | True |
| stringEmpty | Bool | True |

<a id="endswith" />

## endsWith
<a id="endswith" class="xliff"></a>
`endsWith(stringToSearch, stringToFind)`

确定某个字符串是否以某个值结尾。 比较不区分大小写。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| stringToSearch |是 |字符串 |包含要查找的项的值。 |
| stringToFind |是 |字符串 |要查找的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

如果字符串的最后一个或多个字符与该值匹配，则为 **True**；否则为 **False**。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何使用 startsWith 和 endsWith 函数：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| startsTrue | Bool | True |
| startsCapTrue | Bool | True |
| startsFalse | Bool | False |
| endsTrue | Bool | True |
| endsCapTrue | Bool | True |
| endsFalse | Bool | False |

<a id="first" />

## first
<a id="first" class="xliff"></a>
`first(arg1)`

返回字符串的第一个字符，或数组的第一个元素。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |数组或字符串 |要检索第一个元素或字符的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

第一个字符的字符串，或者数组中第一个元素的类型（字符串、整数、数组或对象）。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何对不同的类型使用 first 函数。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[first(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[first('One Two Three')]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| arrayOutput | String | one |
| stringOutput | String | O |

<a id="indexof" />

## indexOf
<a id="indexof" class="xliff"></a>
`indexOf(stringToSearch, stringToFind)`

返回字符串中某个值的第一个位置。 比较不区分大小写。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| stringToSearch |是 |字符串 |包含要查找的项的值。 |
| stringToFind |是 |字符串 |要查找的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

一个整数，表示要查找的项的位置。 该值从零开始。 如果未找到该项，则返回 -1。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何使用 indexOf 和 lastIndexOf 函数：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| firstT | int | 0 |
| lastT | int | 3 |
| firstString | int | 2 |
| lastString | int | 0 |
| notFound | int | -1 |

<a id="last" />

## last
<a id="last" class="xliff"></a>
`last (arg1)`

返回字符串的最后一个字符，或数组的最后一个元素。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |数组或字符串 |要检索最后一个元素或字符的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

最后一个字符的字符串，或者数组中最后一个元素的类型（字符串、整数、数组或对象）。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何对不同的类型使用 last 函数。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[last(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[last('One Two Three')]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| arrayOutput | String | three |
| stringOutput | String | e |

<a id="lastindexof" />

## lastIndexOf
<a id="lastindexof" class="xliff"></a>
`lastIndexOf(stringToSearch, stringToFind)`

返回字符串中某个值的最后一个位置。 比较不区分大小写。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| stringToSearch |是 |字符串 |包含要查找的项的值。 |
| stringToFind |是 |字符串 |要查找的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

一个整数，表示要查找的项的最后一个位置。 该值从零开始。 如果未找到该项，则返回 -1。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何使用 indexOf 和 lastIndexOf 函数：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| firstT | int | 0 |
| lastT | int | 3 |
| firstString | int | 2 |
| lastString | int | 0 |
| notFound | int | -1 |

<a id="length" />

## length
<a id="length" class="xliff"></a>
`length(string)`

返回字符串中的字符数，或数组中的元素数。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| arg1 |是 |数组或字符串 |用于获取元素数的数组，或用于获取字符数的字符串。 |

### 返回值
<a id="return-value" class="xliff"></a>

一个整数。 

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何对数组和字符串使用 length：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "stringToTest": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "arrayLength": {
            "type": "int",
            "value": "[length(parameters('arrayToTest'))]"
        },
        "stringLength": {
            "type": "int",
            "value": "[length(parameters('stringToTest'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| arrayLength | int | 3 |
| stringLength | int | 13 |

<a id="padleft" />

## padLeft
<a id="padleft" class="xliff"></a>
`padLeft(valueToPad, totalLength, paddingCharacter)`

通过向左侧添加字符直至到达指定的总长度返回右对齐的字符串。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| valueToPad |是 |字符串或整数 |要右对齐的值。 |
| totalLength |是 |int |返回字符串中的字符总数。 |
| paddingCharacter |否 |单个字符 |要用于向左填充直到达到总长度的字符。 默认值为空格。 |

如果原始字符串的长度超过要填充的字符数，则不会添加任何字符。

### 返回值
<a id="return-value" class="xliff"></a>

一个字符串，其中至少包含指定的字符数。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何通过添加零字符直到字符总数，来填充用户提供的参数值。 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123"
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[padLeft(parameters('testString'),10,'0')]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| stringOutput | String | 0000000123 |

<a id="replace" />

## replace
<a id="replace" class="xliff"></a>
`replace(originalString, oldString, newString)`

返回其中某个字符串的所有实例均替换为另一个字符串的新字符串。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| originalString |是 |字符串 |包含某一个字符串的所有实例均替换为另一个字符串的值。 |
| oldString |是 |字符串 |要从原始字符串中删除的字符串。 |
| newString |是 |字符串 |要添加以替代已删除字符串的字符串。 |

### 返回值
<a id="return-value" class="xliff"></a>

包含被替换字符的字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何从用户提供的字符串中删除所有短划线，以及如何将字符串的一部分替换为其他字符串。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123-123-1234"
        }
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'-', '')]"
        },
        "secodeOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'1234', 'xxxx')]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| firstOutput | String | 1231231234 |
| secodeOutput | String | 123-123-xxxx |

<a id="skip" />

## skip
<a id="skip" class="xliff"></a>
`skip(originalValue, numberToSkip)`

返回一个字符串，其中包含指定字符数后面的所有字符；或者返回一个数组，其中包含指定元素数后面的所有元素。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| originalValue |是 |数组或字符串 |用于跳过的数组或字符串。 |
| numberToSkip |是 |int |要跳过的元素数或字符数。 如果此值小于或等于 0，则返回值中的所有元素或字符。 如果此值大于数组或字符串的长度，则返回空数组或字符串。 |

### 返回值
<a id="return-value" class="xliff"></a>

数组或字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例跳过数组中指定数目的元素，以及字符串中指定数目的字符。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToSkip": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToSkip": {
            "type": "int",
            "defaultValue": 4
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[skip(parameters('testArray'),parameters('elementsToSkip'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[skip(parameters('testString'),parameters('charactersToSkip'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| arrayOutput | Array | ["three"] |
| stringOutput | String | two three |

<a id="split" />

## split
<a id="split" class="xliff"></a>
`split(inputString, delimiter)`

返回包含输入字符串的子字符串的字符串数组，其中的子字符串使用指定的分隔符进行分隔。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| inputString |是 |字符串 |要拆分的字符串。 |
| delimiter |是 |字符串或字符串数组 |用于拆分字符串的分隔符。 |

### 返回值
<a id="return-value" class="xliff"></a>

字符串数组。

### 示例
<a id="examples" class="xliff"></a>

以下示例使用逗号以及使用逗号或分号拆分输入字符串。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstString": {
            "type": "string",
            "defaultValue": "one,two,three"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "one;two,three"
        }
    },
    "variables": {
        "delimiters": [ ",", ";" ]
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "array",
            "value": "[split(parameters('firstString'),',')]"
        },
        "secondOutput": {
            "type": "array",
            "value": "[split(parameters('secondString'),variables('delimiters'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| firstOutput | Array | ["one", "two", "three"] |
| secondOutput | Array | ["one", "two", "three"] |

<a id="startswith" />

## startsWith
<a id="startswith" class="xliff"></a>
`startsWith(stringToSearch, stringToFind)`

确定某个字符串是否以某个值开头。 比较不区分大小写。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| stringToSearch |是 |字符串 |包含要查找的项的值。 |
| stringToFind |是 |字符串 |要查找的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

如果字符串的最前面一个或多个字符与该值匹配，则为 **True**；否则为 **False**。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何使用 startsWith 和 endsWith 函数：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| startsTrue | Bool | True |
| startsCapTrue | Bool | True |
| startsFalse | Bool | False |
| endsTrue | Bool | True |
| endsCapTrue | Bool | True |
| endsFalse | Bool | False |

<a id="string" />

## 字符串
<a id="string" class="xliff"></a>
`string(valueToConvert)`

将指定的值转换为字符串。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| valueToConvert |是 | 任意 |要转换为字符串的值。 可以转换任何类型的值，包括对象和数组。 |

### 返回值
<a id="return-value" class="xliff"></a>

转换后的值的字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何将不同类型的值转换为字符串：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testObject": {
            "type": "object",
            "defaultValue": {
                "valueA": 10,
                "valueB": "Example Text"
            }
        },
        "testArray": {
            "type": "array",
            "defaultValue": [
                "a",
                "b",
                "c"
            ]
        },
        "testInt": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "resources": [],
    "outputs": {
        "objectOutput": {
            "type": "string",
            "value": "[string(parameters('testObject'))]"
        },
        "arrayOutput": {
            "type": "string",
            "value": "[string(parameters('testArray'))]"
        },
        "intOutput": {
            "type": "string",
            "value": "[string(parameters('testInt'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| objectOutput | String | {"valueA":10,"valueB":"Example Text"} |
| arrayOutput | String | ["a","b","c"] |
| intOutput | String | 5 |

<a id="substring" />

## substring
<a id="substring" class="xliff"></a>
`substring(stringToParse, startIndex, length)`

返回从指定的字符位置开始且包含指定数量的字符的子字符串。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| stringToParse |是 |字符串 |从中提取子字符串的原始字符串。 |
| startIndex |否 |int |子字符串的从零开始的字符位置。 |
| length |否 |int |子字符串的字符数。 必须引用该字符串内的一个位置。 |

### 返回值
<a id="return-value" class="xliff"></a>

子字符串。

### 备注
<a id="remarks" class="xliff"></a>

如果子字符串超出了字符串的末尾，该函数将会失败。 以下示例将失败，并出现错误“索引和长度参数必须引用字符串内的一个位置。 索引参数“0”，长度参数“11”，字符串参数长度“10”。”。

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### 示例
<a id="examples" class="xliff"></a>

以下示例从参数中提取子字符串。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        }
    },
    "resources": [],
    "outputs": {
        "substringOutput": {
            "value": "[substring(parameters('testString'), 4, 3)]",
            "type": "string"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| substringOutput | String | two |

<a id="take" />

## take
<a id="take" class="xliff"></a>
`take(originalValue, numberToTake)`

返回一个字符串，其中包含从字符串开头位置算起的指定数目的字符；或返回一个数组，其中包含从数组开头位置算起的指定数目的元素。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| originalValue |是 |数组或字符串 |要从中提取元素的数组或字符串。 |
| numberToTake |是 |int |要提取的元素或字符数。 如果此值为 0 或更小，则返回一个空数组或字符串。 如果此值大于给定数组或字符串的长度，则返回数组或字符串中的所有元素。 |

### 返回值
<a id="return-value" class="xliff"></a>

数组或字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例从数组中提取指定数目的元素，并从字符串中提取指定数目的字符。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToTake": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToTake": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[take(parameters('testArray'),parameters('elementsToTake'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[take(parameters('testString'),parameters('charactersToTake'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| arrayOutput | Array | ["one", "two"] |
| stringOutput | String | on |

<a id="tolower" />

## toLower
<a id="tolower" class="xliff"></a>
`toLower(stringToChange)`

将指定的字符串转换为小写。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| stringToChange |是 |字符串 |要转换为小写的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

已转换为小写的字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例将参数值转换为小写和大写。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| toLowerOutput | String | one two three |
| toUpperOutput | String | ONE TWO THREE |

<a id="toupper" />

## toUpper
<a id="toupper" class="xliff"></a>
`toUpper(stringToChange)`

将指定的字符串转换为大写。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| stringToChange |是 |字符串 |要转换为大写的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

已转换为大写的字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例将参数值转换为小写和大写。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| toLowerOutput | String | one two three |
| toUpperOutput | String | ONE TWO THREE |

<a id="trim" />

## trim
<a id="trim" class="xliff"></a>
`trim (stringToTrim)`

从指定的字符串中删除所有前导和尾随空白字符。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| stringToTrim |是 |字符串 |要剪裁的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

不带前导和尾随空白字符的字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例裁剪掉参数中的空白字符。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "    one two three   "
        }
    },
    "resources": [],
    "outputs": {
        "return": {
            "type": "string",
            "value": "[trim(parameters('testString'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| return | String | one two three |

<a id="uniquestring" />

## uniqueString
<a id="uniquestring" class="xliff"></a>
`uniqueString (baseString, ...)`

根据作为参数提供的值创建确定性哈希字符串。 

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| baseString |是 |字符串 |哈希函数中用于创建唯一字符串的值。 |
| 根据需要使用其他参数 |否 |字符串 |你可以添加任意数目的字符串，以创建指定唯一性级别的值。 |

### 备注
<a id="remarks" class="xliff"></a>

当你需要创建资源的唯一名称时，此函数很有帮助。 提供参数值，这些值用于限制结果的唯一性范围。 可以指定该名称对于订阅、资源组或部署是否唯一。 

返回的值不是随机字符串，而是哈希函数的结果。 返回的值长度为 13 个字符。 并非全局唯一。 可能需要根据命名约定使用前缀来组合值，以创建有意义的名称。 以下示例显示了返回值的格式。 实际值取决于提供的参数。

    tcvhiyu5h2o5o

以下示例演示如何使用 uniqueString 创建通用级别唯一值。

仅对订阅唯一

```json
"[uniqueString(subscription().subscriptionId)]"
```

仅对资源组唯一

```json
"[uniqueString(resourceGroup().id)]"
```

仅对资源组的部署唯一

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

以下示例演示显示如何根据资源组创建存储帐户的唯一名称。 在资源组中，如果构造方式相同，则名称不唯一。

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

### 返回值
<a id="return-value" class="xliff"></a>

包含 13 个字符的字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例从 uniquestring 返回结果：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "uniqueRG": {
            "value": "[uniqueString(resourceGroup().id)]",
            "type" : "string"
        },
        "uniqueDeploy": {
            "value": "[uniqueString(resourceGroup().id, deployment().name)]",
            "type" : "string"
        }
    }
}
```

<a id="uri" />

## uri
<a id="uri" class="xliff"></a>
`uri (baseUri, relativeUri)`

通过组合 baseUri 和 relativeUri 字符串来创建绝对 URI。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| baseUri |是 |字符串 |基本 uri 字符串。 |
| relativeUri |是 |字符串 |要添加到基本 uri 字符串的相对 uri 字符串。 |

**baseUri** 参数的值可包含特定文件，但在构造 URI 时，只使用基路径。 例如，将 `http://contoso.com/resources/azuredeploy.json`作为 baseUri 参数传递会生成 `http://contoso.com/resources/` 的基 URI。

### 返回值
<a id="return-value" class="xliff"></a>

表示基值和相对值的绝对 URI 的字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例演示如何根据父模板的值构造嵌套模板的链接。

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

以下示例展示了如何使用 uri、uriComponent 和 uriComponentToString：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| uriOutput | String | http://contoso.com/resources/nested/azuredeploy.json |
| componentOutput | String | http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json |
| toStringOutput | String | http://contoso.com/resources/nested/azuredeploy.json |

<a id="uricomponent" />

## uriComponent
<a id="uricomponent" class="xliff"></a>
`uricomponent(stringToEncode)`

将 URI 编码。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| stringToEncode |是 |字符串 |要编码的值。 |

### 返回值
<a id="return-value" class="xliff"></a>

URI 编码值的字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何使用 uri、uriComponent 和 uriComponentToString：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| uriOutput | String | http://contoso.com/resources/nested/azuredeploy.json |
| componentOutput | String | http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json |
| toStringOutput | String | http://contoso.com/resources/nested/azuredeploy.json |

<a id="uricomponenttostring" />

## uriComponentToString
<a id="uricomponenttostring" class="xliff"></a>
`uriComponentToString(uriEncodedString)`

返回 URI 编码值的字符串。

### 参数
<a id="parameters" class="xliff"></a>

| 参数 | 必选 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| uriEncodedString |是 |字符串 |要转换为字符串的 URI 编码值。 |

### 返回值
<a id="return-value" class="xliff"></a>

URI 编码值的解码后字符串。

### 示例
<a id="examples" class="xliff"></a>

以下示例展示了如何使用 uri、uriComponent 和 uriComponentToString：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

采用默认值，前面示例的输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| uriOutput | String | http://contoso.com/resources/nested/azuredeploy.json |
| componentOutput | String | http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json |
| toStringOutput | String | http://contoso.com/resources/nested/azuredeploy.json |

## 后续步骤
<a id="next-steps" class="xliff"></a>
* 有关 Azure Resource Manager 模板中各部分的说明，请参阅[创作 Azure Resource Manager 模板](resource-group-authoring-templates.md)。
* 若要合并多个模板，请参阅[将链接的模板与 Azure Resource Manager 配合使用](resource-group-linked-templates.md)。
* 若要在创建资源类型时迭代指定的次数，请参阅[在 Azure Resource Manager 中创建多个资源实例](resource-group-create-multiple.md)。
* 若要查看如何部署已创建的模板，请参阅[使用 Azure Resource Manager 模板部署应用程序](resource-group-template-deploy.md)。