---
title: 在 Azure 应用服务移动应用中创建 iOS 应用 | Azure
description: 按照本教程，开始使用 Azure 移动应用后端，以 Objective-C 或 Swift 语言开发 iOS
services: app-service\mobile
documentationcenter: ios
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 6461a899-9340-42dd-b118-ffc5ba00e846
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: conceptual
origin.date: 08/17/2018
ms.date: 06/17/2019
ms.author: v-biyu
ms.openlocfilehash: 2f7e57ee3b476c5c4bbb9b6a7ecc7c5d98de1a0f
ms.sourcegitcommit: d7db02d1b62c7b4deebd5989be97326b4425d1d3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687466"
---
# <a name="create-an-ios-app"></a>创建 iOS 应用

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

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