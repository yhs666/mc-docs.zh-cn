---
title: 在 Azure 通知中心配置百度云推送 | Azure Docs
description: 了解如何为 Azure 通知中心配置百度设置。
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
origin.date: 03/25/2019
ms.date: 04/29/2019
ms.author: v-biyu
ms.openlocfilehash: 8e9691764ac49fe6b30d0c58ac497f4f7b27d5fa
ms.sourcegitcommit: f9d082d429c46cee3611a78682b2fc30e1220c87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2019
ms.locfileid: "59566420"
---
# <a name="configure-baidu-cloud-push-settings-for-a-notification-hub-in-the-azure-portal"></a>在 Azure 门户中为通知中心配置百度云推送设置
本文介绍如何使用 Azure 门户为 Azure 通知中心配置百度云推送设置。 

## <a name="prerequisites"></a>先决条件
如果你尚未创建通知中心，现在请创建一个。 有关详细信息，请参阅[在 Azure 门户中创建 Azure 通知中心](create-notification-hub-portal.md)。 

## <a name="configure-baidu-cloud-push"></a>配置百度云推送
以下过程提供的步骤演示了如何为通知中心配置百度云推送设置：

1. 在 Azure 门户的“通知中心”页上，在左侧菜单中选择“Baidu (Android China)”。 
2. 在百度云推送项目中，输入从百度控制台获取的“API 密钥”。 
3. 在百度云推送项目中，输入从百度控制台获取的“机密密钥”。 
4. 选择“其他安全性验证” 。 

    ![通知中心的屏幕截图，其中显示了百度 (Android China) 的推送通知配置](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

## <a name="next-steps"></a>后续步骤
如需通过教程的分步说明来了解如何使用 Azure 通知中心和百度云推送将通知推送到百度，请参阅[使用百度的通知中心入门](notification-hubs-baidu-china-android-notifications-get-started.md)。
