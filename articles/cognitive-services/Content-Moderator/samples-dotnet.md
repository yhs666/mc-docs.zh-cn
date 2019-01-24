---
title: 代码示例 - 内容审查器、.NET
description: 在应用程序中使用内容审查器
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: sample
origin.date: 01/10/2018
ms.date: 01/22/2019
ms.author: v-junlch
ms.openlocfilehash: 0c5d864779955de485e6057e932fee4524f58524
ms.sourcegitcommit: f248afb1039011d34579baed2980f0632061f5b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2019
ms.locfileid: "54858065"
---
# <a name="net-sdk-samples"></a>.NET SDK 示例

下面列出了使用用于 .NET 的 Azure 内容审查器 SDK 生成的代码示例的链接。

- **帮助程序库**：请参阅[快速入门](content-moderator-helper-quickstart-dotnet.md)。

## <a name="moderation"></a>审查

- **图像审查**：[评估图像中是否有成人和挑逗性、文本和人脸](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs)。 请参阅[快速入门](image-moderation-quickstart-dotnet.md)。
- **自定义图像**：[使用自定义图像列表进行审查](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs)。 请参阅[快速入门](image-lists-quickstart-dotnet.md)。

> [!NOTE]
> 最多只能使用 5 个图像列表，每个列表中的图像数不得超过 10,000 张。
>

- **文本审查**：[筛查文本中是否有猥亵内容和个人身份信息 (PII)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs)。 请参阅[快速入门](text-moderation-quickstart-dotnet.md)。
- **自定义术语**：[使用自定义术语列表进行审查](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs)。 请参阅[快速入门](term-lists-quickstart-dotnet.md)。

> [!NOTE]
> 最多只能使用 5 个术语列表，每个列表中的术语数不得超过 10,000 个。
>

- **视频审查**：[扫描视频中是否有成人内容和挑逗性内容，并获取结果](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs)。 请参阅[快速入门](video-moderation-api.md)。

请参阅 [GitHub 上内容审查器 .NET 示例](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator)中的所有 .NET 示例。

