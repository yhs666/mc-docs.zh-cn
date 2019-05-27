---
title: Azure CLI 脚本示例 - 订阅资源组 | Microsoft Docs
description: Azure CLI 脚本示例 - 订阅资源组
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
ms.openlocfilehash: 41f7299a2fe3b7d4fd6abe9186aed2d47eb88fd5
ms.sourcegitcommit: 5a57f99d978b78c1986c251724b1b04178c12d8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66195330"
---
# <a name="subscribe-to-events-for-a-resource-group-with-azure-cli"></a>使用 Azure CLI 订阅资源组的事件

此脚本创建资源组事件的事件网格订阅。 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

预览示例脚本需要事件网格扩展。 若要安装，请运行 `az extension add --name eventgrid`。

## <a name="sample-script---stable"></a>示例脚本 - 稳定版

<!--[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-resource-group/subscribe-to-resource-group.sh "Subscribe to resource group")]-->

## <a name="sample-script---preview-extension"></a>示例脚本 - 预览扩展

<!--[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-resource-group-preview/subscribe-to-resource-group-preview.sh "Subscribe to resource group")]-->

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建事件订阅。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az eventgrid event-subscription create](/cli/eventgrid/event-subscription#az-eventgrid-event-subscription-create) | 创建事件网格订阅。 |
| [az eventgrid event-subscription create](/cli/ext/eventgrid/eventgrid/event-subscription#ext-eventgrid-az-eventgrid-event-subscription-create) - 扩展版本 | 创建事件网格订阅。 |

## <a name="next-steps"></a>后续步骤

* 有关查询订阅的信息，请参阅[查询事件网格订阅](../query-event-subscriptions.md)。
* 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure)。
