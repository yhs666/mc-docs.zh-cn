---
title: 将 Azure 资源部署到多个订阅和资源组 | Azure
description: 介绍如何在部署期间将多个 Azure 订阅和资源组作为目标。
services: azure-resource-manager
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 06/02/2018
ms.date: 07/09/2018
ms.author: v-yeche
ms.openlocfilehash: 689a6daad5b5b0801a7f02f7f657cbf5851e4bc6
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52657198"
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>将 Azure 资源部署到多个订阅或资源组

通常情况下，将模板中的所有资源部署到单个[资源组](resource-group-overview.md)。 不过，在某些情况下，你可能希望将一组资源部署在一起但将其放置在不同的资源组或订阅中。 例如，你可能希望将 Azure Site Recovery 的备份虚拟机部署到一个单独的资源组和位置。 资源管理器允许使用嵌套的模板将不同于父模板所用订阅和资源组的多个不同订阅和资源组作为目标。

> [!NOTE]
> 在单个部署中可以仅部署到五个资源组。 通常情况下，此限制意味着，在嵌套或链接的部署中可以部署到为父模板指定的一个资源组和最多四个资源组。 但是，如果父模板仅包含嵌套或链接的模板，并且本身不部署任何资源，则在嵌套或链接的部署中最多可包含五个资源组。

## <a name="specify-a-subscription-and-resource-group"></a>指定订阅和资源组

若要将其他资源作为目标，请使用嵌套模板或链接模板。 `Microsoft.Resources/deployments` 资源类型提供 `subscriptionId` 和 `resourceGroup` 的参数。 使用这些属性可为嵌套部署指定不同的订阅和资源组。 在运行部署之前，所有资源组都必须存在。 如果未指定订阅 ID 或资源组，将使用父模板中的订阅和资源组。

用于部署模板的帐户必须有权部署到指定的订阅 ID。 
<!-- Not Available on [add guest users from another directory](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md) -->

若要指定其他资源组和订阅，请使用：

```json
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "resourceGroup": "[parameters('secondResourceGroup')]",
        "subscriptionId": "[parameters('secondSubscriptionID')]",
        ...
    }
]
```

如果资源组属于同一订阅，则可删除 **subscriptionId** 值。

以下示例部署两个存储帐户 - 一个在部署期间指定的资源组中，另一个在 `secondResourceGroup` 参数指定的资源组中：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        },
        "secondResourceGroup": {
            "type": "string"
        },
        "secondSubscriptionID": {
            "type": "string",
            "defaultValue": ""
        },
        "secondStorageLocation": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "firstStorageName": "[concat(parameters('storagePrefix'), uniqueString(resourceGroup().id))]",
        "secondStorageName": "[concat(parameters('storagePrefix'), uniqueString(parameters('secondSubscriptionID'), parameters('secondResourceGroup')))]"
    },
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "[parameters('secondResourceGroup')]",
            "subscriptionId": "[parameters('secondSubscriptionID')]",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[variables('secondStorageName')]",
                            "apiVersion": "2017-06-01",
                            "location": "[parameters('secondStorageLocation')]",
                            "sku":{
                                "name": "Standard_LRS"
                            },
                            "kind": "Storage",
                            "properties": {
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('firstStorageName')]",
            "apiVersion": "2017-06-01",
            "location": "[resourceGroup().location]",
            "sku":{
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {
            }
        }
    ]
}
```

如果将 `resourceGroup` 设置为不存在的资源组的名称，则部署将失败。

## <a name="use-the-resourcegroup-and-subscription-functions"></a>使用 resourceGroup() 和 subscription() 函数

对于跨资源组部署，[resourceGroup()](resource-group-template-functions-resource.md#resourcegroup) 和 [subscription()](resource-group-template-functions-resource.md#subscription) 函数根据指定嵌套模板的方式以不同的方式解析。 

如果在一个模板内嵌入另一个模板，嵌套模板中的函数会解析到父资源组和订阅。 嵌入模板使用以下格式：

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() and subscription() refer to parent resource group/subscription
    }
}
```

