---
title: 容器支持
titleSuffix: Azure Cognitive Services
description: 了解如何创建 Azure 容器实例资源。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/3/2019
ms.author: dapine
ms.openlocfilehash: 081cfb3b085920c42310a8352df1fbb0cea155da
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70103858"
---
## <a name="create-an-azure-container-instance-resource"></a>创建 Azure 容器实例资源

1. 转到容器实例的[创建](https://ms.portal.azure.cn/#create/Microsoft.ContainerInstances)页。

2. 在“基本信息”选项卡中输入以下详细信息： 

    |设置|Value|
    |--|--|
    |订阅|选择订阅。|
    |资源组|选择可用的资源组，或者创建一个新的，例如 `cognitive-services`。|
    |容器名称|输入一个名称，例如 `cognitive-container-instance`。 此名称必须小写。|
    |Location|选择要部署的区域。|
    |映像类型|`Public`|
    |映像名称|输入认知服务容器位置。 此位置可以与 `docker pull` 命令中使用的相同，请参阅[容器存储库和映像](../../cognitive-services-container-support.md#container-repositories-and-images)，获取可用的映像名称及其相应的存储库。|
    |OS 类型|`Linux`|
    |大小|对于特定的认知服务容器，请将大小更改为建议的设置：<br>2 个CPU 核心<br>4 GB

3. 在“网络”选项卡上，输入以下详细信息： 

    |设置|Value|
    |--|--|
    |端口|将 TCP 端口设置为 `5000`。 在端口 5000 上公开此容器。|

4. 在“高级”选项卡上，  输入所需的**环境变量**作为 ACI 资源的容器计费设置：

    | 键 | Value |
    |--|--|
    |`apikey`|从资源的“键”页复制。  它是一个由 32 个字母数字组成的字符串（不包含空格或短划线），即 `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`。|
    |`billing`|从资源的“概览”页复制。 |
    |`eula`|`accept`|

1. 单击“查看并创建” 
1. 验证通过后，单击“创建”，完成创建过程 
1. 成功部署资源以后，即可使用它