---
title: 为 Azure Functions 中的函数应用自动执行资源部署 | Microsoft Docs
description: 了解如何生成用于部署函数应用的 Azure 资源管理器模板。
services: Functions
documtationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure functions, functions 无服务体系结构, 基础结构即代码, azure resource manager
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.service: azure-functions
ms.server: functions
ms.devlang: multiple
ms.topic: conceptual
origin.date: 04/03/2019
ms.date: 06/04/2019
ms.author: v-junlch
ms.openlocfilehash: 886f9c6dbb6694d8553e3187c861d891ccc40a14
ms.sourcegitcommit: 9e839c50ac69907e54ddc7ea13ae673d294da77a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491452"
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a>为 Azure Functions 中的函数应用自动执行资源部署

可以使用 Azure 资源管理器模板来部署函数应用。 本文概述了完成此操作所需的资源和参数。 可能还需要部署其他资源，具体取决于函数应用中的[触发器和绑定](functions-triggers-bindings.md)。

有关创建模板的详细信息，请参阅[创作 Azure 资源管理器模板](../azure-resource-manager/resource-group-authoring-templates.md)。

有关示例模板，请参阅：
- [基于消耗计划的函数应用]
- [基于 Azure 应用服务计划的函数应用]

## <a name="required-resources"></a>所需资源

Azure Functions 部署通常包括以下资源：

