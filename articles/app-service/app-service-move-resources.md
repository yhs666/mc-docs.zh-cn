---
title: 将 Azure Web 应用移动到另一个资源组 | Microsoft Docs
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
origin.date: 10/19/2017
ms.date: 12/04/2017
ms.author: v-yiso
ms.openlocfilehash: 6733626e7170d1bff135af59646eb6abe96f1d1c
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52656212"
---
# <a name="move-an-azure-web-app-to-another-resource-group"></a>将 Azure Web 应用移动到另一个资源组
可以将 Web 应用和/或其相关资源移动到同一订阅中的另一个资源组，或者移动到不同订阅中的资源组。 这是作为 Azure 中的标准资源管理的一部分完成的。 有关详细信息，请参阅 [Move Azure resources to new subscription or resource group](../azure-resource-manager/resource-group-move-resources.md)（将 Azure 资源移动到新的订阅或资源组）。

## <a name="limitations-when-moving-within-the-same-subscription"></a>在同一订阅中移动时的限制

_在同一订阅中_移动 Web 应用时，无法移动已上传的 SSL 证书。 不过，可以将 Web 应用移动到新的资源组而不移动其已上传的 SSL 证书，并且，应用的 SSL 功能仍然可以工作。 

如果希望随 Web 应用移动 SSL 证书，请执行以下步骤：

1.  从 Web 应用中删除已上传的证书
2.  移动 Web 应用。
3.  将证书上传到移动后的 Web 应用。

## <a name="limitations-when-moving-across-subscriptions"></a>在订阅之间移动时的限制

_在订阅之间_移动 Web 应用时存在以下限制：

- 目标资源组中不能有任何现有的应用服务资源。 应用服务资源包括：
    - Web 应用
    - 应用服务计划
    - 上传或导入的 SSL 证书
- 资源组中的所有应用服务资源必须一起移动。
- 只能从最初创建应用服务资源的资源组中移动它们。 如果某个应用服务资源不再位于其原始资源组中，则必须首先将其移动回该原始资源组，然后才能将其在订阅之间移动。 
