---
title: 在 Azure 应用服务移动应用中创建 iOS 应用 | Azure
description: 按照本教程，开始使用 Azure 移动应用后端，以 Objective-C 或 Swift 语言开发 iOS
services: app-service\mobile
documentationcenter: ios
author: elamalani
manager: crdun
editor: ''
ms.assetid: 6461a899-9340-42dd-b118-ffc5ba00e846
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: conceptual
origin.date: 06/25/2019
ms.date: 09/10/2019
ms.author: v-tawe
ms.openlocfilehash: f9a37c302a5d90b2197ab3918574efdfd34d6529
ms.sourcegitcommit: 32d62e27e59e42c8d21a667e77b61b8d87efbc19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2019
ms.locfileid: "71006582"
---
# <a name="create-an-ios-app"></a>创建 iOS 应用

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center 正在投资于对移动应用开发至关重要的新集成服务。 开发人员可以使用**生成**、**测试**和**分发**服务来设置持续集成和交付管道。 部署应用后，开发人员可以使用**分析**和**诊断**服务监视其应用的状态和使用情况，并使用**推送**服务与用户互动。 开发人员还可以利用 **Auth** 对用户进行身份验证，利用**数据**服务在云中持久保存和同步应用数据。 立即查看 [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-ios-get-started)。
>

## <a name="overview"></a>概述

本教程说明如何将云后端服务 [Azure 应用服务移动应用](app-service-mobile-value-prop.md)添加到 iOS 应用。 第一步是在 Azure 上创建一个新的移动后端。 然后，下载一个简单的“待办事项列表”  iOS 示例应用以在 Azure 中存储数据。

若要完成本教程，需要一台 Mac 和 [一个 Azure 帐户](https://www.azure.cn/pricing/1rmb-trial/)

## <a name="create-a-new-azure-mobile-app-backend"></a>创建新的 Azure 移动应用后端

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>创建数据库连接并配置客户端和服务器项目
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-ios-app"></a>运行 iOS 应用

[!INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.cn/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203