| 资源                                                                           | 要求 | 语法和属性参考                                                         |   |
|------------------------------------------------------------------------------------|-------------|-----------------------------------------------------------------------------------------|---|
| 函数应用                                                                     | 必需    | [Microsoft.Web/sites](https://docs.microsoft.com/azure/templates/microsoft.web/sites)                             |   |
| [Azure 存储](../storage/index.yml)帐户                                   | 必需    | [Microsoft.Storage/storageAccounts](https://docs.microsoft.com/azure/templates/microsoft.storage/storageaccounts) |   |
| [托管计划](./functions-scale.md)                                             | 可选<sup>1</sup>    | [Microsoft.Web/serverfarms](https://docs.microsoft.com/azure/templates/microsoft.web/serverfarms)                 |   |

<sup>1</sup>只有选择在[应用服务计划](../app-service/overview-hosting-plans.md)上运行你的函数应用时，托管计划才是必需的。

<a name="storage"></a>
### <a name="storage-account"></a>存储帐户

函数应用需要 Azure 存储帐户。 你需要一个支持 blob、表、队列和文件的常规用途帐户。 有关详细信息，请参阅 [Azure Functions 存储帐户要求](functions-create-function-app-portal.md#storage-account-requirements)。

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2018-07-01",
    "location": "[resourceGroup().location]",
    "kind": "StorageV2",
    "properties": {
        "accountType": "[parameters('storageAccountType')]"
    }
}
```

此外，在站点配置中，必须将属性 `AzureWebJobsStorage` 指定为应用设置。 函数应用应将 `AzureWebJobsDashboard` 指定为应用设置。

Azure Functions 运行时使用 `AzureWebJobsStorage` 连接字符串创建内部队列。 运行时使用 `AzureWebJobsDashboard` 连接字符串登录到 Azure 表存储并为为门户中的“监视器”  选项卡提供支持。

这些属性在 `siteConfig` 对象中的 `appSettings` 集合中指定：

```json
"appSettings": [
    {
        "name": "AzureWebJobsStorage",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';EndpointSuffix=core.chinacloudapi.cn;AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    },
    {
        "name": "AzureWebJobsDashboard",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';EndpointSuffix=core.chinacloudapi.cn;AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    }
]
```

### <a name="hosting-plan"></a>托管计划

托管计划的定义是变化的，并且可能是下列项之一：
* [消耗计划](#consumption)（默认值）
* [应用服务计划](#app-service-plan)

### <a name="function-app"></a>函数应用

函数应用资源是使用类型为 **Microsoft.Web/sites** 且种类为 **functionapp** 的资源定义的：

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Insights/components', variables('appInsightsName'))]"
    ]
```

> [!IMPORTANT]
> 如果你要显式定义托管计划，则 dependsOn 数组中可能将需要一个额外的项：`"[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"`

函数应用必须包括以下应用程序设置：

| 设置名称                 | 说明                                                                               | 示例值                        |
|------------------------------|-------------------------------------------------------------------------------------------|---------------------------------------|
| AzureWebJobsStorage          | Functions 运行时用于内部排队的存储帐户的连接字符串 | 请参阅[存储帐户](#storage)       |
| FUNCTIONS_EXTENSION_VERSION  | Azure Functions 运行时的版本                                                | `~2`                                  |
| FUNCTIONS_WORKER_RUNTIME     | 要为此应用中的函数使用的语言堆栈                                   | `dotnet`、`node`、`java` 或 `python` |
| WEBSITE_NODE_DEFAULT_VERSION | 只有当使用 `node` 语言堆栈时必需，指定要使用的版本              | `10.14.1`                             |

这些属性是在 `siteConfig` 属性中的 `appSettings` 集合中指定的：

```json
"properties": {
    "siteConfig": {
        "appSettings": [
            {
                "name": "AzureWebJobsStorage",
                "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';EndpointSuffix=core.chinacloudapi.cn;AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
            },
            {
                "name": "FUNCTIONS_WORKER_RUNTIME",
                "value": "node"
            },
            {
                "name": "WEBSITE_NODE_DEFAULT_VERSION",
                "value": "10.14.1"
            },
            {
                "name": "FUNCTIONS_EXTENSION_VERSION",
                "value": "~2"
            }
        ]
    }
}
```

<a name="consumption"></a>

## <a name="deploy-on-consumption-plan"></a>在消耗计划上部署

代码运行时，消耗计划会自动分配计算能力，根据处理负载的需要进行扩展，然后在代码停止运行时进行缩减。 你不需要为空闲的 VM 付费，也不需要提前保留容量。 若要了解详细信息，请参阅 [Azure Functions 的缩放和托管](functions-scale.md#consumption-plan)。

有关 Azure 资源管理器模板示例，请参阅[基于消耗计划的函数应用]。

### <a name="create-a-consumption-plan"></a>创建消耗计划

不需要定义消耗计划。 创建函数应用资源本身时，不会基于区域自动创建或选择消耗计划。

消耗计划是一种特殊的“serverfarm”资源。 对于 Windows，可以通过为 `computeMode` 和 `sku` 属性使用 `Dynamic` 值来指定它：

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "computeMode": "Dynamic",
        "sku": "Dynamic"
    }
}
```

> [!NOTE]
> 不能显式为 Linux 定义消耗计划。 它将自动创建。

如果你显式定义你的消耗计划，则需要在应用上设置 `serverFarmId` 属性，以使其指向计划的资源 ID。 你还应当确保函数应用有一个针对该计划的 `dependsOn` 设置。

### <a name="create-a-function-app"></a>创建函数应用

#### <a name="windows"></a>Windows

在 Windows 上，消耗计划还需要站点配置中的两个附加设置：`WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` 和 `WEBSITE_CONTENTSHARE`。 这些属性用于配置存储函数应用代码和配置的存储帐户和文件路径。

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTSHARE",
                    "value": "[toLower(variables('functionAppName'))]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```                    


<a name="app-service-plan"></a> 

## <a name="deploy-on-app-service-plan"></a>在应用服务计划上部署

在应用服务计划中，函数应用在基本、标准和高级 SKU 中的专用 VM 上运行，类似于 Web 应用。 如需详细了解如何使用应用服务计划，请参阅 [Azure 应用服务计划深入概述](../app-service/overview-hosting-plans.md)。

有关 Azure 资源管理器模板示例，请参阅[基于 Azure 应用服务计划的函数应用]。

### <a name="create-an-app-service-plan"></a>创建应用服务计划

应用服务计划是由“serverfarm”资源定义的。

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "[parameters('sku')]",
        "workerSize": "[parameters('workerSize')]",
        "hostingEnvironment": "",
        "numberOfWorkers": 1
    }
}
```

### <a name="create-a-function-app"></a>创建函数应用 

应用服务计划上的函数应用必须将 `serverFarmId` 属性设置为之前创建的计划的资源 ID。

```json
{
    "apiVersion": "2016-03-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';EndpointSuffix=core.chinacloudapi.cn;AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "FUNCTIONS_WORKER_RUNTIME",
                    "value": "node"
                },
                {
                    "name": "WEBSITE_NODE_DEFAULT_VERSION",
                    "value": "10.14.1"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~2"
                }
            ]
        }
    }
}
```

## <a name="deploy-your-template"></a>部署模板

可以使用以下任意方法部署模板：

* [PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
* [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [Azure 门户](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-to-azure-button"></a>“部署到 Azure”按钮

将 ```<url-encoded-path-to-azuredeploy-json>``` 替换为 GitHub 中 `azuredeploy.json` 文件的原始路径的 [URL 编码](https://www.bing.com/search?q=url+encode)版本。

以下示例使用 Markdown：

```markdown
[![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.cn/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

以下示例使用 HTML：

```html
<a href="https://portal.azure.cn/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"></a>
```

### <a name="deploy-using-powershell"></a>使用 PowerShell 进行部署

以下 PowerShell 命令创建一个资源组并部署一个模板，该模板创建函数应用及其必需的资源。 若要在本地运行，必须安装 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)。 运行 [`Connect-AzAccount -Environment AzureChinaCloud`](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) 进行登录。

```powershell
# Register Resource Providers if they're not already registered
Register-AzResourceProvider -ProviderNamespace "microsoft.web"
Register-AzResourceProvider -ProviderNamespace "microsoft.storage"

# Create a resource group for the function app
New-AzResourceGroup -Name "MyResourceGroup" -Location 'China North'

# Create the parameters for the file, which for this template is the function app name.
$TemplateParams = @{"appName" = "<function-app-name>"}

# Deploy the template
New-AzResourceGroupDeployment -ResourceGroupName "MyResourceGroup" -TemplateFile template.json -TemplateParameterObject $TemplateParams -Verbose
```

若要测试此部署，可以使用[这样的模板](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-function-app-create-dynamic/azuredeploy.json)，该模板在消耗计划中在 Windows 上创建函数应用。 将 `<function-app-name>` 替换为你的函数应用的唯一名称。

## <a name="next-steps"></a>后续步骤

深入了解如何开发和配置 Azure Functions。

* [Azure Functions 开发人员参考](functions-reference.md)
* [如何配置 Azure 函数应用设置](functions-how-to-use-azure-function-app-settings.md)
* [创建第一个 Azure 函数](functions-create-first-azure-function.md)

<!-- LINKS -->

[基于消耗计划的函数应用]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[基于 Azure 应用服务计划的函数应用]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json

<!-- Update_Description: wording update -->