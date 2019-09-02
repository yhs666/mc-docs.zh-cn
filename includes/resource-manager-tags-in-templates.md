---
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: include
origin.date: 03/11/2019
ms.date: 04/15/2019
ms.author: v-yeche
ms.openlocfilehash: 9c75f00ab3c605f82889d05207477d0e00222d53
ms.sourcegitcommit: 9f7a4bec190376815fa21167d90820b423da87e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "59532681"
---
若要在部署过程中标记资源，请将 `tags` 元素添加到要部署的资源。 提供标记名称和值。

### <a name="apply-a-literal-value-to-the-tag-name"></a>将文本值应用到标记名称
以下示例显示了具有设置为文本值的两个标记（`Dept` 和 `Environment`）的存储帐户：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

若要设置日期/时间值的标记，请使用 [utcNow 函数](../articles/azure-resource-manager/resource-group-template-functions-string.md#utcnow)。

### <a name="apply-an-object-to-the-tag-element"></a>将对象应用到标记元素
可以定义一个存储多个标记的对象参数，并将该对象应用于标记元素。 对象中的每个属性会成为资源的单独标记。 以下示例有一个名为 `tagValues` 的参数，该标记应用于标记元素。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "tagValues": {
      "type": "object",
      "defaultValue": {
        "Dept": "Finance",
        "Environment": "Production"
      }
    }
  },
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": "[parameters('tagValues')]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {}
    }
  ]
}
```

### <a name="apply-a-json-string-to-the-tag-name"></a>将 JSON 字符串应用到标记名称

要将多个值存储在单个标记中，请应用表示这些值的 JSON 字符串。 整个 JSON 字符串存储为一个标记，该标记不能超过 256 个字符。 以下示例有一个名为 `CostCenter` 的标记，其中包含 JSON 字符串中的多个值：  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": {
        "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

<!--Update_Description: update meta properties -->
