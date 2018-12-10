---
title: include 文件
description: include 文件
services: functions
author: tdykstra
manager: cfowler
ms.service: functions
ms.topic: include
origin.date: 05/17/2018
ms.date: 05/30/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 804b5814b7470cac6a5d81e711ea094ec7be949f
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52647640"
---
### <a name="azure-storage-sdk-version-in-functions-1x"></a>Functions 1.x 中的 Azure 存储 SDK 版本

在 Functions 1.x 中，存储触发器和绑定使用 7.2.1 版的 Azure 存储 SDK（[WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) NuGet 包）。 如果引用另一版本的存储 SDK，而且在函数签名中绑定到某个存储 SDK 类型，则 Functions 运行时可能会报告它不能绑定到该类型。 此解决方案是为了确保项目引用 [WindowsAzure.Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1)。

<!-- ms.date: 05/30/2018 -->