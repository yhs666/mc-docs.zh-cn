---
title: Azure 资源管理器模板变量 | Azure
description: 介绍如何使用声明性 JSON 语法在 Azure 资源管理器模板中定义变量。
services: azure-resource-manager
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 12/12/2017
ms.date: 12/25/2017
ms.author: v-yeche
ms.openlocfilehash: 05204857909c60041fcac251a3ccebdb216cf2e0
ms.sourcegitcommit: 3e0cad765e3d8a8b121ed20b6814be80fedee600
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/22/2017
ms.locfileid: "27162896"
---
# <a name="variables-section-of-azure-resource-manager-templates"></a>Azure 资源管理器模板的 variables 节
在 variables 节中构造可在整个模板中使用的值。 不需要定义变量，但使用变量可以减少复杂的表达式，从而简化模板。

## <a name="define-and-use-a-variable"></a>定义和使用变量

以下示例显示了一个变量定义。 它创建适用于存储帐户名称的字符串值。 它使用多个模板函数来获取参数值，并将其连接到唯一字符串。

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

在定义资源时使用该变量。

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    ...
```

## <a name="available-definitions"></a>可用定义

前面的示例介绍了一种用于定义变量的方法。 可以使用下面的任意定义：

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    },
    "<variable-object-name>": {
        "copy": [
            {
                "name": "<name-of-array-property>",
                "count": <number-of-iterations>,
                "input": {
                    <properties-to-repeat>
                }
            }
        ]
    },
    "copy": [
        {
            "name": "<variable-array-name>",
            "count": <number-of-iterations>,
            "input": {
                <properties-to-repeat>
            }
        }
    ]
}
```

## <a name="configuration-variables"></a>配置变量

可以使用复杂的 JSON 类型为环境定义相关值。 

```json
"variables": {
    "environmentSettings": {
        "test": {
            "instanceSize": "Small",
            "instanceCount": 1
        },
        "prod": {
            "instanceSize": "Large",
            "instanceCount": 4
        }
    }
},
```

