---
title: Azure 资源管理器模板参数节 | Azure
description: 使用声明性 JSON 语法描述 Azure 资源管理器模板的 parameters 节。
services: azure-resource-manager
documentationcenter: na
author: rockboyfor
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 02/03/2019
ms.date: 02/18/2019
ms.author: v-yeche
ms.openlocfilehash: 7ad02250f523604e8004e7de47efc6d6a65e11df
ms.sourcegitcommit: cdcb4c34aaae9b9d981dec534007121b860f0774
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2019
ms.locfileid: "56306183"
---
# <a name="parameters-section-of-azure-resource-manager-templates"></a>Azure 资源管理器模板的 Parameters 节
在模板的 parameters 节中，可以指定在部署资源时能够输入的值。 提供针对特定环境（例如开发、测试和生产环境）定制的参数值可以自定义部署。 无需在模板中提供参数，但如果没有参数，模板始终部署具有相同名称、位置和属性的相同资源。

一个模板中最多可以有 256 个参数。 如本文中所示，可以通过使用包含多个属性的对象来减少参数的数量。

## <a name="define-and-use-a-parameter"></a>定义和使用参数

以下示例展示了一个简单的参数定义。 其中定义了参数的名称，并指定该参数要采用字符串值。 该参数仅接受对其预期用途有意义的值。 如果在部署过程未提供任何值时，则它会指定默认值。 最后，该参数包含其用途的说明。 

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }   
}
```

在模板中，可以使用以下语法引用参数值：

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="available-properties"></a>可用属性

前面的示例仅显示了可在参数节中使用的一部分属性。 可用属性包括：

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-the parameter>" 
        }
    }
}
```

| 元素名称 | 必须 | 说明 |
|:--- |:--- |:--- |
| parameterName |是 |参数的名称。 必须是有效的 JavaScript 标识符。 |
| type |是 |参数值的类型。 允许的类型和值为 **string**、**securestring**、**int**、**bool**、**object**、**secureObject** 和 **array**。 |
| defaultValue |否 |参数的默认值，如果没有为参数提供任何值。 |
| allowedValues |否 |用来确保提供正确值的参数的允许值数组。 |
| minValue |否 |int 类型参数的最小值，此值是包容性的。 |
| maxValue |否 |int 类型参数的最大值，此值是包容性的。 |
| minLength |否 |string、secure string 和 array 类型参数的最小长度，此值是包容性的。 |
| maxLength |否 |string、secure string 和 array 类型参数的最大长度，此值是包容性的。 |
| 说明 |否 |通过门户向用户显示的参数的说明。 有关详细信息，请参阅[模板中的注释](resource-group-authoring-templates.md#comments)。 |

## <a name="template-functions-with-parameters"></a>包含参数的模板函数

为参数指定默认值时，可以使用大多数模板函数。 可以使用另一个参数值生成默认值。 以下模板演示了如何在默认值中使用函数：

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

不能在 parameters 节中使用 `reference` 函数。 参数在部署之前进行评估，因此，`reference` 函数无法获取资源的运行时状态。 

## <a name="objects-as-parameters"></a>用作参数的对象

将相关值作为对象传入可以更轻松地对其进行组织。 此方法还可以减少模板中的参数数目。

在模板中定义参数，并在部署过程中指定 JSON 对象，而不是单个值。 

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "chinaeast",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

然后，使用点运算符引用参数的子属性。

```json
"resources": [
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="example-templates"></a>示例模板

以下示例模板演示了一些参数使用方案。 请部署这些模板，测试不同方案中参数的处理方式。

|模板  |说明  |
|---------|---------|
|[用于默认值的参数与函数](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | 演示在定义参数的默认值时如何使用模板函数。 该模板不部署任何资源。 它构造参数值并返回这些值。 |
|[参数对象](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | 演示如何使用参数的对象。 该模板不部署任何资源。 它构造参数值并返回这些值。 |

## <a name="next-steps"></a>后续步骤

* 若要查看许多不同类型的解决方案的完整模型，请参阅 [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates/)（Azure 快速入门模板）。
* 若要了解如何在部署过程中输入参数值，请参阅 [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md)（使用 Azure Resource Manager 模板部署应用程序）。 
* 有关创建模板的建议，请参阅 [Azure 资源管理器模板的最佳做法](template-best-practices.md)。
* 有关使用参数对象的信息，请参阅[将对象用作 Azure 资源管理器模板中的参数](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters)。

<!-- Update_Description: update meta properties, wording update, update link -->