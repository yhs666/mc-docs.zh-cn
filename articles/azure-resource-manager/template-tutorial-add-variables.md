---
title: 教程 - 将变量添加到模板
description: 将变量添加到 Azure 资源管理器模板以简化语法。
author: rockboyfor
origin.date: 10/04/2019
ms.date: 11/25/2019
ms.topic: tutorial
ms.author: v-yeche
ms.openlocfilehash: 00866fe7acfbf26ea62fbe2117b0348ae24c5980
ms.sourcegitcommit: c5e012385df740bf4a326eaedabb987314c571a1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74203683"
---
# <a name="tutorial-add-variables-to-your-resource-manager-template"></a>教程：将变量添加到资源管理器模板

本教程介绍如何将变量添加到模板。 变量可以简化模板。有了变量，你只需编写一次表达式，然后即可在模板中重复使用该表达式。 完成本教程需要 **7 分钟**。

## <a name="prerequisites"></a>先决条件

建议完成[有关函数的教程](template-tutorial-add-functions.md)，但这不是必需的。

必须有包含资源管理器工具扩展的 Visual Studio Code，以及 Azure PowerShell 或 Azure CLI。 有关详细信息，请参阅[模板工具](template-tutorial-create-first-template.md#get-tools)。

## <a name="review-your-template"></a>检查模板

在上一教程的末尾，模板有以下 JSON：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string",
            "minLength": 3,
            "maxLength": 24
        },
        "storageSKU": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
                "Standard_LRS",
                "Standard_GRS",
                "Standard_RAGRS",
                "Premium_LRS",
                "Premium_ZRS",
                "Standard_GZRS",
                "Standard_RAGZRS"
            ]
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-04-01",
            "name": "[parameters('storageName')]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('storageSKU')]"
            },
            "kind": "StorageV2",
            "properties": {
                "supportsHttpsTrafficOnly": true
            }
        }
    ]
}
```

存储帐户名称的参数难以使用，因为你必须提供唯一名称。 如果已完成本系列中的前述教程，你可能已厌倦了猜测某个唯一的名称。 若要解决此问题，可以添加一个变量，以便为存储帐户构造唯一名称。

## <a name="use-variable"></a>使用变量

以下示例突出显示了在将变量添加到模板时所做的更改，该模板用于创建唯一的存储帐户名称。 复制整个文件，将模板替换为该文件的内容。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "type": "string",
      "minLength": 3,
      "maxLength": 11
    },
    "storageSKU": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Standard_ZRS",
        "Premium_LRS",
        "Premium_ZRS",
        "Standard_GZRS",
        "Standard_RAGZRS"
      ]
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    },
    "appServicePlanName": {
      "type": "string",
      "defaultValue": "exampleplan"
    },
    "webAppName": {
      "type": "string",
      "metadata": {
        "description": "Base name of the resource such as web app name and app service plan "
      },
      "minLength": 2
    },
    "linuxFxVersion": {
      "type": "string",
      "defaultValue": "php|7.0",
      "metadata": {
        "description": "The Runtime stack of current web app"
      }
    },
    "resourceTags": {
        "type": "object",
        "defaultValue": {
            "Environment": "Dev",
            "Project": "Tutorial"
        }
    }
  },
  "variables": {
    "uniqueStorageName": "[concat(parameters('storagePrefix'), uniqueString(resourceGroup().id))]",
    "webAppPortalName": "[concat(parameters('webAppName'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[variables('uniqueStorageName')]",
      "location": "[parameters('location')]",
      "tags": "[parameters('resourceTags')]",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "StorageV2",
      "properties": {
        "supportsHttpsTrafficOnly": true
      }
    },
    {
      "type": "Microsoft.Web/serverfarms",
      "apiVersion": "2016-09-01",
      "name": "[parameters('appServicePlanName')]",
      "location": "[parameters('location')]",
      "tags": "[parameters('resourceTags')]",
      "sku": {
        "name": "B1",
        "tier": "Basic",
        "size": "B1",
        "family": "B",
        "capacity": 1
      },
      "kind": "linux",
      "properties": {
        "perSiteScaling": false,
        "reserved": true,
        "targetWorkerCount": 0,
        "targetWorkerSizeId": 0
      }
    },
    {
      "type": "Microsoft.Web/sites",
      "apiVersion": "2016-08-01",
      "kind": "app",
      "name": "[variables('webAppPortalName')]",
      "location": "[parameters('location')]",
      "tags": "[parameters('resourceTags')]",
      "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('appServicePlanName'))]",
        "siteConfig": {
          "linuxFxVersion": "[parameters('linuxFxVersion')]"
        }
      },
      "dependsOn": [
        "[parameters('appServicePlanName')]"
      ]
    }
  ],
  "outputs": {
    "storageEndpoint": {
      "type": "object",
      "value": "[reference(variables('uniqueStorageName')).primaryEndpoints]"
    }
  }
}
```

