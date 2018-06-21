---
title: 使用 Azure 门户克隆 Web 应用
description: 了解如何使用 Azure 门户将 Web 应用克隆到新的 Web 应用。
services: app-service\web
documentationcenter: ''
author: ahmedelnably
manager: stefsch
editor: ''
ms.assetid: 20b0ae4e-67e8-4bae-9d74-8a24dc445cce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/08/2016
ms.date: 09/26/2016
ms.author: v-dazen
ms.openlocfilehash: a43698375c1f11d1b24b56f42bb2701d4b86afeb
ms.sourcegitcommit: 86616434c782424b2a592eed97fa89711a2a091c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
ms.locfileid: "20453118"
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a>使用 Azure 门户克隆 Azure App Service 应用
[Azure 应用服务 Web 应用](/app-service-web/app-service-changes-existing-services)中的克隆功能可以轻松将现有 Web 应用克隆到位于不同或相同区域中的新建应用。 这样，客户就可以快速轻松地跨不同区域部署许多应用。

应用克隆目前仅支持高级层应用服务计划。 新功能使用与 Web 应用备份功能相同的限制，具体请参阅[在 Azure App Service 中备份 Web 应用](web-sites-backup.md)。

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>克隆现有应用
必须在“高级”模式下运行 Web 应用才能为其创建副本。

1. 在 [Azure 门户](https://portal.azure.cn/)中，打开 Web 应用的边栏选项卡。
2. 单击“工具”。 然后，在“工具”边栏选项卡中，单击“克隆应用”。

    ![][1]

   > [!NOTE]
   > 如果 Web 应用未处于“高级”模式，则会提示消息，指出应用克隆支持的模式。 此时可以选择“升级”。
   > 
   > 
3. “克隆应用”边栏选项卡中提供了新 Web 应用、资源组和应用服务计划的名称。 用户也可以选择是否克隆多个源 Web 应用设置。

    ![][2]
4. 单击“创建”后，平台将开始创建源 Web 应用的副本。

## <a name="current-restrictions"></a>当前限制
此功能目前为预览版，我们正在不断地添加新功能。以下是有关当前 Azure 门户中应用克隆支持的已知限制：

* 不会克隆 Azure 流量管理器设置
* 不会克隆自动缩放设置
* 不会克隆备份计划设置
* 不会克隆 VNET 设置
* 不会自动在目标 Web 应用上设置 App Insights
* 不会克隆简易身份验证设置
* 不会克隆 Kudu 扩展
* 不会克隆 TiP 规则
* 不会克隆数据库内容

### <a name="references"></a>参考
* [使用 PowerShell 克隆 Web 应用](app-service-web-app-cloning.md)
* [在 Azure App Service 中备份 Web 应用](web-sites-backup.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png