可以在参数中创建一个值，用于指示要使用的配置值。

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
```

可以使用以下命令检索当前设置：

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

## <a name="use-copy-element-in-variable-definition"></a>在变量定义中使用 copy 元素

你可以使用 copy 语法创建带有多个元素数组的变量。 为元素数量提供一个数字。 每个元素的属性都包含在 input 对象中。 你可以在变量中使用 copy，或用其创建变量。 定义某个变量并在该变量中使用 **copy** 时，可以创建一个具有数组属性的对象。 在顶级使用 **copy** 并在其中定义一个或多个变量时，可以创建一个或多个数组。 下例说明了这两种方法：

```json
"variables": {
    "disk-array-on-object": {
        "copy": [
            {
                "name": "disks",
                "count": 3,
                "input": {
                    "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
                    "diskSizeGB": "1",
                    "diskIndex": "[copyIndex('disks')]"
                }
            }
        ]
    },
    "copy": [
        {
            "name": "disks-top-level-array",
            "count": 3,
            "input": {
                "name": "[concat('myDataDisk', copyIndex('disks-top-level-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('disks-top-level-array')]"
            }
        }
    ]
},
```

变量 **disk-array-on-object** 包含以下对象，其中的数组名为 **disks**：

```json
{
  "disks": [
    {
      "name": "myDataDisk1",
      "diskSizeGB": "1",
      "diskIndex": 0
    },
    {
      "name": "myDataDisk2",
      "diskSizeGB": "1",
      "diskIndex": 1
    },
    {
      "name": "myDataDisk3",
      "diskSizeGB": "1",
      "diskIndex": 2
    }
  ]
}
```

变量 **disks-top-level-array** 包含以下数组：

```json
[
  {
    "name": "myDataDisk1",
    "diskSizeGB": "1",
    "diskIndex": 0
  },
  {
    "name": "myDataDisk2",
    "diskSizeGB": "1",
    "diskIndex": 1
  },
  {
    "name": "myDataDisk3",
    "diskSizeGB": "1",
    "diskIndex": 2
  }
]
```

使用 copy 创建变量时，还可以指定多个对象。 以下示例将两个数组定义为变量。 一个数组名为 **disks-top-level-array**，包含五个元素。 另一个数组名为 **a-different-array**，包含三个元素。

```json
"variables": {
    "copy": [
        {
            "name": "disks-top-level-array",
            "count": 5,
            "input": {
                "name": "[concat('oneDataDisk', copyIndex('disks-top-level-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('disks-top-level-array')]"
            }
        },
        {
            "name": "a-different-array",
            "count": 3,
            "input": {
                "name": "[concat('twoDataDisk', copyIndex('a-different-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('a-different-array')]"
            }
        }
    ]
},
```

需要使用参数值并确保这些值采用了适合模板值的正确格式时，此方法很适用。 以下示例格式化了参数值，使之适用于定义安全规则：

```json
{
  "$schema": "https://schema.management.chinacloudapi.cn/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "securityRules": {
      "type": "array"
    }
  },
  "variables": {
    "copy": [
      {
        "name": "securityRules",
        "count": "[length(parameters('securityRules'))]",
        "input": {
          "name": "[parameters('securityRules')[copyIndex('securityRules')].name]",
          "properties": {
            "description": "[parameters('securityRules')[copyIndex('securityRules')].description]",
            "priority": "[parameters('securityRules')[copyIndex('securityRules')].priority]",
            "protocol": "[parameters('securityRules')[copyIndex('securityRules')].protocol]",
            "sourcePortRange": "[parameters('securityRules')[copyIndex('securityRules')].sourcePortRange]",
            "destinationPortRange": "[parameters('securityRules')[copyIndex('securityRules')].destinationPortRange]",
            "sourceAddressPrefix": "[parameters('securityRules')[copyIndex('securityRules')].sourceAddressPrefix]",
            "destinationAddressPrefix": "[parameters('securityRules')[copyIndex('securityRules')].destinationAddressPrefix]",
            "access": "[parameters('securityRules')[copyIndex('securityRules')].access]",
            "direction": "[parameters('securityRules')[copyIndex('securityRules')].direction]"
          }
        }
      }
    ]
  },
  "resources": [
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "NSG1",
      "location": "[resourceGroup().location]",
      "properties": {
        "securityRules": "[variables('securityRules')]"
      }
    }
  ],
  "outputs": {}
}
```

## <a name="recommendations"></a>建议
使用变量时，以下信息可以提供帮助：

* 针对需要在模板中多次使用的值使用变量。 如果一次只使用一个值，则硬编码值可使模板更易于阅读。
* 不能在模板的 **variables** 节中使用 [reference](resource-group-template-functions-resource.md#reference) 函数。 **reference** 函数从资源的运行时状态中派生其值。 但是，变量是在初始模板分析期间解析的。 直接在模板的 **resources** 或 **outputs** 节中构造需要 **reference** 函数的值。
* 包括的变量适用于必须唯一的资源名称。

## <a name="example-templates"></a>示例模板

以下示例模板演示了一些变量使用方案。 请部署这些模板，测试不同方案中变量的处理方式。 

|模板  |说明  |
|---------|---------|
| [变量定义](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | 演示不同类型的变量。 此模板不部署任何资源。 它构造变量值并返回这些值。 |
| [配置变量](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | 演示如何使用变量来定义配置值。 此模板不部署任何资源。 它构造变量值并返回这些值。 |
| [网络安全规则](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json)和[参数文件](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | 以正确格式构造一个数组，以便向网络安全组分配安全规则。 |

## <a name="next-steps"></a>后续步骤
* 若要查看许多不同类型的解决方案的完整模型，请参阅 [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates/)（Azure 快速入门模板）。
* 有关用户可以使用的来自模板中的函数的详细信息，请参阅 [Azure Resource Manager Template Functions](resource-group-template-functions.md)（Azure Resource Manager 模板函数）。
* 要在部署期间合并多个模板，请参阅[将已链接的模板与 Azure 资源管理器配合使用](resource-group-linked-templates.md)。
* 可能需要使用不同资源组中的资源。 使用跨多个资源组共享的存储帐户或虚拟网络时，此方案很常见。 有关详细信息，请参阅 [resourceId 函数](resource-group-template-functions-resource.md#resourceid)。
<!-- Update_Description: new articles on resource manager templates variables -->