请注意，它包含一个名为 **uniqueStorageName** 的变量。 此变量使用四个函数来构造一个字符串值。

你已熟悉 [parameters](resource-group-template-functions-deployment.md#parameters) 函数，因此我们不详细介绍它。

你也熟悉 [resourceGroup](resource-group-template-functions-resource.md#resourcegroup) 函数。 在此示例中，你获得 **id** 属性而不是 **location** 属性，如上一教程所示。 **id** 属性返回资源组的完整标识符，包括订阅 ID 和资源组名称。

[uniqueString](resource-group-template-functions-string.md#uniquestring) 函数创建一个 13 个字符的哈希值。 返回的值取决于传入的参数。 在本教程中，我们使用资源组 ID 作为哈希值的输入。 这意味着，我们可以将该模板部署到不同的资源组，获取不同的唯一字符串值。 但是，如果部署到同一资源组，则获得同一值。

[concat](resource-group-template-functions-string.md#concat) 函数采用多个值并对其进行组合。 就此变量来说，它采用参数中的字符串和 uniqueString 函数中的字符串，并将二者组合成一个字符串。

可以通过 **storagePrefix** 参数传入一个用于标识存储帐户的前缀。 可以创建你自己的命名约定，以便在从长的资源列表完成部署后，通过该约定轻松地标识存储帐户。

最后请注意，存储帐户名称现在设置为变量而非参数。

## <a name="deploy-the-template"></a>部署模板

让我们部署此模板。 部署此模板比部署上述模板容易，因为你只提供存储名称的前缀。

如果尚未创建资源组，请参阅[创建资源组](template-tutorial-create-first-template.md#create-resource-group)。 此示例假设已根据[第一篇教程](template-tutorial-create-first-template.md#deploy-template)中所述，将 **templateFile** 变量设置为模板文件的路径。

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addnamevariable `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az group deployment create \
  --name addnamevariable \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS
```

---

## <a name="verify-the-deployment"></a>验证部署

可以通过在 Azure 门户中浏览资源组来验证部署。

1. 登录到 [Azure 门户](https://portal.azure.cn)。
1. 在左侧菜单中选择“资源组”。 
1. 选择已将内容部署到其中的资源组。
1. 可以看到，我们已部署一项存储帐户资源。 该存储帐户的名称为 **store** 加上一个由随机字符组成的字符串。

## <a name="clean-up-resources"></a>清理资源

若要继续学习下一篇教程，则不需删除该资源组。

如果你不打算继续学习，请删除该资源组以清理部署的资源。

1. 在 Azure 门户上的左侧菜单中选择“资源组”  。
2. 在“按名称筛选”字段中输入资源组名称。 
3. 选择资源组名称。
4. 在顶部菜单中选择“删除资源组”。 

## <a name="next-steps"></a>后续步骤

在本教程中，我们添加了一个变量，用于创建存储帐户的唯一名称。 在下一教程中，我们从已部署的存储帐户返回一个值。

> [!div class="nextstepaction"]
> [添加输出](template-tutorial-add-outputs.md)

<!-- Update_Description: new article about template tutorial add variables -->
<!--NEW.date: 11/25/2019-->