---
title: 使用 CLI 2.0 创建 Azure 媒体服务帐户 | Azure
description: 按照本快速入门的步骤，创建 Azure 媒体服务帐户。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
origin.date: 03/27/2018
ms.date: 05/28/2018
ms.author: v-nany
ms.openlocfilehash: f10ba62dd068eb483e27d538cd5937ed0ad5dd13
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475119"
---
# <a name="create-an-azure-media-services-account"></a>创建 Azure 媒体服务帐户

若要开始加密、编码、分析、管理和流式处理 Azure 中的媒体内容，需要创建媒体服务帐户。 创建媒体服务帐户时，还将在此帐户所在的地理区域内创建一个关联的存储帐户（或使用现有存储帐户）。

本主题介绍使用 CLI 2.0 创建新 Azure 媒体服务帐户的步骤。  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>登录 Azure

登录 [Azure 门户](http://portal.azure.cn)并启动 **CloudShell** 以执行 CLI 命令。



如果选择在本地安装并使用 CLI，本主题要求使用 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可确定你拥有的版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0]( /cli/azure/install-azure-cli)。 

## <a name="set-the-azure-subscription"></a>设置 Azure 订阅

在以下命令中，为媒体服务帐户提供想要使用的 Azure 订阅 ID。 导航到[订阅](https://portal.azure.cn/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)即可查看有权访问的订阅列表。

```azurecli-interactive
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
