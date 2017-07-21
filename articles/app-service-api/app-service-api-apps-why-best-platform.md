---
title: "API 应用简介 |Azure"
description: "了解 Azure App Service 如何帮助开发、托管和使用 RESTful API。"
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 60049a16-8159-47aa-a34b-110be0d8dab6
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 08/23/2016
ms.date: 11/25/2016
ms.author: v-dazen
ms.openlocfilehash: 190c513df8adba4e36cbc4f0777e9805ab95224c
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="api-apps-overview"></a>API 应用概述

[!INCLUDE [azure-sdk-developer-differences](../../includes/azure-sdk-developer-differences.md)]

Azure App Service 中的 API 应用提供各种功能，使用这些功能可以在云和本地轻松地开发、托管和使用 API。 使用 API 应用可实现企业级安全性、便捷的访问控制和 SDK 的自动生成。

[Azure App Service](../app-service/app-service-value-prop-what-is.md) 是完全托管的平台，适用于 Web、移动和集成方案。 API 应用是 [Azure App Service](../app-service/app-service-value-prop-what-is.md) 提供的三种应用类型之一。

![Azure App Service 中的应用类型](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>为何使用 API 应用？
下面是 API 应用的一些主要功能：

* 保留现有的 API - 无需更改现有 API 的任何代码就能使用 API 应用，只需将代码部署到 API 应用即可。 API 可以使用应用服务支持的任何语言或框架，包括 ASP.NET 和 C#、Java、PHP、Node.js 和 Python。
* 易于使用 - 集成了对 [Swagger API 元数据](http://swagger.io/) 的支持，各种客户端都能轻松使用 API。  使用各种语言（包括 C#、Java 和 Javascript）为 API 自动生成客户端代码。 轻松配置 [CORS](app-service-api-cors-consume-javascript.md) 而无需更改代码。 有关详细信息，请参阅[用于 API 发现和代码生成的应用服务 API 应用元数据](app-service-api-metadata.md)和[借助 CORS 从 JavaScript 使用 API 应用](app-service-api-cors-consume-javascript.md)。 
* 简单的访问控制 - 可以保护 API 应用免受未经身份验证的访问，且无需更改代码。 其他服务或代表用户的客户端访问 API 时，内置的身份验证服务将提供保护。 支持的标识提供者中包括 Azure Active Directory 和 Microsoft 帐户。 客户端可以使用 Active Directory 身份验证库 (ADAL) 或移动应用 SDK。 有关详细信息，请参阅 [Azure App Service 中 API 应用的身份验证和授权](app-service-api-authentication.md)。
* Visual Studio 集成 - Visual Studio 中的专用工具可以简化创建、部署、使用、调试和管理 API 应用的工作。 有关详细信息，请参阅[宣布推出用于 .NET 的 Azure SDK 2.8.1](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/)。

此外，API 应用可以利用 [Web 应用](../app-service-web/app-service-web-overview.md)和[移动应用](../app-service-mobile/app-service-mobile-value-prop.md)提供的功能。 反之亦然：如果使用 Web 应用或移动应用来托管 API，该应用可以利用 Swagger 元数据等 API 应用功能来生成客户端代码，以及使用 CORS 进行跨域浏览器访问。 这三个应用程序类型（API、Web、移动）之间的唯一差别在于 Azure 门户中使用的名称和图标。

## <a name="getting-started"></a>入门
若要通过将示例代码部署到其中一个应用以开始使用 API 应用，请参阅适用于所需框架的教程：

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

若要咨询有关 API 应用的问题，请在 [API 应用论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)中发贴。