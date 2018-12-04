---
title: 将 Web 应用资源迁移到另一个资源组
description: 介绍了可以将 Web 应用和应用服务从一个资源组迁移到另一个资源组的方案。
services: app-service
documentationcenter: ''
author: ZainRizvi
manager: erikre
editor: ''
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 12/21/2016
ms.date: 03/01/2017
ms.author: v-dazen
ms.openlocfilehash: 384461aadf19f353dbdcd92f3ed585beba1ebe3c
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52661224"
---
# <a name="supported-move-configurations"></a>受支持的迁移配置
可以使用 [Resource Manager 迁移资源 API](../azure-resource-manager/resource-group-move-resources.md) 迁移 Azure Web 应用资源。

Azure Web 应用目前支持以下迁移方案：

* 将资源组（Web 应用、应用服务计划和证书）的所有内容迁移到另一个资源组。 

   > [!Note]
   > 此方案中，目标资源组不能包含任何 Microsoft.Web 资源。

* 将一个 Web 应用迁移到其他资源组，同时仍然在其当前应用服务计划中托管该 Web 应用（旧资源组中仍然保留该应用服务计划）。