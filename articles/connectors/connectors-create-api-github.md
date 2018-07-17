---
title: 使用 Azure 逻辑应用连接到 GitHub
description: 使用 Azure 逻辑应用自动执行 GitHub 的工作流
services: logic-apps
documentationcenter: ''
author: ecfan
manager: anneta
editor: ''
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
origin.date: 03/02/2018
ms.author: v-yiso
ms.date: 04/23/2018
ms.openlocfilehash: b0f210f79d7c6c9afd1afaf868810156fbb32b64
ms.sourcegitcommit: 00c8a6a07e6b98a2b6f2f0e8ca4090853bb34b14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2018
ms.locfileid: "38939036"
---
# <a name="connect-to-github"></a>连接到 GitHub

GitHub 是基于 Web 的 Git 存储库托管服务，它提供 Git 的所有分布式修订控制和源代码管理 (SCM) 功能以及其他功能。

若要开始使用 GitHub 连接器，[首先创建逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。

## <a name="create-a-connection-to-github"></a>创建到 GitHub 的连接

若要在逻辑应用中使用 GitHub 连接器，必须先创建*连接*，然后提供以下属性的详细信息： 

| 属性 | 必须 | 说明 | 
| -------- | -------- | ----------- | 
| 令牌 | 是 | 提供 GitHub 凭据。 |

创建连接后，可执行操作并侦听触发器，如本文所述。

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>特定于连接器的详细信息

有关 Swagger 中定义的触发器和操作以及任何限制，请查看[连接器详细信息](/connectors/github/)。

## <a name="find-more-connectors"></a>查找更多连接器

* 查看[连接器列表](apis-list.md)。