如果链接到单独的模板，则链接模板中的函数会解析到嵌套资源组和订阅。 链接模板使用以下格式：

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() and subscription() in linked template refer to linked resource group/subscription
    }
}
```

## <a name="example-templates"></a>示例模板

以下模板演示了多个资源组部署。 用于部署模板的脚本在表后显示。

|模板  |说明  |
|---------|---------|
|[跨订阅模板](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crosssubscription.json) |将一个存储帐户部署到一个资源组，再将一个存储帐户部署到另一个资源组。 当第二个资源组属于其他订阅时，包括一个用于订阅 ID 的值。 |
|[跨资源组属性模板](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) |演示 `resourceGroup()` 函数如何解析。 它不部署任何资源。 |

### <a name="powershell"></a>PowerShell

对于 PowerShell，若要将两个存储帐户部署到**同一订阅**中的两个资源组，请使用：

```powershell
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

New-AzureRmResourceGroup -Name $firstRG -Location chinaeast
New-AzureRmResourceGroup -Name $secondRG -Location chinaeast

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation chinaeast
```

对于 PowerShell，若要将两个存储帐户部署到**两个订阅**，请使用：

```powershell
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

$firstSub = "<first-subscription-id>"
$secondSub = "<second-subscription-id>"

Select-AzureRmSubscription -Subscription $secondSub
New-AzureRmResourceGroup -Name $secondRG -Location chinaeast

Select-AzureRmSubscription -Subscription $firstSub
New-AzureRmResourceGroup -Name $firstRG -Location chinaeast

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation chinaeast `
  -secondSubscriptionID $secondSub
```

对于 PowerShell，若要测试**资源组对象**如何针对父模板、内联模板和链接的模板进行解析，请使用：

```powershell
New-AzureRmResourceGroup -Name parentGroup -Location chinaeast
New-AzureRmResourceGroup -Name inlineGroup -Location chinaeast
New-AzureRmResourceGroup -Name linkedGroup -Location chinaeast

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

在前面的示例中，**parentRG** 和 **inlineRG** 都解析为 **parentGroup**。 **linkedRG** 解析为 **linkedGroup**。 前述示例的输出为：

```powershell
 Name             Type                       Value
 ===============  =========================  ==========
 parentRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
                                               "name": "parentGroup",
                                               "location": "chinaeast",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
 inlineRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
                                               "name": "parentGroup",
                                               "location": "chinaeast",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
 linkedRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/linkedGroup",
                                               "name": "linkedGroup",
                                               "location": "chinaeast",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
```

### <a name="azure-cli"></a>Azure CLI

对于 Azure CLI，若要将两个存储帐户部署到**同一订阅**中的两个资源组，请使用：

```azurecli
firstRG="primarygroup"
secondRG="secondarygroup"

az group create --name $firstRG --location chinaeast
az group create --name $secondRG --location chinaeast
az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=tfstorage secondResourceGroup=$secondRG secondStorageLocation=chinaeast
```

对于 Azure CLI，若要将两个存储帐户部署到**两个订阅**，请使用：

```azurecli
firstRG="primarygroup"
secondRG="secondarygroup"

firstSub="<first-subscription-id>"
secondSub="<second-subscription-id>"

az account set --subscription $secondSub
az group create --name $secondRG --location chinaeast

az account set --subscription $firstSub
az group create --name $firstRG --location chinaeast

az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=storage secondResourceGroup=$secondRG secondStorageLocation=chinaeast secondSubscriptionID=$secondSub
```

对于 Azure CLI，若要测试**资源组对象**如何针对父模板、内联模板和链接的模板进行解析，请使用：

```azurecli
az group create --name parentGroup --location chinaeast
az group create --name inlineGroup --location chinaeast
az group create --name linkedGroup --location chinaeast

az group deployment create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json 
```

在前面的示例中，**parentRG** 和 **inlineRG** 都解析为 **parentGroup**。 **linkedRG** 解析为 **linkedGroup**。 前述示例的输出为：

```azurecli
...
"outputs": {
  "inlineRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
      "location": "chinaeast",
      "name": "parentGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  },
  "linkedRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/linkedGroup",
      "location": "chinaeast",
      "name": "linkedGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  },
  "parentRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
      "location": "chinaeast",
      "name": "parentGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  }
},
...
```

## <a name="next-steps"></a>后续步骤

* 若要了解如何在模板中定义参数，请参阅[了解 Azure 资源管理器模板的结构和语法](resource-group-authoring-templates.md)。
* 有关解决常见部署错误的提示，请参阅[排查使用 Azure Resource Manager 时的常见 Azure 部署错误](resource-manager-common-deployment-errors.md)。
* 有关部署需要 SAS 令牌的模板的信息，请参阅[使用 SAS 令牌部署专用模板](resource-manager-powershell-sas-token.md)。

<!-- Update_Description: wording update, update meta properties, update link -->