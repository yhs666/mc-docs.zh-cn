---
title: "将 Azure 资源部署到多个资源组 | Azure"
description: "介绍了在部署期间如何将多个 Azure 资源组作为目标。"
services: azure-resource-manager
documentationcenter: na
author: rockboyfor
manager: digimobile
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 06/15/2017
ms.date: 07/03/2017
ms.author: v-yeche
ms.openlocfilehash: ae200dc916e261c225e078435a947acac328f2a3
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# <a name="deploy-azure-resources-to-more-than-one-resource-group"></a>将 Azure 资源部署到多个资源组

通常情况下，将模板中的所有资源部署到单个资源组。 不过，在某些情况下，你可能希望将一组资源一起部署但将其放置在不同的资源组中。 例如，你可能希望将 Azure Site Recovery 的备份虚拟机部署到一个单独的资源组和位置。 Resource Manager 允许你使用嵌套的模板将不同于父模板所用资源组的多个不同资源组作为目标。

资源组是应用程序及其资源集合的生命周期容器。 可在模板外部创建资源组，并在部署期间指定要用作目标的资源组。 有关资源组的简介，请参阅 [Azure Resource Manager 概述](resource-group-overview.md)。

## <a name="example-template"></a>示例模板

若要将不同的资源作为目标，在部署期间必须使用嵌套模板或链接模板。 `Microsoft.Resources/deployments` 资源类型提供 `resourceGroup` 参数，使用该参数可以为嵌套部署指定不同资源组。 在运行部署之前，所有资源组都必须存在。 下面的示例部署两个存储帐户 - 一个在部署期间指定的资源组中，另一个在名为 `crossResourceGroupDeployment` 的资源组中：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "StorageAccountName1": {
            "type": "string"
        },
        "StorageAccountName2": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "crossResourceGroupDeployment",
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
                            "name": "[parameters('StorageAccountName2')]",
                            "apiVersion": "2015-06-15",
                            "location": "China North",
                            "properties": {
                                "accountType": "Standard_LRS"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('StorageAccountName1')]",
            "apiVersion": "2015-06-15",
            "location": "China North",
            "properties": {
                "accountType": "Standard_LRS"
            }
        }
    ]
}
```

如果将 `resourceGroup` 设置为不存在的资源组的名称，则部署将失败。 如果没有为 `resourceGroup` 提供值，则 Resource Manager 将使用父资源组。  

## <a name="deploy-the-template"></a>部署模板

若要部署示例模板，可以使用门户、Azure PowerShell 或 Azure CLI。 对于 Azure PowerShell 或 Azure CLI，必须使用 2017 年 5 月或之后发布的版本。 这些示例假定已在本地将模板保存为名为 **crossrgdeployment.json** 的文件。

对于 PowerShell：

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud

New-AzureRmResourceGroup -Name mainResourceGroup -Location "China East"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "China North"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

对于 Azure CLI：

```azurecli
az login

az group create --name mainResourceGroup --location "China East"
az group create --name crossResourceGroupDeployment --location "China North"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

在部署完成后，可以看到两个资源组。 每个资源组都包含一个存储帐户。

## <a name="use-resourcegroup-function"></a>使用 resourceGroup() 函数

对于跨资源组部署，[resouceGroup() 函数](resource-group-template-functions-resource.md#resourcegroup)将根据嵌套模板的指定方式不同地进行解析。 

如果在一个模板中嵌入另一个模板，则嵌套模板中的 resouceGroup() 会解析为父资源组。 嵌入模板使用以下格式：

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers to parent resource group
    }
}
```

如果链接到单独的模板，链接模板中的 resouceGroup() 会解析为嵌套资源组。 链接模板使用以下格式：

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers to linked resource group
    }
}
```

## <a name="next-steps"></a>后续步骤

* 若要了解如何在模板中定义参数，请参阅[了解 Azure Resource Manager 模板的结构和语法](resource-group-authoring-templates.md)。
* 有关解决常见部署错误的提示，请参阅[排查使用 Azure Resource Manager 时的常见 Azure 部署错误](resource-manager-common-deployment-errors.md)。
* 有关部署需要 SAS 令牌的模板的信息，请参阅[使用 SAS 令牌部署专用模板](resource-manager-powershell-sas-token.md)。