---
title: "使用模板部署 Azure Web 应用指南"
description: "关于创建 Azure 资源管理器模板来部署 Web 应用的建议。"
services: app-service
documentationcenter: app-service
author: tfitzmac
manager: timlt
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: v-yiso
origin.date: 03/12/2018
ms.openlocfilehash: 588bf796152e025104aede85ce64a4e9316ac423
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="guidance-on-deploying-web-apps-with-azure-resource-manager-templates"></a>使用 Azure 资源管理器模板部署 Web 应用指南

本文提供关于创建 Azure 资源管理器模板来部署应用服务解决方案的建议。 这些建议可帮助你避免常见问题。

## <a name="define-dependencies"></a>定义依赖关系

定义 Web 应用的依赖关系需要了解 Web 应用内的资源如何进行交互。 如果以错误顺序指定依赖关系，可能会导致部署错误，或者创建使部署停止的争用条件。

> [!WARNING]
> 如果模板中包含 MS 部署站点扩展，则必须将任何配置资源设置为依赖于 MS 部署资源。 配置更改会导致站点以异步方式重启。 将配置资源设为依赖于 MS 部署，可确保在站点重启之前完成 MS 部署。 如果没有这些依赖关系，站点可能会在 MS 部署的部署过程中重启。 有关示例模板，请参阅[包含 Web 部署依赖关系的 Wordpress 模板](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json)。

下图显示了各种应用服务资源的依赖顺序。

![Web 应用依赖关系](media/web-sites-rm-template-guidance/web-dependencies.png)

按以下顺序部署资源：

**第 1 层**
* 应用服务计划
* 任何其他相关资源，如数据库或存储帐户

**第 2 层**
* Web 应用 - 依赖于应用服务计划
* 面向目标服务器场的 Application Insights - 依赖于应用服务计划

**第 3 层**
* 源代码管理 - 依赖于 Web 应用
* MSDeploy 站点扩展 - 依赖于 Web 应用
* 面向目标服务器场的 Application Insights - 依赖于 Web 应用

**第 4 层**
* 应用服务证书 - 依赖于源代码管理或 MSDeploy（如果任一项存在，否则依赖于 Web 应用）
* 配置设置（连接字符串、Web 配置值、应用设置）- 依赖于源代码管理或 MSDeploy（如果任一项存在，否则依赖于 Web 应用）

**第 5 层**
* 主机名绑定 - 依赖于证书（如果存在，否则依赖于更高级别资源）
* 站点扩展 - 依赖于配置设置（如果存在，否则依赖于更高级别资源）

通常，解决方案仅包括上述某些资源和层。 对于缺少的层，将较低资源映射到下一个更高层。

以下示例显示了模板的一部分。 连接字符串配置值依赖于 MSDeploy 扩展。 MSDeploy 扩展依赖于 Web 应用和数据库。

```json
{
    "name": "[parameters('name')]",
    "type": "Microsoft.Web/sites",
    "resources": [
      {
          "name": "MSDeploy",
          "type": "Extensions",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('name'))]",
            "[concat('SuccessBricks.ClearDB/databases/', parameters('databaseName'))]"
          ],
          ...
      },
      {
          "name": "connectionstrings",
          "type": "config",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('name'), '/Extensions/MSDeploy')]"
          ],
          ...
      }
    ]
}
```

## <a name="unique-web-app-name"></a>唯一的 Web 应用名称

Web 应用的名称必须全局唯一。 可以使用可能唯一的命名约定，也可以使用 [uniqueString 函数](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring)帮助生成唯一名称。

```json
{
  "apiVersion": "2016-08-01",
  "name": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
  "type": "Microsoft.Web/sites",
  ...
}
```

## <a name="next-steps"></a>后续步骤

* 有关使用模板部署 Web 应用的教程，请参阅[按可预见的方式在 Azure 中预配和部署微服务](app-service-deploy-complex-application-predictably.md)。