---
title: Azure CLI 脚本示例 - 创建自定义主题 | Microsoft Docs
description: Azure CLI 脚本示例 - 创建自定义主题
services: event-grid
documentationcenter: na
author: tfitzmac
ms.service: event-grid
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2018
ms.author: v-yiso
ms.openlocfilehash: 14730c22fced0469c46d2c80d1358320447df637
ms.sourcegitcommit: 5a57f99d978b78c1986c251724b1b04178c12d8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66195332"
---
# <a name="create-event-grid-custom-topic-with-azure-cli"></a>使用 Azure CLI 创建事件网格自定义主题

此脚本创建事件网格自定义主题。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

<!--[!code-azurecli[main](../../../cli_scripts/event-grid/create-custom-topic/create-custom-topic.sh "Create custom topic")]-->

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建自定义主题。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az eventgrid topic create](/cli/eventgrid/topic#az-eventgrid-topic-create) | 创建事件网格自定义主题。 |


## <a name="next-steps"></a>后续步骤

* 有关查询订阅的信息，请参阅[查询事件网格订阅](../query-event-subscriptions.md)。
* 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure)。
