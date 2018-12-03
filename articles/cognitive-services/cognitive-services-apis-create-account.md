---
title: 在 Azure 门户中创建认知服务 API 帐户
titlesuffix: Azure Cognitive Services
description: 如何在 Azure 门户中创建 Microsoft 认知服务 API 帐户。
services: cognitive-services
author: garyericson
manager: cgronlun
ms.service: cognitive-services
ms.topic: conceptual
origin.date: 02/01/2018
ms.date: 10/25/2018
ms.author: v-junlch
ms.openlocfilehash: ef1a0fcce3ad81386566cce5a31497eb89c0c496
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52645619"
---
# <a name="quickstart-create-a-cognitive-services-account-in-the-azure-portal"></a>快速入门：在 Azure 门户中创建认知服务帐户

使用此快速入门开始使用 Azure 认知服务。 这些服务由 Azure [资源](/azure-resource-manager/resource-group-portal)表示，允许连接到一个或多个可用的认知服务 API。

## <a name="prerequisites"></a>先决条件

- 有效的 Azure 订阅。 可以免费[创建一个帐户](https://www.azure.cn/pricing/1rmb-trial/)。

## <a name="create-and-subscribe-to-an-azure-cognitive-services-resource"></a>创建并订阅 Azure 认知服务资源

1. 登录 [Azure 门户](http://portal.azure.cn)，然后单击“创建资源”。
    
    ![选择认知服务 API](./media/cognitive-services-apis-create-account/azurePortalScreen.png)

2. 在“Azure 市场”下，选择“数据 + 分析”。 如果没有看到感兴趣的服务，请单击“查看全部”，以查看认知服务 API 的完整目录。

    ![选择认知服务 API](./media/cognitive-services-apis-create-account/azureMarketplace.png)

3. 在“创建”页中提供以下信息： 

    |    |    |
    |--|--|
    | **名称** | 认知服务资源的描述性名称。 建议使用描述性的名称，例如“MyNameFaceAPIAccount”。 |
    | **订阅** | 选择一个可用的 Azure 订阅。 |
    | **API 类型** | 要创建的 API。 |
    | **定价层** | 认知服务帐户的费用取决于你所选的选项和你的使用情况。 有关详细信息，请参阅 API [定价详细信息](https://www.azure.cn/pricing/details/cognitive-services/)。
    | **资源组** | 将包含认知服务资源的 Azure 资源组。 可以创建新组或将其添加到预先存在的组。 |

    ![“创建资源”屏幕](./media/cognitive-services-apis-create-account/resource_create_screen.png)

## <a name="access-your-resource"></a>访问资源 

> [!NOTE]
> 订阅所有者可以通过应用 Azure 策略，分配“不允许的资源类型”策略定义并指定“Microsoft.CognitiveServices/accounts”作为目标资源类型来禁止为资源组和订阅创建认知服务帐户。

创建资源后，如果已固定该资源，则可以从 Azure 仪表板对其进行访问。 否则，可以在“资源组”中查找该资源。

在认知服务资源中，可以使用“概述”部分中的终结点 URL 和密钥在应用程序中进行 API 调用。

![“资源”屏幕](./media/cognitive-services-apis-create-account/resourceScreen.png)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [计算机视觉 API C# 教程](/cognitive-services/computer-vision/tutorials/csharptutorial)

## <a name="see-also"></a>另请参阅

- [快速入门：从图像中提取手写文本](/cognitive-services/computer-vision/quickstarts/csharp-hand-text)
- [教程：创建一个用于检测和定格图像中人脸的应用](/cognitive-services/Face/Tutorials/FaceAPIinCSharpTutorial)
