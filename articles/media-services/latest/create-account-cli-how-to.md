---
title: 使用 Azure CLI 创建媒体服务帐户 - Azure | Microsoft Docs
description: 按照本快速入门的步骤，创建 Azure 媒体服务帐户。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
origin.date: 05/19/2019
ms.date: 09/23/2019
ms.author: v-jay
ms.openlocfilehash: 3fe431de1b428310da69b832305bbb551d1d82a8
ms.sourcegitcommit: 8248259e4c3947aa0658ad6c28f54988a8aeebf8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124691"
---
# <a name="create-an-azure-media-services-account"></a>创建 Azure 媒体服务帐户

若要开始加密、编码、分析、管理和流式处理 Azure 中的媒体内容，需要创建媒体服务帐户。 媒体服务帐户需与一个或多个存储帐户相关联。

> [!NOTE]
> 媒体服务帐户和所有关联的存储帐户必须位于同一 Azure 订阅中。 强烈建议在媒体服务帐户所在的位置使用存储帐户，避免额外的延迟和数据出口成本。

本文介绍使用 Azure CLI 创建新 Azure 媒体服务帐户的步骤。  

## <a name="prerequisites"></a>先决条件

一个有效的 Azure 订阅。 如果没有 Azure 订阅，可在开始前创建一个 [1 元人民币试用帐户](https://www.azure.cn/pricing/1rmb-trial-full/?form-type=identityauth)。

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

## <a name="set-the-azure-subscription"></a>设置 Azure 订阅

在以下命令中，为媒体服务帐户提供想要使用的 Azure 订阅 ID。 导航到[订阅](https://portal.azure.cn/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)即可查看有权访问的订阅列表。

```azurecli
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>后续步骤

* [访问 v3 API](access-api-cli-how-to.md)
* [流式传输文件](stream-files-dotnet-quickstart.md)
* [将辅助存储附加到媒体服务帐户](/cli/ams/account/storage?view=azure-cli-latest#az-ams-account-storage-add)

## <a name="see-also"></a>另请参阅

[Azure CLI](/cli/ams?view=azure-cli-latest)

