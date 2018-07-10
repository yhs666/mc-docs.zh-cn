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
ms.date: 06/25/2018
ms.author: v-nany
ms.openlocfilehash: 9b51a06763c6f08cc1c6a1f8b71e8a30da498655
ms.sourcegitcommit: 18810626635f601f20550a0e3e494aa44a547f0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2018
ms.locfileid: "37405386"
---
# <a name="create-an-azure-media-services-account"></a>创建 Azure 媒体服务帐户

若要开始加密、编码、分析、管理和流式处理 Azure 中的媒体内容，需要创建媒体服务帐户。 创建媒体服务帐户时，还将在此帐户所在的地理区域内创建一个关联的存储帐户（或使用现有存储帐户）。

本主题介绍使用 CLI 2.0 创建新 Azure 媒体服务帐户的步骤。  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>登录 Azure

登录到 [Azure 门户](http://portal.azure.cn)。
如果选择在本地安装并使用 CLI，本主题要求使用 Azure CLI 2.0 版或更高版本。 运行 `az --version` 即可确定你拥有的版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0]( /cli/install-azure-cli)。 

## <a name="set-the-azure-subscription"></a>设置 Azure 订阅

在以下命令中，为媒体服务帐户提供想要使用的 Azure 订阅 ID。 导航到[订阅](https://portal.azure.cn/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)即可查看有权访问的订阅列表。

```cli
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [流式传输文件](stream-files-dotnet-quickstart.md)
