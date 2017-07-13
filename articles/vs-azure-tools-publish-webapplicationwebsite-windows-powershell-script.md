---
title: "Publish-WebApplicationWebSite（Windows PowerShell 脚本）| Azure"
description: "了解如何将 Web 项目发布到 Azure 网站。 此脚本将在 Azure 订阅中创建所需的资源（如果这些资源不存在）。"
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
origin.date: 11/11/2016
ms.date: 03/30/2017
ms.author: v-junlch
ms.openlocfilehash: fd9e195febd7f4bff51c6e9dc9da4d88935754f9
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# Publish-WebApplicationWebSite（Windows PowerShell 脚本）
<a id="publish-webapplicationwebsite-windows-powershell-script" class="xliff"></a>
## 语法
<a id="syntax" class="xliff"></a>
将 Web 项目发布到 Azure 网站。 如果资源不存在，脚本会在 Azure 订阅中创建所需的资源。

```
Publish-WebApplicationWebSite
-Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

## 配置
<a id="configuration" class="xliff"></a>
描述部署的详细信息的 JSON 配置文件的路径。

| 参数 | 默认值 |
| --- | --- |
| 别名 |无 |
| 必需？ |true |
| 位置 |名为 |
| 默认值 |无 |
| 接受管道输入？ |false |
| 接受通配符？ |false |

## SubscriptionName
<a id="subscriptionname" class="xliff"></a>
想要在其中创建网站的 Azure 订阅的名称。

| 参数 | 默认值 |
| --- | --- |
| 别名 |无 |
| 必需？ |false |
| 位置 |名为 |
| 默认值 |无 |
| 接受管道输入？ |false |
| 接受通配符？ |false |

## WebDeployPackage
<a id="webdeploypackage" class="xliff"></a>
要发布到网站的 Web 部署包的路径。 可以在 Visual Studio 中使用“发布 Web”向导来创建此包。 有关详细信息，请参阅 [Azure 云服务和 ASP.NET 入门](http://go.microsoft.com/fwlink/p/?LinkID=623089)。

| 参数 | 默认值 |
| --- | --- |
| 别名 |无 |
| 必需？ |false |
| 位置 |名为 |
| 默认值 |无 |
| 接受管道输入？ |false |
| 接受通配符？ |false |

## DatabaseServerPassword
<a id="databaseserverpassword" class="xliff"></a>
Azure 中的 SQL 数据库的用户名和密码。

| 参数 | 默认值 |
| --- | --- |
| 别名 |无 |
| 必需？ |false |
| 位置 |名为 |
| 默认值 |无 |
| 接受管道输入？ |false |
| 接受通配符？ |false |

## SendHostMessagesToOutput
<a id="sendhostmessagestooutput" class="xliff"></a>
如果为 true，则将消息从脚本打印到输出流。

| 参数 | 默认值 |
| --- | --- |
| 别名 |无 |
| 必需？ |false |
| 位置 |名为 |
| 默认值 |false |
| 接受管道输入？ |false |
| 接受通配符？ |false |

## 备注
<a id="remarks" class="xliff"></a>
有关如何使用脚本创建开发和测试环境的完整说明，请参阅[使用 Windows PowerShell 脚本发布到开发和测试环境](./vs-azure-tools-publishing-using-powershell-scripts.md)。

JSON 配置文件指定要部署的内容的详细信息。 它包括当你创建项目时指定的信息，如网站的名称和用户名。 它还包括要预配的数据库（如果有的话）。 以下代码显示一个示例 JSON 配置文件：

```
{
    "environmentSettings": {
        "webSite": {
            "name": "WebApplication10554",
            "location": "China North"
        },
        "databases": [
            {
                "connectionStringName": "DefaultConnection",
                "databaseName": "WebApplication10554_db",
                "serverName": "iss00brc88",
                "user": "sqluser2",
                "password": "",
                "edition": "",
                "size": "",
                "collation": "",
                "location": "China North"
            }
        ]
    }
}
```

你可以编辑 JSON 配置文件以更改部署的内容。 网站部分是必需的，但数据库部分则是可选的。

## 后续步骤
<a id="next-steps" class="xliff"></a>
有关详细信息，请参阅 [Publish-WebApplicationVM（Windows PowerShell 脚本）](./vs-azure-tools-publish-webapplicationvm.md)