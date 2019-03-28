---
title: 在 Azure API 管理中管理协议和密码 | Microsoft Docs
description: 了解如何在 Azure API 管理中管理协议（TLS、SSL）和密码 (DES)。
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/26/2019
ms.date: 04/01/2019
ms.author: v-yiso
ms.openlocfilehash: 8b6e828130e26b757bc78e089eeb3c1758ab4f78
ms.sourcegitcommit: 41a1c699c77a9643db56c5acd84d0758143c8c2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58348674"
---
# <a name="manage-protocols-and-ciphers-in-azure-api-management"></a>在 Azure API 管理中管理协议和密码

Azure API 管理支持同时用于客户端和后端的多个 TLS 协议版本以及 3DES 密码。

本指南介绍如何管理 Azure API 管理实例的协议和密码配置。

![在 APIM 中管理协议和密码](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers.png)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="prerequisites"></a>先决条件

若要执行本文中的步骤，必须提供：

* API 管理实例

## <a name="how-to-manage-tls-protocols-and-3des-cipher"></a>如何管理 TLS 协议和 3DES 密码

1. 在 Azure 门户中导航到自己的 **API 管理实例**。
2. 从菜单中选择“协议设置”。  
3. 启用或禁用所需的协议或密码。
4. 单击“保存” 。 更改将在一小时内应用。  

## <a name="next-steps"></a>后续步骤

* 详细了解 [TLS (传输层安全性)](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls)。
