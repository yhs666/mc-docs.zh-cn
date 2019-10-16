---
title: Azure 资源管理器模板资源位置
description: 介绍如何在 Azure 资源管理器模板中设置资源位置。
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: conceptual
origin.date: 09/04/2019
ms.date: 09/23/2019
ms.author: v-yeche
ms.openlocfilehash: 97cf2f4f75107cc92d3bde3515e950cccdc5a6e8
ms.sourcegitcommit: 6a62dd239c60596006a74ab2333c50c4db5b62be
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2019
ms.locfileid: "71156483"
---
<!--Verify successfully-->
# <a name="set-resource-location-in-resource-manager-template"></a>在资源管理器模板中设置资源位置

部署模板时，必须提供每个资源的位置。 该位置不必与资源组位置相同。

## <a name="get-available-locations"></a>获取可用位置

不同位置支持的资源类型不一样。 若要获取资源类型支持的位置，请使用 Azure PowerShell 或 Azure CLI。

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes `
  | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az provider show \
  --namespace Microsoft.Batch \
  --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" \
  --out table
```

---

## <a name="use-location-parameter"></a>使用 location 参数

若要在部署模板时实现灵活性，请使用参数指定资源的位置。 将参数的默认值设置为 `resourceGroup().location`。

以下示例显示了部署到作为参数指定的位置的存储帐户：

<!--Not Available on  "Standard_ZRS"-->

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat('storage', uniquestring(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "apiVersion": "2018-07-01",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
  "outputs": {
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    }
  }
}
```

## <a name="next-steps"></a>后续步骤

* 有关模板函数的完整列表，请参阅 [Azure Resource Manager 模板函数](resource-group-template-functions.md)。
* 有关模板文件的详细信息，请参阅[了解 Azure 资源管理器模板的结构和语法](resource-group-authoring-templates.md)。

<!--Update_Description: new articles on resource location -->
<!--ms.date: 09/23/2019-->