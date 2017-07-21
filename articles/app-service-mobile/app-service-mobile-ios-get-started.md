---
title: "在 Azure 应用服务移动应用中创建 iOS 应用 | Azure"
description: "按照本教程，开始使用 Azure 移动应用后端，以 Objective-C 或 Swift 语言开发 iOS"
services: app-service\mobile
documentationCenter: ios
authors: yuaxu
manager: yochayk
editor: 
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: v-yiso
ms.openlocfilehash: 4655892e1b8fe1c6977f67b43ae3cb740b538767
ms.sourcegitcommit: 86616434c782424b2a592eed97fa89711a2a091c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
#<a name="create-an-ios-app"></a>创建 iOS 应用

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>概述

本教程说明如何将云后端服务 [Azure 移动应用](./app-service-mobile-value-prop.md)添加到 iOS 应用。 首先将创建新的移动后端。 然后，使用一个简单的待办事项列表 iOS 应用在 Azure 中存储数据。

若要完成本教程，需要一台 Mac 和 [一个 Azure 帐户](https://www.azure.cn/pricing/1rmb-trial/)

## <a name="step-i-create-a-new-azure-mobile-app-backend"></a>步骤 I：创建新的 Azure 移动应用后端

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="step-ii-configure-the-backend-project"></a>步骤 II：配置后端项目

[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="step-iii-download-and-run-the-ios-app"></a>步骤 III：下载并运行 iOS 应用

[!INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.cn/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203